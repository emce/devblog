---
title: Dockerized Tools - part 1
date: 2025-02-20 07:52:01 +0100
categories: [docker]
tags: [docker,self-hosted,home-server,owntone,nginx-proxy-manager,portainer,home-assistant,jellyfin,docker]
image:
  path: /assets/img/docker-apps.png
  alt: docker
author: michal_cwiklinski
toc: false
---

In the world of **self-hosting** and **home server management**, Docker has become a powerful solution for running various applications efficiently. I wanted to share some of the tools I'm using on daily basis ti make my life easier, more organized and much more pleasent.

---

## **1. OwnTone – Self-Hosted Music Server with Docker**
**OwnTone** is a powerful **DAAP (Digital Audio Access Protocol) server** that enables you to stream music via **AirPlay** to connected devices. It’s an ideal choice for self-hosted music streaming within your network.

### **Key Features**  
✅ **Supports AirPlay, MPD, and DAAP** for multi-room audio streaming.  
✅ **Integrates with Spotify, MPD clients, and web interfaces.**  
✅ **Works with Apple devices** and other compatible audio players.  

### **How to Run OwnTone in Docker**  
Use the following `docker-compose.yml` file:

```yaml
services:
  owntone:
    container_name: owntone
    image: lscr.io/linuxserver/daapd:latest
    restart: unless-stopped
    network_mode: host
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Europe/Warsaw
    volumes:
      - ./config:/config
      - ./music:/music
      - ./cache:/cache
```
This will start the server, and you can access the web interface at `http://your-server-ip:3689`.

#### My comment

I'm using it as my own radio device - I already added some local radio stations streams to configuration, set up Siri Shortcuts to make it work (using Owntone API), and thats it! I can listen to my favourite music via set of speakers (HomePod and JBL SoundBar so far).

---

## **2. Nginx Proxy Manager – User-Friendly Reverse Proxy**  
**Nginx Proxy Manager (NPM)** simplifies the **management of reverse proxies and SSL certificates** for self-hosted applications, making it easy to access services securely.

### **Key Features:**  
✅ Simple web UI to manage proxy hosts.  
✅ **SSL certificate management with Let’s Encrypt integration.**  
✅ Easily route web applications and subdomains.  

### **How to Deploy Nginx Proxy Manager with Docker**
To deploy **Nginx Proxy Manager**, use this **docker-compose.yml** file:

```yaml
services:
  proxy-manager:
    image: 'jc21/nginx-proxy-manager:latest'
    restart: always
    ports:
      - '80:80'
      - '81:81'
      - '443:443'
    volumes:
      - ./data:/data
      - ./letsencrypt:/etc/letsencrypt
```
Run `docker-compose up -d` to start the service.  
Then, access the web UI via `http://your-server-ip:81`.

#### My comment
I've set up my custom domain to redirect to this instance. Now, I'm using this tool to configure sudomains to support all the tools I'm using, to sepcific domains (like radio for OwnTone, proxy for this, etc).

---

## **3. Portainer – Simplified Docker Container Management**
Managing **Docker containers** via the command line can be complex. **Portainer** provides a **web-based UI** for managing Docker containers, images, networks, and volumes efficiently.

### **Key Features:**  
✅ **Graphical UI** to monitor and manage containers.  
✅ Supports **Docker Swarm, Kubernetes, and standalone Docker instances.**  
✅ **User access control** for security.  

### **How to Install Portainer in Docker**
Use the following `docker-compose.yml` file:

```yaml
services:
  portainer:
    container_name: portainer
    image: portainer/portainer-ce:latest
    restart: unless-stopped
    ports:
      - 9000:9000
      - 8000:8000
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
      - ./data:/data
```
Then, open `http://your-server-ip:9000` in your browser to complete the setup.

#### My comment
One of the easiest tools to manage docker applications.

---

## **4. Home Assistant – Smart Home Automation**
**Home Assistant** is a leading platform for **home automation**, allowing users to integrate various smart devices (lights, sensors, cameras) into a centralized control system.

### **Key Features:**  
✅ **Supports 1000+ smart home integrations** (Zigbee, MQTT, IoT).  
✅ **Automate routines** across connected devices.  
✅ **Privacy-first** system; runs locally without cloud dependencies.  

### **Install Home Assistant as a Docker Container**
Use the following `docker-compose.yml` file:

```yaml
services:
  homeassistant:
    container_name: homeassistant
    image: "homeassistant/home-assistant:latest"
    volumes:
      - /path/to/your/config:/config
    restart: always
    network_mode: host
```
Please remember to use `network_mode: host` to make all your smart devices to be discoverable by this application. Then, access Home Assistant at `http://your-server-ip:8123`.

#### My comment
As a huge fan of smart home and home automation, this is a dailt driver for me. I will share more information about this tools in another blog post.

---

## **5. Jellyfin – Open-Source Media Server**
If you're looking for an **alternative to Plex or Emby**, **Jellyfin** is a great choice. It’s a **free, open-source media server** that allows you to organize and stream your movies, TV shows, and music from any device.

### **Key Features:**  
✅ **No subscriptions** – 100% free and self-hosted.  
✅ **Supports transcoding and multiple clients** (smart TVs, mobile apps, web).  
✅ **Fast and efficient media playback.**  

### **Deploy Jellyfin in Docker**
Run the following `docker-compose.yml` file:

```yaml
services:
  jellyfin:
    container_name: jellyfin
    image: jellyfin/jellyfin
    ports:
      - 8096:8096
      - 8920:8920
    volumes:
      - /path/to/media:/media
      - /path/to/config:/config
    restart: unless-stopped
```
Once running, access Jellyfin at `http://your-server-ip:8096`.

#### My comment
Having NAS server or even external drive with photos or movies you want to have available (also only for yourself) at every time - JellyFin is here to help. 

---

## **Conclusion**
Using **Dockerized tools** like **OwnTone, Nginx Proxy Manager, Portainer, Home Assistant, and Jellyfin** allows you to **simplify service management, enhance automation, and improve media accessibility** in a reliable, scalable way. This "format" makes it really easy to manage thos tools, makes them really simple to be moved to new server, device, etc, also makes them "hardware" independent.