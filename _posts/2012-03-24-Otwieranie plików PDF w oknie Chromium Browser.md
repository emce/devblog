---
title: Otwieranie plików PDF w oknie Chromium Browser
date: 2012-03-24 09:12:54 +0100
categories: [linux]
tags: [linux,ubuntu,chromium,pdf]
image:
  path: /assets/img/chromium.webp
  alt: pdf w chromium
author: michal_cwiklinski
toc: false
---

# Otwieranie plików PDF w oknie Chromium Browser

Szybka wskazówka: jak otwierać PDF'y w oknie przeglądarki? W Google Chrome sprawa jest prosta - Google ma swoją wtyczkę. 

A co z Chromium? Można skopiować wtyczkę z Google Chrome, ale ciężko trafić z wersją (niekoniecznie ta wtyczka będzie działać). W tej sytuacji można skorzystać z możliwości MozPlugger'a.

Jak to wygląda? Oto przepis:
```bash
apt-get install mozplugger acroread
mkdir ~/.mozplugger
cp /etc/mozpluggerrc ~/.mozplugger/mozpluggerrc
gedit ~/.mozplugger/mozpluggerrc
```
podmieniamy 82 linijkę: `define(ACROREAD, [repeat needs_xembed swallow(acroread)  fill : acroread -openInNewWindow /a "$fragment" "$file"])`, zapisujemy i restartujemy przeglądarkę.

U mnie działa :)