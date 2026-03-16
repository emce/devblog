---
title: 🇬🇧 Smart Home Display - From Photo Frame to Info Hub
date: 2026-02-14 15:06:10 +0100
categories: [smarthome]
tags: [smarthome,integration,home-assistant,display,ssh,self-hosted]
image:
  path: /assets/img/smart-home-display.png
  alt: Smart Home Display
author: michal_cwiklinski
toc: false
---
I transformed a [cheap touchscreen monitor from AliExpress](https://pl.aliexpress.com/item/1005006827629161.html) into an interactive smart home info system. Connected to a Raspberry Pi 4B running Raspberry Pi OS (Wayland), it displays photos from Immich (Immich Frame), Home Assistant notifications, and weather data.

### Full Setup
- HGFRTEE 16" portable touchscreen monitor (16:10, 96% sRGB, 350 cd/m², USB-C/HDMI) for ~400 PLN from AliExpress. Nothing fancy—but perfect for a photo frame.
- Raspberry Pi 4B: HDMI + USB-C cable (touch support), handles DDC/CI via ddcutil for brightness/power control.
- OS: Raspberry Pi OS with Wayland (`sudo raspi-config → Advanced → Wayland`).
- Extras: Immich for photo management (self-hosted), Immich Frame for display (self-hosted).

## Home Assistant Integration
Config in `configuration.yaml` (HA on separate host, SSH to Pi "display"):

{% raw %}
```yaml
shell_command:
  display_reboot: ssh -F /config/ssh/config display 'sudo /sbin/shutdown -r now'
  display_shutdown: ssh -F /config/ssh/config display 'sudo /sbin/shutdown -h now'
  display_screen_off: ssh -F /config/ssh/config display 'ddcutil setvcp D6 04'
  display_screen_on: ssh -F /config/ssh/config display 'ddcutil setvcp D6 01'
  display_brightness: ssh -F /config/ssh/config display 'ddcutil setvcp 10 {{ brightness }}'
  display_notify_message: ssh -F /config/ssh/config display 'DBUS_SESSION_BUS_ADDRESS=unix:path=/run/user/1000/bus notify-send -u {{ mode }} -i "{{ icon }}" "{{ title | replace("\\"", "\\\\\\"") }}" "{{ message | replace("\\"", "\\\\\\"") }}"'
  clear_notifications: ssh -F /config/ssh/config display 'dunstctl close-all'
```
{% endraw %}
## HA Scripts
Key automation scripts:

{% raw %}
```yaml
script:
  display_screen_on:
    sequence:
      - service: shell_command.display_screen_on
      - service: input_boolean.turn_on
        target: input_boolean.display_screen
  display_show_message:
    sequence:
      - if: "{{ is_state('binary_sensor.display_screen_power', 'off') }}"
        then: script.display_screen_on
      - service: shell_command.clear_notifications
      - service: shell_command.display_notify_message
        data:
          title: "{{ title | default('Home Assistant') }}"
          message: "{{ message }}"
          timeout: "{{ states('input_number.czas_notyfikacji') | int(150) }}"

```
{% endraw %}

## Real-World Automations
- Photo frame: Automation turns on screen at 8:00 AM, displays Immich Frame in surf browser.
- Notifications: script.display_show_message with title="Weather", message="Rain 80%", icon="weather-rainy"—queues notifications, clears previous ones.
- Power saving: binary_sensor → adjust config to your hardware setup.

## `ddcutil` & Autostart Install
On Raspberry Pi:

```shell
sudo apt update && sudo apt install ddcutil dunst swayidle
echo 'pi ALL=(ALL) NOPASSWD: /usr/bin/ddcutil' | sudo tee -a /etc/sudoers.d/ddcutil
```

This setup saves power (turns off screen), integrates Immich (Frame) and Home Assistant—scalable for homelab. Running stably for months!
