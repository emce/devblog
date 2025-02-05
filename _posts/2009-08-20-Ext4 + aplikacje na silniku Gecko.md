---
title: Ext4 + aplikacje na silniku Gecko (Mozilla, Liferea) = 100% CPU load + 100% HDD load
date: 2009-08-20 09:12:54 +0100
categories: [linux]
tags: [linux,ubuntu,ext4,mozilla,rss]
image:
  path: /assets/img/ubuntu.jpg
  alt: ext4 + aplikacje
author: michal_cwiklinski
toc: false
---

# Ext4 + aplikacje na silniku Gecko (Mozilla, Liferea) = 100% CPU load + 100% HDD load

Ext4 to nowy i całkiem obiecujący system plików. Ale prace nad nim cały czas trwają, więc trzeba na bieżąco śledzisz, co się dzieje, żeby wszystko śmigało. A czasami nie śmiga. Dzieje się tak z aplikacjami opartymi na silniku Gecko (czyli Mozilla Firefox, Mozilla Thunderbird, czytnik Liferea i inne). Otóż korzystanie z nich w sysytemie opartym o ext4 powoduje bardzo duże obciążenie procesora i/lub dysku twardego.

Problem ten jest związany z obsługą _sqlite_ i _bit-barrier_ w EXT4. Wyłączenie tej opcji przy montowaniu powoduje znaczny wzrost wydajności sqlite, jak i aplikacji z niego korzystających.
Przykładowy wpis w fstab:

```bash
[...]
UUID=d818ddf9-ff01-e21a-a67d-3ceab43a9e2b / ext4 noatime,barrier=0 0 1
UUID=0d339122-74e0-e0ea-805a-7879b1fa3172 /home ext4 noatime,barrier=0,data=writeback,nobh,commit=100 0 2
[...]
```