---
title: Tailscale for Home Assistant
date: 2025-07-21 11:02:02 +0100
categories: [smarthome]
tags: [smarthome,vpn,tailscale,home-assistant,interface]
image:
  path: /assets/img/tailscale.png
  alt: tailscale
author: michal_cwiklinski
toc: false
---

Home Assistant is the ultimate platform for local home automation, giving you unparalleled control over your devices and data. But what if you want to check your lights, adjust your thermostat, or monitor your security cameras when you're away from home? Opening up ports on your router can be a security headache, and VPNs can be complex to set up.

---

Tailscale builds a secure, point-to-point mesh network using the open-source WireGuard protocol. It's incredibly easy to set up, highly secure, and perfect for accessing your Home Assistant instance remotely without exposing your network to the internet. Think of it as your own private, encrypted highway to your Home Assistant.

This guide focuses on Home Assistant OS and includes the powerful **Tailscale Funnel** feature, which allows you to access Home Assistant even from devices that don't have Tailscale installed!

## What You'll Need:

* A running Home Assistant OS instance.
* An active Tailscale account (the free tier is usually more than enough for personal use).
* Internet access for both your Home Assistant device and the device you'll be using to connect remotely.
* **A unique Tailscale machine name** for your Home Assistant instance (this is usually set automatically, but you'll use it for Funnel).

## Step 1: Install the Tailscale Add-on on Home Assistant OS

The easiest and recommended way to integrate Tailscale with Home Assistant OS is by using the official add-on.

1.  **Open your Home Assistant interface.**
2.  Navigate to **Settings > Add-ons**.
3.  Click on the **Add-on Store** button in the bottom right corner.
4.  In the search bar, type "Tailscale" and select the official add-on.
5.  Click the **Install** button and wait for the installation process to complete.
6.  Once installed, go to the **Configuration** tab of the Tailscale add-on. For most users, the default settings will be sufficient.
7.  Go to the **Info** tab and click the **Start** button to launch the add-on.
8.  Switch to the **Logs** tab of the Tailscale add-on. You should see a unique URL that begins with `https://login.tailscale.com/a/`. **Copy this entire URL.**

## Step 2: Authenticate Your Home Assistant OS Device with Tailscale

This step links your Home Assistant instance to your Tailscale account.

1.  **Open a web browser** (on any device – your computer, phone, etc.) and paste the URL you copied from the add-on logs.
2.  You will be redirected to the Tailscale website. If prompted, **log in to your Tailscale account**.
3.  You'll be asked to authorize the new device. Give it a descriptive name (e.g., "Home Assistant Base," "HA Server," etc.) and click **Connect**.

## Step 3: Find Your Home Assistant's Tailscale IP Address (for VPN Access)

Now that your Home Assistant is connected to your Tailscale network, you need its unique Tailscale IP.

1.  Go to your **Tailscale admin console** in your web browser: [https://login.tailscale.com/admin/machines](https://login.tailscale.com/admin/machines).
2.  You should see your Home Assistant device listed among your other Tailscale-connected machines. Note down its **Tailscale IP address** (it will always start with `100.x.x.x`) and its **Machine Name**. You'll need both later.

## Step 4: Access Home Assistant via Tailscale VPN (Optional, but Good to Know)

This method requires Tailscale to be running on your client device.

1.  **Install the Tailscale client** on the device you wish to use for remote access (e.g., your smartphone, laptop, another computer). Tailscale offers clients for nearly all platforms: Windows, macOS, Linux, Android, iOS, ChromeOS, and more.
2.  **Authenticate this device** with your Tailscale account, just as you did for your Home Assistant OS instance.
3.  **Ensure Tailscale is running and connected** on your remote device. You'll typically see a Tailscale icon in your system tray, menu bar, or notification area, indicating it's active.
4.  **Open your web browser** or the Home Assistant mobile app.
5.  Instead of using your local IP address (e.g., `http://192.168.1.100:8123` or `http://homeassistant.local:8123`), use the **Tailscale IP address** you noted in Step 3, followed by the Home Assistant port (which is `8123` by default).

    For example: `http://100.x.x.x.x:8123`

    *Important Note for Home Assistant Mobile App:* When using the mobile app, you will need to add a new connection manually. In the app settings, look for "Add Connection" or "Manage Servers" and input the Tailscale IP address and port as a new server.

## Step 5: Configure Tailscale Funnel for Public Access (No VPN Required!)

This is where Tailscale Funnel comes in. It allows you to expose Home Assistant via a unique public URL, meaning anyone with that URL can access it, *without* needing to install or connect to Tailscale themselves.

**Security Warning:** This exposes your Home Assistant to the broader internet. Ensure strong authentication (username/password, MFA) is enabled on Home Assistant.

1.  **Enable Tailscale Funnel in the Add-on Configuration:**
    * In Home Assistant, go to **Settings > Add-ons**.
    * Click on the **Tailscale add-on**.
    * Go to the **Configuration** tab.
    * Find the option related to `funnel` or `serve_config` and set it to enable Funnel. The exact syntax might vary slightly with add-on updates, but you're looking to tell Tailscale to serve Home Assistant on port 8123.
    * A common way to do this for Home Assistant OS is to add `serve_config` to the add-on configuration, like this:

        ```yaml
        serve_config:
          '/:8123': {}
        ```
        This tells Tailscale to expose port 8123 (Home Assistant's default) to the public internet via Funnel.
    * Click **Save** and then **Restart** the Tailscale add-on from the **Info** tab.

2.  **Authorize Funnel in Tailscale Admin Console:**
    * Go back to your **Tailscale admin console** ([https://login.tailscale.com/admin/machines](https://login.tailscale.com/admin/machines)).
    * Find your Home Assistant OS device.
    * Click the three dots (`...`) next to it and select **Edit route settings**.
    * Under "Funnel", you should see an option to enable Funnel or "Turn on Funnel". Make sure this is activated for your Home Assistant machine.
    * Tailscale will automatically generate an HTTPS certificate for your Funnel URL.

3.  **Get Your Funnel URL:**
    * After enabling Funnel for the device in the admin console, your Home Assistant OS device will have a public Funnel URL. This URL is typically in the format: `https://<your-machine-name>.<your-tailnet-name>.ts.net:8123`.
    * You can find this exact URL on the **Machines** page in your Tailscale admin console, next to your Home Assistant device's entry, once Funnel is active.
    * Alternatively, you can check the **Logs** tab of the Tailscale add-on in Home Assistant after restarting it, and it might display the Funnel URL.

## Step 6: Access Home Assistant via Funnel URL

1.  On any device (even one without Tailscale installed), open a web browser.
2.  Enter the **Tailscale Funnel URL** you obtained in Step 5.
    For example: `https://your-ha-machine-name.your-tailnet-name.ts.net:8123`
3.  You should now be able to access your Home Assistant login page!

## Step 7 (Optional but Recommended): Enable Subnet Routing for Local Network Access

If you want to access other devices on your Home Assistant's local network (e.g., a network-attached storage device or another smart home hub) through the same Tailscale VPN connection (not Funnel), you can enable subnet routing.

1.  **In your Tailscale admin console**, go to the **Machines** tab ([https://login.tailscale.com/admin/machines](https://login.tailscale.com/admin/machines)).
2.  Find your Home Assistant OS device and click the three dots (`...`) next to it.
3.  Select **Edit route settings**.
4.  In the "Subnet routes" field, enter the **subnet of your local network**. This is typically `192.168.X.0/24` (replace `X` with the third octet of your Home Assistant's local IP address, e.g., `192.168.1.0/24`).
5.  Ensure the "Advertise routes" toggle is enabled.
6.  Click **Save**.

Now, when you're connected to Tailscale on your remote device (using the VPN, not the Funnel URL), you should be able to access other devices on your Home Assistant's local network by using their local IP addresses.

## You're Done!

You've now successfully configured Tailscale to access your Home Assistant OS instance securely from anywhere in the world! Whether you prefer the enhanced security of the VPN connection or the convenience of Tailscale Funnel for direct public access, you have powerful options for managing your smart home on the go. Remember to always prioritize security when exposing services to the internet.