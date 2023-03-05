---
title: Genymotion, ADB i problemy
date: 2014-10-28 09:12:54 +0100
categories: [android]
tags: [android,genymotion,adb,android sdk]
image:
  path: /assets/img/genymotion-adb.jpg
  alt: genymotion
author: michal_cwiklinski
toc: false
---

# Genymotion, ADB i problemy

Po jednej z ostatnich aktualizacji zauważyłem ,że bardzo często występuje błąd:
```bash
adb server is out of date. killing... 
cannot bind 'tcp:5037' ADB server didn't ACK 
* failed to start daemon *
```

Po researchu okazało się, że Genymotion używa swojego własnego ADB, które nie do końca jest kompatybilne z najnowszym ADB z Android SDK. Ale na szczęście można to zmienić - w ustawieniach Genymotion jest zakładka ADB, gdzie można zdefiniować ścieżkę do Android SDK, i od tej pory Genymotion będzie korzystała z najnowszej, kompatybilnej wersji...