---
title: WhatsApp Notifications
date: 2025-07-28 12:38:54 +0100
categories: [smarthome]
tags: [smarthome,whatsapp,bot,notifications,multimedia]
image:
  path: /assets/img/whatsapp.jpg
  alt: whatsapp
author: michal_cwiklinski
toc: false
---

WhatsApp automation is a powerful tool, but finding the right solution can be complex. In my previous post, I described integration with WAHA, but I found it limited without additional costs for functionality extensions. I needed other solution, pretty simple allowing me to use it as a bridge for notifications - text first of all, but also some media (images, and videos potentially).

This guide will walk you through creating your own lightweight, containerized WhatsApp API using `whatsapp-web.js` and Docker. Our focus will be on building a single, powerful endpoint to send a text message and an image in one go, using a clear and direct API call.

## Ready-to-Use Docker Appliance

The `chrishubert/whatsapp-web-api` image is a complete, pre-packaged application that turns whatsapp-web.js into a full-featured REST API. You don't need to write any JavaScript or manage a Dockerfile. You simply run the image and start making API calls.

### Setup with Docker Compose
The easiest way to run the service and persist your session data is with docker-compose.

Create a file named docker-compose.yml.

Paste the following content into it:

```yaml
# docker-compose.yml
services:
  whatsapp-api:
    container_name: whatsapp_web_api
    image: chrishubert/whatsapp-web-api:latest
    restart: unless-stopped
    environment:
      # - API_KEY=your-api-key
      # - BASE_WEBHOOK_URL=url-to-callback
      # - ENABLE_LOCAL_CALLBACK_EXAMPLE=FALSE # OPTIONAL, NOT RECOMMENDED FOR PRODUCTION
      - MAX_ATTACHMENT_SIZE=5000000 # IN BYTES
      - SET_MESSAGES_AS_SEEN=TRUE # WILL MARK THE MESSAGES AS READ AUTOMATICALLY
      # ALL CALLBACKS: auth_failure|authenticated|call|change_state|disconnected|group_join|group_leave|group_update|loading_screen|media_uploaded|message|message_ack|message_create|message_reaction|message_revoke_everyone|qr|ready|contact_changed
      - DISABLED_CALLBACKS=message_ack|message_reaction  # PREVENT SENDING CERTAIN TYPES OF CALLBACKS BACK TO THE WEBHOOK
      - ENABLE_SWAGGER_ENDPOINT=TRUE # OPTIONAL, ENABLES THE /api-docs ENDPOINT
      # - RATE_LIMIT_MAX=1000 # OPTIONAL, THE MAXIUM NUMBER OF CONNECTIONS TO ALLOW PER TIME FRAME
      # - RATE_LIMIT_WINDOW_MS=1000 # OPTIONAL, TIME FRAME FOR WHICH REQUESTS ARE CHECKED IN MS
      # - WEB_VERSION='2.2328.5' # OPTIONAL, THE VERSION OF WHATSAPP WEB TO USE
      # - WEB_VERSION_CACHE_TYPE=none # OPTIONAL, DETERMINES WHERE TO GET THE WHATSAPP WEB VERSION(local, remote or none), DEFAULT 'none'
      - RECOVER_SESSIONS=TRUE # OPTIONAL, SHOULD WE RECOVER THE SESSION IN CASE OF PAGE FAILURES
    volumes:
      - ./sessions:/usr/src/app/sessions
```

Now, you can run your appliance:

```bash
# Build and run the container in the background
docker-compose up -d
```

## WhatsApp session setup

There is few additional steps you need to do to start session in WhatsApp. You'll need working logged-in instance of WhatsApp on your phone, to connect it with docker image via scanning QR code. But one step after another:
### Step 1: Start a New Session
Open your browser and navigate to this URL to start a session named "my-session".

`http://localhost:3000/session/start?name=my-session`

### Step 2: Get the QR Code Image
Once the session is starting, get the QR code by visiting this URL. You may need to refresh after a few seconds if it's not ready immediately.

`http://localhost:3000/session/qr/my-session/image`

Scan this image with your phone to link the device.

### Step 3: Check the Session Status
After scanning, you can verify that the session is connected.

`http://localhost:3000/session/status/my-session`

If connecting: It will return a pending status.

If connected: It will return `{"status":"ok","state":"CONNECTED"}`.

If the session doesn't exist: It will return a not_found status.

### Step 4: Terminate All Sessions (Optional)
To log out of all active sessions and clear the connections, visit this URL.

`http://localhost:3000/terminateAll`

After terminating, you'll need to restart the service (docker-compose restart) and start new sessions to get new QR codes.

## Sending a Text Message
For sending a text-only message, the process is even simpler. You use the /my-session/sendText endpoint.

Example curl command:

```bash
curl -X POST 'http://localhost:3000/client/sendMessage/my-session' \
    -H 'Content-Type: application/json' \
    -d '{
        "chatId": "YOUR_CHAT_ID@c.us",
        "contentType": "string",
        "content": "Hello World!"
    }'
```

## Send a Message with an Image
The key feature is sending multimedia. To send a message with an attached image, you make a POST request to the /my-session/sendMedia endpoint.

The API expects the image data to be a base64-encoded string.

Example curl command:

```bash
curl -X POST 'http://localhost:3000/client/sendMessage/my-session' \
    -H 'Content-Type: application/json' \
    -d '{
        "chatId": "YOUR_CHAT_ID@c.us",
        "contentType": "MessageMedia",
        "content": {
            "mimetype": "image/jpeg",
            "data": "iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAQAAAC1HAwCAAAAC0lEQVR42mNk+A8AAQUBAScY42YAAAAASUVORK5CYII=",
            "filename": "report.jpg"
        }
    }'
```

## Alternative image sending
There is another way/method to send image with text. Here's curl example:
```bash
curl -X POST 'http://localhost:3000/client/sendMessage/my-session' \
    -H 'Content-Type: application/json' \
    -d '{
        "chatId": "YOUR_CHAT_ID@c.us",
        "contentType": "string",
        "content": "Here is your document!",
        "options": {
            "media": {
                "data": "data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAUAAAAFCAYAAACNbyblAAAAHElEQVQI12P4//8/w38GIAXDIBKE0DHxgljNBAAO9TXL0Y4OHwAAAABJRU5ErkJggg==",
                "filename": "report.png"
            }
        }
    }'
```

Bon apetit!!!
