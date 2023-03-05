---
title: Dropbox i PCManFM w LXDE
date: 2011-05-09 09:12:54 +0100
categories: [linux]
tags: [linux,ubuntu,lubuntu,lxde,openbox,pcmanfm,dropbox]
image:
  path: /assets/img/pcmanfm.png
  alt: pcmanfm
author: michal_cwiklinski
toc: false
---

#Dropbox i PCManFM w LXDE

Problem z domyślnym otwieraniem przeglądarki zamiast menedżera plików PCManFM w środowisku LXDE po kliknięciu ikonki w zasobniku można rozwiązać w prosty sposób:
```bash
sudo leafpad /usr/bin/xdg-open
```

Edytujemy plik, zastępując tekst:
```c
generic)
open_generic "$url"
;;
```
nowym:
```c
generic)
pcmanfm "$url"
;;
```