---
title: Ipla Lite pod LXDE
date: 2010-11-29 09:12:54 +0100
categories: [linux]
tags: [linux,ubuntu,lubuntu,lxde,ipla,streaming]
image:
  path: /assets/img/lubuntu.png
  alt: ipla lite lxde
author: michal_cwiklinski
toc: false
---

# Ipla Lite pod LXDE

Dla fanów siatkówki, którzy nie maja dostępu do platformy cyfrowej Polsat bądź programu Polsat Sport jest to jedyna alternatywa kibicowania polskim drużynom w rozgrywkach PlusLigi i Ligi Mistrzów. Ale (bo jest jedno ale) czytając wymagania techniczne dotyczące programu Ipla Lite na Linuksa, można znaleźć informacje:

Obsługiwane środowiska pulpitu: GNOME, KDE; Systemy zarządzania pakietami: RPM, Debian; Minimalna wersja biblioteki GTK+: 2.6; Menedżery okien: Metacity (domyślny dla środowiska GNOME), KWin (domyślny dla środowiska KDE); Przezroczystość: Obsługa efektu przezroczystości w aplikacjach AIR wymaga menedżera okien z opcją składania oraz dodatkowych rozszerzeń serwera X.

Jak można przeczytać, obsługiwane są tylko dwa środowiska: Gnome i KDE.

ROZWIĄZANIE:

1. Instalacja gnome-keyring (chyba, że już w systemie jest)
```bash
apt-get install gnome-keyring
```
2. utworzenie skryptu startowego dla Ipla (nazwa dowolna, np. startipla):
```bash
#!/bin/bash
export GNOME_DESKTOP_SESSION_ID=1
/opt/iplalite/bin/iplalite
```
3. i zapisanie go najlepiej w katalogu binarnym Ipli `/opt/ipla/bin`
4. zmiana ścieżki pliku wykonywalnego w aktywatorze `/usr/share/applications/ipla(...).desktop`

Works for me...