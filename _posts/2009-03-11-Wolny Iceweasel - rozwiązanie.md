---
title: Wolny Iceweasel (Firefox) - rozwiązanie
date: 2009-03-11 09:12:54 +0100
categories: [linux]
tags: [linux,ubuntu,iceweasel,firefox,tweaking]
image:
  path: /assets/img/iceweasel.jpg
  alt: wolny iceweasel
author: michal_cwiklinski
toc: false
---

# Wolny Iceweasel (Firefox) - rozwiązanie

Korzystając z Iceweasela (Firefoxa) w Debianie (Ubuntu) można było zauważyć bardzo duże zużycie procesora podczas przewijania stron i odtwarzania filmów Flash'owych, i nie tylko. Można to ogólnie przypisać wolnemu działaniu aplikacji GTK.

Próby "tuningowania" przeglądarki (w stylu `about:config`, nowy profil, itp) nic nie dały. Dopiero po "Google'owaniu" znalazł się dobry sposób. 
Otóż, ze sprawą można poradzić sobie wykorzystując metody `AccelMethod` w konfiguracji serwera X'ów systemu. Wg autorów związanego z tym i zgłoszonego  buga problem wynika z automatycznej konfiguracji serwera X w większości systemów opartych na Debianie/Ubuntu. 
Wg autorów sterownik i810 nie rozpoznaje automatycznie, że powinna zostać użyta metoda `XAA`.  A najważniejszy jest fakt, że drastycznie wzrasta wydajność aplikacji GTK.

```
Section "Device"
    Identifier  "Configured Video Device"
    Option "AccelMethod" "XAA"
EndSection
```