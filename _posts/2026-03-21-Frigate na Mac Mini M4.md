---
title: 🇵🇱 Frigate na Mac Mini M4
date: 2026-03-21 08:43:27 +0100
categories: [self-hosted]
tags: [docker,self-hosted,home-server,frigate,macos,m series]
image:
  path: /assets/img/frigate-mac-mini.png
  alt: image
author: michal_cwiklinski
toc: false
---

**Frigate** to system NVR (Network Video Recorder) - z otwartymi źródłami - zaprojektowany do **detekcji obiektów w czasie rzeczywistym**. W przeciwieństwie do tradycyjnych systemów CCTV, które nagrywają wszystko lub polegają na prostym wykrywaniu ruchu, Frigate wykorzystuje AI do:

* wykrywania ludzi, samochodów i innych obiektów
* redukcji fałszywych alarmów (drzewa, cienie, deszcz)
* nagrywania tylko istotnych zdarzeń
* integracji z systemami automatyki domowej

Początkowo Frigate był zoptymalizowany pod systemy Linux z akceleratorami sprzętowymi, takimi jak GPU lub dedykowane układy AI. Jednak wraz z pojawieniem się Apple Silicon (M1–M4) pojawiła się nowa możliwość:

**Wykorzystanie Apple Neural Engine poprzez Apple Silicon Detector**

Dzięki temu uruchamianie Frigate na macOS jest nie tylko możliwe — ale również zaskakująco wydajne.

---

## Dlaczego Apple Silicon świetnie się sprawdzi

Układy Apple z serii M (w tym M4) oferują:

* szybki wielordzeniowy CPU
* wydajny GPU
* **Neural Engine (NPU)**

Sam Frigate nie może bezpośrednio korzystać z Neural Engine wewnątrz kontenera.
Zamiast tego łączy się z **detektorem działającym na hoście** poprzez ZeroMQ (ZMQ).

Daje to:

* szybkie wnioskowanie (znacznie szybsze niż CPU)
* niskie zużycie energii
* brak potrzeby dodatkowego sprzętu

---

## Architektura rozwiązania

```
Mac Mini (host)
   ├── Apple Silicon Detector (port 5555)
   └── Podman machine (VM)
        └── Frigate container
             └── połączenie przez ZMQ → host.containers.internal:5555
```

Podejście:
Frigate działa w kontenerze, ale **AI działa natywnie na macOS**.

---

## Wymagania

* Mac Mini M4 (lub dowolny Mac z Apple Silicon)
* Podman (`podman machine`)
* Python 3.11
* Kamery IP (np. Reolink)

---

## Instalacja Apple Silicon Detector

```bash
git clone https://github.com/frigate-nvr/apple-silicon-detector
cd apple-silicon-detector
make install
```

Można do tego użyć dedykowanej aplikacji, ale obecnie zgłoszony jest problem, gdzie aplikacja ta wymagająca instalacji Rosetty, dlatego zdecydowałem się na użycie wersji skryptowej.

---

## Uruchomienie detektora w tle

### Szybka metoda

```bash
nohup make run > detector.log 2>&1 &
```

---

### Zalecane: uruchomienie jako usługa macOS

Utwórz plik:

```bash
nano ~/Library/LaunchAgents/com.frigate.detector.plist
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN">
<plist version="1.0">
<dict>
  <key>Label</key>
  <string>com.frigate.detector</string>

  <key>WorkingDirectory</key>
  <string>/Users/YOUR_USER/Development/apple-silicon-detector</string>

  <key>ProgramArguments</key>
  <array>
    <string>/bin/zsh</string>
    <string>-lc</string>
    <string>make run</string>
  </array>

  <key>RunAtLoad</key>
  <true/>

  <key>KeepAlive</key>
  <true/>

  <key>StandardOutPath</key>
  <string>/tmp/frigate-detector.log</string>

  <key>StandardErrorPath</key>
  <string>/tmp/frigate-detector.err</string>
</dict>
</plist>
```

Następnie:

```bash
launchctl bootstrap gui/$(id -u) ~/Library/LaunchAgents/com.frigate.detector.plist
launchctl kickstart -k gui/$(id -u)/com.frigate.detector
```

---

## Weryfikacja działania detektora

```bash
lsof -i :5555
```

---

## Konfiguracja Podman Compose

```yaml
services:
  frigate:
    container_name: frigate
    image: ghcr.io/blakeblackshear/frigate:stable-standard-arm64
    restart: unless-stopped
    shm_size: "512m"

    volumes:
      - ./config:/config
      - ./media:/media/frigate

    ports:
      - "8971:8971"
      - "8554:8554"

    environment:
      TZ: Europe/Warsaw
      FRIGATE_RTSP_PASSWORD: "password"
```

---

## Konfiguracja Frigate

```yaml
detectors:
  apple-silicon:
    type: zmq
    endpoint: tcp://host.containers.internal:5555

model:
  model_type: yolo-generic
  width: 640
  height: 640
  input_tensor: nchw
  input_dtype: float
  path: /config/model_cache/yolo.onnx
  labelmap_path: /config/model_cache/coco-80.txt
```

---

## Pobranie modelu YOLO

```bash
mkdir -p ./config/model_cache
cd ./config/model_cache

curl -L -o yolo.onnx \
https://github.com/ultralytics/yolov5/releases/download/v7.0/yolov5n.onnx

curl -L -o coco-80.txt \
https://raw.githubusercontent.com/pjreddie/darknet/master/data/coco.names
```

---

## Przykład konfiguracji kamery

```yaml
cameras:
  reolink_ogrodek:
    ffmpeg:
      inputs:
        - path: rtsp://USER:PASS@IP:554/h264Preview_01_main
          roles: [record]
        - path: rtsp://USER:PASS@IP:554/h264Preview_01_sub
          roles: [detect]
    detect:
      width: 640
      height: 480
      fps: 2
```

---

## Nagrywanie tylko istotnych zdarzeń

```yaml
record:
  enabled: true
  alerts:
    retain:
      days: 1
      mode: active_objects
  detections:
    retain:
      days: 1
      mode: active_objects
  continuous:
    days: 0
```

Nagrania zapisywane są tylko wtedy, gdy wykryte zostaną **rzeczywiste obiekty**.

---

## Uruchomienie Frigate

```bash
podman-compose up -d
```

---

### Wydajność

Na Mac Mini M4:

* Apple detector = bardzo szybki
* niskie zużycie CPU
* bez problemu obsługuje wiele kamer

---

### Najczęstsze problemy

* Używaj `host.containers.internal`, nie `host.docker.internal`
* Upewnij się, że detektor działa przed uruchomieniem Frigate
* YOLO wymaga wejścia **640x640**
* Strumienie kamer powinny być w **H.264**, nie H.265

---

Zapraszam do komentowania i zgłąszania sugestii!
