---
title: Drobne, acz przydatne...
date: 2009-02-23 09:12:54 +0100
categories: [linux]
tags: [linux,ubuntu,music,media player,virtualbox]
image:
  path: /assets/img/ubuntu.jpg
  alt: drobne, acz przydatne
author: michal_cwiklinski
toc: false
---

# Drobne, acz przydatne...

Kilka wskazówek, gdzieś znalezionych, gdzieś wyeksperymentowanych - do codziennego funkcjonowania ze środowiskiem Desktop'owym niezbędnych...

## Audacious - polskie literki w liście odtwarzania
W _Ustawieniach_**_ w zakładce _Lista odtwarzania_ w polu _Zastępcze kodowanie znaków_ wpisujemy `cp1250`.

## MPlayer - odtwarzanie z linii poleceń:
Odtwarzanie pliku dźwiękowego w konsoli: `mplayer /sciezka/plik.mp3`
Odtwarzanie wszystkich plików z bieżącego katalogu: `mplayer *`
Odtwarzanie filmów VCD: `mplayer vcd://1 -cdrom-device /dev/cdrom`
Odtwarzanie filmów DVD: `mplayer dvd://1 -cdrom-device /dev/cdrom`
Odtwarzanie strumieni internetowych: `mplayer http://www.strona.pl/filmik.avi`

## Podłączanie myszki bluetooth:
w `/etc/default/bluetooth` ustawiamy `HIDD_ENABLED=1`
w linii poleceń `/etc/init.d/bluetooth restart`
wyszukujemy urządzenie `hidd --show`
łączymy myszkę poleceniem `hidd --connect <adres MAC myszki>`
jeżeli chcemy połączyć myszkę na stałe, dodajemy do `/etc/default/bluetooth` wpis `HIDD_OPTIONS="--connect <adres MAC myszki>"`

## Pulpit z Debiana w VirtualBox'ie
Oczywiście przy prawidłowym skonfigurowaniu VirtualBox'a, jeżeli chcemy od razu zamontować nasz oryginalny Pulpit jako zasób sieciowy `Z:/` wystarczy dodać do Autostartu skrót: `net.exe use z: \vboxsvrDesktop`

## Konwersja plików *.bin do *.iso
Jeżeli nie korzystamy z Ubuntu, to w repozytoriach nie znajdziemy programu `bin2iso`. Ale jest alternatywa - `bchunk` - dostępna w repozytoriach.

## Wyłączenie sprawdzania sumy kontrolnej w Brasero
Zaoszczędza bardzo dużo czasu przy nagrywaniu: uruchamiamy `gconf-editor`, a następnie w kolejności `apps` > `brasero` > `config`, a następnie klikamy dwukrotnie na `plugins` i usuwamy wszystkie wtyczki związane z MD5.