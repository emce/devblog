---
title: Problemy z Intel Centrino Wireless-N na nowych kernelach w Ubuntu
date: 2011-11-23 09:12:54 +0100
categories: [linux]
tags: [linux,ubuntu,lubuntu,intel,kernel]
image:
  path: /assets/img/lubuntu.png
  alt: intel centrino i kernel
author: michal_cwiklinski
toc: false
---

# Problemy z Intel Centrino Wireless-N na nowych kernelach w Ubuntu

Pracuję w chwili obecnej na laptopie Lenovo ThinkPad L412, który jako rozszerzenie WiFi posiada kartę  Intel Corporation Centrino Advanced-N 6200. I po zainstalowaniu Lubuntu i nowych wersji kernela z repo zaczęły się problemy z WiFi. Dłuuuugo google'owałem za rozwiązaniem, znalazłem kilka pośrednich, ale dwa okazały się działać prawidłowo.
Pierwszym rozwiązaniem jest wyłączenie trybu N dla sieci bezprzewodowych (bo to właśnie jest głównym problemem z modułem `iwlagn`). Wystarczy kilka poleceń (oczywiście z roota lub sudo):

```bash
modprobe -r iwlagn
touch /etc/modprobe.d/options.conf
echo "options iwlagn 11n_disable=1" >> /etc/modprobe.d/options.conf
modprobe iwlagn
```
Kod ten wyłącza całkiem tryb N dla modułu iwlagn (sterownika Inter Wireless Centrino). Drugi sposób to wykorzystanie starego ucode dla `iwlwifi`:
```bash
mv /lib/firmware/iwlwifi-1000-5.ucode /lib/firmware/iwlwifi-1000-5.ucode.backup
modprobe -r iwlagn
modprobe iwlagn
```

U mnie działa. Jak ktoś znalazł lepsze rozwiązanie - niech napiszę w komentarzu - chętnie skorzystam.