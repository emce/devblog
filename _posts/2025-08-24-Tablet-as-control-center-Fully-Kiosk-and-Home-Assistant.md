---
title: 🇬🇧 Tablet as control center - Fully Kiosk and Home Assistant
date: 2025-08-24 08:36:14 +0100
categories: [smarthome]
tags: [smarthome,tablet,control center,dashboard,home assistant]
image:
  path: /assets/img/fully-kiosk-ha.jpg
  alt: fully_kiosk
author: michal_cwiklinski
toc: false
---

# Turn an Old Tablet into a Home Assistant Dashboard with Fully Kiosk

Do you want to create an intuitive and always-on control panel for your smart home? Instead of buying expensive, dedicated devices, you can repurpose an old Android tablet. Thanks to the **Fully Kiosk Browser** app and its integration with **Home Assistant**, you can build a fully functional and aesthetically pleasing system that's always at your fingertips.

### Step 1: Install Fully Kiosk Browser

The first step is to download and install the Fully Kiosk Browser app on your tablet. You can do this directly from the **Google Play** store. Search for "Fully Kiosk Browser" and install the app.

The app has a free version, but for full functionality, such as remote access and sensor control, a paid license is required. It's definitely worth purchasing to unlock the full potential of the program.

### Step 2: Basic Fully Kiosk Configuration

After installing and launching the app, you'll see the startup screen. Start by configuring the most important options:

1.  **Set the Start URL**: In Fully Kiosk's settings (tap the hamburger icon in the top left corner), go to **Web Content Settings** and enter the URL of your Home Assistant dashboard in the **Start URL** field, for example: `http://127.0.0.1:8123/lovelace/home`.
2.  **Fullscreen Mode**: For a professional-looking panel, enable fullscreen mode to hide Android's top status bar and bottom navigation buttons. You'll find this option under **Kiosk Mode & Security**.
3.  **Screen On/Off Management**: Fully Kiosk can manage the tablet's screen. In the **Device Management** settings, you can configure the screen to turn off automatically after a period of inactivity (e.g., 10 minutes) and to wake up when the motion sensor (if your tablet has one) detects a person nearby.

### Step 3: Integration with Home Assistant

For Fully Kiosk to communicate with Home Assistant, you need to install the integration in HA.

1.  In **Home Assistant**, go to **Settings > Devices & Services > Add Integration**.
2.  Search for **"Fully Kiosk Browser"**.
3.  During configuration, you will be asked for the **tablet's IP address** and a **remote administration password**, which you'll set in Fully Kiosk.
    * In the **Fully Kiosk app**, go to **Remote Administration (ADMIN) > Enable Remote Administration**. Set a password and note your tablet's IP address.
4.  Upon successful connection, Home Assistant will detect the tablet as a new device, and a new entity will appear in your panel, such as `media_player.tablet_name`, along with sensor entities like `sensor.tablet_name_battery_level`.

### Step 4: Using Automations

After integrating with HA, you can create automations that will take your panel to the next level.

* **Wake the Screen**: Create an automation that wakes the screen when a motion sensor (e.g., from Home Assistant, Zigbee, or Z-Wave) detects movement in the room.
    * **Trigger**: `when motion sensor detects motion`
    * **Action**: `call service fully_kiosk_browser.screen_on`
* **Change the View**: You can automatically switch the view on the panel. For example, when you arm your alarm, Home Assistant can switch the view to a security camera feed.
    * **Trigger**: `when alarm is turned on`
    * **Action**: `call service fully_kiosk_browser.load_url`, providing the URL of the camera view.

By using these features, your panel becomes more than just a static display; it becomes an interactive command center that **reacts to what's happening in your home**.

Installing and configuring Fully Kiosk Browser is a simple yet incredibly effective way to create a professional smart home dashboard.