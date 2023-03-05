---
title: X11vnc i dostęp do systemu przed zalogowaniem
date: 2012-11-12 09:12:54 +0100
categories: [linux]
tags: [ubuntu,x11vnc,remote desktop,vnc]
image:
  path: /assets/img/x11vnc.png
  alt: x11vnc
author: michal_cwiklinski
toc: false
---

# X11vnc i dostęp do systemu przed zalogowaniem

W zestawie pakietów leżących na serwerach dla Ubuntu i pochodnych istnieje parę dość przyzwoitych serwerów VNC, które ogólnie można podzielić na 2 grupy - pierwsza to te serwery, które pozwalają wyświetlić istniejące pulpit (tj. aktualną sesję), i druga - serwery, które tworzą oddzielną sesję istniejącą tylko w ramach serwera VNC. Potrzebny mi był przedstawiciel pierwszej części, bo chciałem zalogować się do X-ów zaraz po starcie systemu bez dostępu do urządzenia. Standardowym pakietem serwera VNC w Ubuntu jest Vino - to takie "idiotoodporne udostępnianie pulpitu", zatytułowane "Zezwalaj użytkownikom na zdalne sterowanie moim komputerem". Jednak należy on do drugiej grupy - serwerów "po zalogowaniu". Od Ubuntu 12.04 domyślnym menedżerem logowanie jest LightDM. I to na nim się skupiłem. Poniższy kod powinien działać też na Ubuntu 12.10 i wszystkich pochodnych.

1. Instalacja pakietu
```bash
sudo -i
apt-get install x11vnc
```
2. wygenerowanie globalnego hasła dostępu:
```bash
x11vnc -storepasswd /etc/x11vnc.pass
```
3. utworzenie upstrat job dla serwera:
```bash
mcedit /etc/init/x11vnc.conf
```
4. wklejamy kod:
```bash
start on login-session-start
script
/usr/bin/x11vnc -xkb -auth /var/run/lightdm/root/:0 -noxrecord -noxfixes -noxdamage -rfbauth /etc/x11vnc.pass -forever -bg -rfbport 5900 -o /var/log/x11vnc.log
end script
```

Reboot i gotowe.