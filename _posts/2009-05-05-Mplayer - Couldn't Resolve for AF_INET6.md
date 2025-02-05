---
title: Mplayer - Couldn't Resolve for AF_INET6
date: 2009-05-05 09:12:54 +0100
categories: [linux]
tags: [linux,ubuntu,mplayer,streaming]
image:
  path: /assets/img/ubuntu.jpg
  alt: mplayer
author: michal_cwiklinski
toc: false
---

# Mplayer - Couldn't Resolve for AF_INET6

Tak - ten bład wyskakuje, gdy wpisuje się do _MPlayera_ jakiś adres internetowy do odtworzenia. Jak sobie poradzić?
Otwieramy plik
```bash
~/.mplayer/config
```
, a następnie dopisujemy linijkę:
```bash
prefer-ipv4 = yes
```

I po sprawie...