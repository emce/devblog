---
title: Home Assistant and WAHA
date: 2025-02-18 13:51:14 +0100
categories: [smarthome]
tags: [smarthome,whatsapp,api,notifications,integration]
image:
  path: /assets/img/whatsapp.jpg
  alt: whatsapp
author: michal_cwiklinski
toc: false
---

If you're using **Home Assistant** to automate your smart home, adding real-time **WhatsApp notifications** can greatly improve how you receive alerts and important updates. **WAHA (WhatsApp HTTP API)** allows seamless integration, enabling Home Assistant to send messages via WhatsApp for automations, security alerts, and system notifications.

In this blog post, I’ll explore Home Assistant integration with WAHA to get WhatsApp messages whenever a specific trigger occurs.

---

## What is WAHA?

**WAHA - WhatsApp HTTP API** is an open-source solution that allows communication with WhatsApp through an API. Unlike cloud-based services, WAHA runs **locally** and allows direct integration with various systems, including **Home Assistant**.

### Benefits of Using WAHA with Home Assistant:
- 🔹 **Instant notifications** via WhatsApp.
- 🔹 **Local hosting** – you have full control over your messages.
- 🔹 **Multiple numbers** – manage notifications across different accounts.
- 🔹 **Works with Home Assistant automations** to send alerts dynamically.
- 
But it's just a small part of appliances of WAHA... But that's a topic for another blog post.

---

## Setting Up WAHA with Home Assistant

### **Step 1: Install WAHA**
To use WAHA with Home Assistant, you first need to set it up. WAHA can be installed using **Docker**, ensuring easy deployment.

1. Run WAHA with the following **Docker command**:
   ```sh
   docker run -d --name waha -p 3000:3000 -v ./data:/app/data/tokens ghcr.io/tulir/whatsmeow-http-api
   ```
2. Open `http://localhost:3000` (or your server IP).
3. Scan the QR code using your **WhatsApp** account.

At this stage, your WAHA instance is ready to send and receive messages.

---

### **Step 2: Configure Home Assistant to Use WAHA**
Once WAHA is running, we need to configure Home Assistant to send messages through it.

1. Open your `configuration.yaml` file in Home Assistant and add the following **RESTful notification** configuration:
   ```yaml
   notify:
     - name: whatsapp_waha
       platform: rest
       resource: "http://<WAHA_SERVER_IP>:3000/sendMessage"
       method: POST
       data:
         jid: "<YOUR_PHONE_NUMBER_WITH_COUNTRY_PREFIX>"
         message: "Home Assistant notification!"
   ```

   - Replace `<WAHA_SERVER_IP>` with the actual IP of your **WAHA server**.
   - Update `YOUR_PHONE_NUMBER_WITH_COUNTRY_PREFIX` with the **phone number** (in international format) you want to send messages to (like 44324555555).

2. Restart Home Assistant to apply the configuration.

---

### **Step 3: Test WhatsApp Notifications**
To verify that the configuration works, trigger a test message in Home Assistant:

1. Go to **Developer Tools > Services**.
2. Select the `notify.whatsapp_waha` service.
3. Enter the following JSON in the **Service Data** field:
   ```json
   {
     "message": "Hello! This is a test message from Home Assistant."
   }
   ```
4. Click **Call Service**.

Your phone should receive a WhatsApp message from WAHA!

---

## **Using WhatsApp Notifications in Home Assistant Automations**
Now that the integration is working, let’s set up an **automation** to notify you via WhatsApp when an event occurs (e.g., motion detection, door opening, or temperature exceeding a threshold).

Example: Notify via WhatsApp when motion is detected:

```yaml
alias: "Motion Alert via WhatsApp"
trigger:
  - platform: state
    entity_id: binary_sensor.motion_sensor
    to: "on"
action:
  - service: notify.whatsapp_waha
    data:
      message: "🚨 Motion detected! Check your security cameras."
mode: single
```

This automation will **send a WhatsApp message** whenever motion is detected.

---

## **Expanding WAHA + Home Assistant Capabilities**
Once WhatsApp notifications are set up, you can extend their use with:
- **Smart Home Alerts**: Get WhatsApp alerts for security, temperature, or device statuses.
- **Reminders & Schedules**: Send daily or weekly reminders via WhatsApp.
- **Command Responses**: Develop automation to reply with real-time **sensor readings** or system status updates.

---

By integrating **WAHA** with **Home Assistant**, you can receive smart and instant notifications via **WhatsApp** for various smart home events. This self-hosted solution ensures **greater privacy** and reliability compared to cloud-based integrations.

**Try it out and enhance your smart home experience!**