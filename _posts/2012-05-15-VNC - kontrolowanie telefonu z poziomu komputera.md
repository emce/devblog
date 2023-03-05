---
title: VNC - kontrolowanie telefonu z poziomu komputera
date: 2012-05-15 09:12:54 +0100
categories: [android]
tags: [adb,vnc,port forwarding,vncs,apk]
image:
  path: /assets/img/vnc.png
  alt: vnc
author: michal_cwiklinski
toc: false
---

# VNC - kontrolowanie telefonu z poziomu komputera

Długo szukałem aplikacji, która pozwoli zarządzać telefonem z poziomu komputera  - nie odwrotnie - gdyż tego rodzaju aplikacji jest na pęczki (nawet - albo przede wszystkim - w Google Play). Z pomocą przyszli zapaleńcy z forum XDA Developers - użytkownik _knoxbrder_ zamieścił skrypt oraz apk do obsługi tego skryptu, który pozwala na odpalenie serwera VNC na telefonie. Oczywiście ja wypróbowałem go na swoim HTC Sensation.

Linki do wspomnianych plików

Skrypt [vncs](https://www.dropbox.com/s/u7j32pqa344mkho/vncs)
Aplikacja do konfiguracji [vnclauncher.apk](https://www.dropbox.com/s/k19lcd6f1c9hq44/vnclauncher.apk)

Cała procedura uruchomienia serwera VNC na telefonie wygląda następująco:
1. Podłączamy telefon do komputera za pomocą kabla USB, i korzystając z ADB kopiujemy skrypt, oraz instalujemy aplikację kontrolną:
```bash
adb push vncs /data/local/vncs adb install vnclauncher.apk
```
2. Przekierowujemy porty dla serwera VNC, żeby działały na lokalnym hoście: 
```bash
adb forward tcp:5901 tcp:5901 adb forward tcp:5801 tcp:5801
```
3. Uruchamiamy aplikację _VNCSPrefs_, i zaznaczamy opcję `Start/Stop Notification` (pozwoli łatwiej zarządzać serwerem). Teraz można zarządzać serwerem z paska notyfikacji.
4. Startujemy serwer korzystając z kontrolki w pasku notyfikacji (domyślnie jest not running, klikamy `Start/Stop`).
Na komputerze uruchamiamy dowolnego klienta VNC, wpisując jako adres serwera VNC `localhost:5901`.

I do dzieła!