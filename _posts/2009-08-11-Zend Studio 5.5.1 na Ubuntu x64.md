---
title: Zend Studio 5.5.1 na Ubuntu x64
date: 2009-08-11 09:12:54 +0100
categories: [linux]
tags: [linux,php,zend studio,x64]
image:
  path: /assets/img/ubuntu.jpg
  alt: zend studio
author: michal_cwiklinski
toc: false
---

# Zend Studio 5.5.1 na Ubuntu x64

Jak wiadomo wszem i wobec, nie każdy lubi najnowsze Eclipsowe wydana Zend Studio. Ja też chyba wole zostać przy starym ZS 5.5.1, choć ma ono parę niedociągnięć. A jednym z nich jest brak wersji na wersję 64-bitową systemu Linux.

Ale oczywiście linux (czytaj: Debian/Ubuntu) szybko się rozwija, i wystarczy teraz doinstalować obsługę bibliotek 32-bitowych, i możemy się cieszyć ZS 5.5.1 w systemie 64-bitowym.

```bash
apt-get install libc6-i386 ia32-libs
```

Na systemach AMD64 może pojawić się problem z uruchomieniem systemu. Należy wtedy stworzyć sobie własny skrypt startowy:

```bash
#!/bin/bash
export AWT_TOOLKIT=MToolkit
export XLOCALELIBDIR=/usr/lib32/X11/locale
exec /usr/local/Zend/ZendStudio-5.5.1/bin/ZDE
```