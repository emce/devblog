---
title: Rapidshare Premium + wget
date: 2008-05-6 09:12:54 +0100
categories: [linux]
tags: [linux,ubuntu,wget,rapidshare]
image:
  path: /assets/img/ubuntu.jpg
  alt: rapidshare
author: michal_cwiklinski
toc: false
---

# Rapidshare Premium + wget

Po poprzednim wpisie dotyczącym prostego skrypciku do ściągania plików wykorzystując konto Premium na serwerach Rapidshare przedstawiam inny sposób - tym razem skorzystam z "ciasteczek".

Sprawa jest prosta:

1. Po pierwsze: zapisujemy ciasteczka z naszym loginem i hasłem do konta Premium - wykorzystujemy oczywiście wget'a:
```bash
wget --save-cookies ~/.rapidshare --no-check-certificate --post-data "login=LOGIN&password=PASSWORD" -O - https://ssl.rapidshare.com/cgi-bin/premiumzone.cgi > /dev/null
```

2. Następnie aby ściągać pliki wystarczy przy wywołaniu wgeta z plikiem wymusić wczytanie zapisanych ciasteczek:
```bash
wget -c --load-cookies ~/.rapidshare http://link_do_pliku
```
bądź skorzystać z ciasteczek Firefoxa z naszego profilu, np.:
```bash
wget -c --load-cookies ~/.mozilla/firefox/{tutaj nazwa profilu}.default/cookies.txt http://link_do_pliku
```

3. ... i do ściągania!!!