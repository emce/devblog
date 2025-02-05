---
title: VirtualBox i kernel 2.6.38
date: 2011-02-22 09:12:54 +0100
categories: [linux]
tags: [linux,debain,kernel,virtualbox]
image:
  path: /assets/img/debian.jpg
  alt: virualbox i kernel
author: michal_cwiklinski
toc: false
---

# VirtualBox i kernel 2.6.38

Problem z kompilacją modułu VirtualBoxa występuje w kernelu 2.6.38 ze względu na fakt, że plik `autoconf.h` został przeniesiony z `linux/autoconf.h` do `generated/autoconf.h`. Wystarczy więc wkleić w konsoli poniższy kod:
```bash
sudo cd /usr/share/virtualbox/src/vboxhost && for file in `grep "autoconf.h" * -R| cut -f1 -d:`; do sed -i 's/^#(s*)include /#1include /g' $file; done
```
a następnie wygenerować moduł VirtualBoxa:
```bash
sudo /etc/init.d/vboxdrv setup
```