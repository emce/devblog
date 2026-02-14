---
title: 🇵🇱 Wyświetlacz Smart Home - Od ramki foto do centrum informacji
date: 2026-02-14 14:43:54 +0100
categories: [smarthome]
tags: [smarthome,integration,home assistant,display,ssh,self-hosted]
image:
  path: /assets/img/smart-home-display.png
  alt: Wyświetlacz Smart Home
author: michal_cwiklinski
toc: false
---
Przekształciłem [tani monitor dotykowy z AliExpress](https://pl.aliexpress.com/item/1005006827629161.html) w interaktywny system informacyjny dla Smart Home. Podpięty do Raspberry Pi 4B z Raspberry Pi OS (Wayland), wyświetla zdjęcia z Immich (Immich Frame), notyfikacje z Home Assistant i dane pogodowe. 


### Cały setup
- HGFRTEE 16" dotykowy przenośny monitor (16:10, 96% sRGB, 350 cd/m², USB-C/HDMI) za ~400 zł z AliExpress. Nothing fancy - ale na ramkę do zdjęć - wystarczający.
- Raspberry Pi 4B: kabel HDMI + USB-C (obsługa dotyku), obsługuje DDC/CI via ddcutil do kontroli jasności/włączania.
- OS: Raspberry Pi OS z Wayland (sudo raspi-config → Advanced → Wayland).
- Dodatki: Immich do zarządzania zdjęciami (self-hosted), i Immich Frame do ich wyświetlania (self-hosted)

## Integracja z Home Assistant
Konfiguracja w configuration.yaml (HA na osobnym hoście, SSH do Pi "display"):

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

## Skrypty HA
Kluczowe skrypty do automatyzacji:

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

## Automatyzacje w praktyce
- Ramka foto: Automatyzacja włącza ekran o 8:00, Immich Frame wyświetlany w przedlądarce `surf`.
- Powiadomienia: `script.display_show_message` z `title="Pogoda", message="Deszcz 80%", icon="weather-rainy"` – kolejkuje powiadomienie, czyści poprzednie.
- Oszczędzanie energii: `binary_sensor` → konfigurację trzeba dostosować do własnego setup'u sprzętowego.

## Instalacja `ddcutil` & autostart
Na Raspberry Pi:

```shell
sudo apt update && sudo apt install ddcutil dunst swayidle
echo 'pi ALL=(ALL) NOPASSWD: /usr/bin/ddcutil' | sudo tee -a /etc/sudoers.d/ddcutil
```

Ten setup oszczędza prąd (wyłącza ekran), integruje Immich (Frame) i Home Assistant – skalowalny dla homelab. Działa stabilnie od miesięcy!
