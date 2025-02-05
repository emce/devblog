---
title: Moje launchpad'owe source'y
date: 2010-05-31 09:12:54 +0100
categories: [linux]
tags: [linux,ubuntu,sources.list,launchpad]
image:
  path: /assets/img/ubuntu.jpg
  alt: launchpad
author: michal_cwiklinski
toc: false
---

# Moje launchpad'owe source'y

Jak zapewne wszem i wobec wiadomo, niecierpliwcy tacy jak ja nie zawsze chcą czekać na wprowadzenie do oficjalnych repozytoriów Ubuntu najnowszych wersji pakietów poszczególnych programów, więc co robią? - szukają repozytoriów ich autorów, i dodają do swoich source'ów. Oczywiście największą "wylęgarnią" takich repozytoriów jest "Canoniczny" Launchpad.

Toteż niniejszym przedstawiam owoc kilkumiesięcznego zbierania różnych source'ów - jeżeli macie również ciekawe repa - DZIELCIE SIĘ!!!

```bash
## Conky - opisywać chyba nie trzeba - statystyki na Pulpicie
add-apt-repository ppa:norsetto/ppa
## Ubuntu-Tweak - najlepszy chyba tweaker dla Ubuntu
add-apt-repository ppa:tualatrix
## Gnome Media Player - zapowiada się niezły, lajtowy odtwarzacz, ale jeszcze trochę mu brakuje
add-apt-repository ppa:gnome-media-player-development/development
## X-updates - update'y do X'ów
add-apt-repository ppa:ubuntu-x-swat/x-updates
## GIMP - testowa wersja najnowszego gimpa (domyślnie 2.8, obecnie 2.7.1)
add-apt-repository ppa:matthaeus123/mrw-gimp-svn
## Liferea - RSS reader
add-apt-repository ppa:liferea/liferea-unstable
## Webkit - biblioteki dla przeglądarek i innych aplikacji internetowych opartych na tym silniku renderowania
add-apt-repository ppa:webkit-team/ppa
## Midori - super lajtowa przeglądarka (wg mnie alternatywa dla Chrome'a)
add-apt-repository ppa:midori/ppa
## Filezilla,Docky - klient FTP i pasek
add-apt-repository ppa:ricotz/testing
## Transmission BT - niezły klient BT
add-apt-repository ppa:transmissionbt/ppa
## Ailurus - drugi po UT tweaker ubuntowy
add-apt-repository ppa:ailurus/ppa
## Firefox - najnowsze stabilne wydania Firefoxa (by Mozilla Team)
add-apt-repository ppa:mozillateam/firefox-stable
## Lubuntu - metapakiet dla środowiska LXDE/OpenBox
add-apt-repository ppa:lubuntu-desktop/ppa
## Elementary - świetne tematy pulpitu i ikonki
add-apt-repository ppa:elementaryart/ppa
## Compiz - tego też chyba nie trzeba opisywać - wybajerzony menedżer okien
add-apt-repository ppa:compiz/ppa
## Claws-Mail najszybszy i bardzo prosty w obsłudze klient pocztowy - polecam
add-apt-repository ppa:claws-mail/ppa
## Free-NX - alternatywa dla VNC - świetne, choć kiepsko aktualizowane
add-apt-repository ppa:freenx-team/ppa
## KeepassX - menedżer haseł i tajnych informacji
add-apt-repository ppa:keepassx/ppa
## Pinta - odpowiednik Paint.NET
add-apt-repository ppa:moonlight-team/pinta
## WebUpd8 repozytoriów świetnego serwisu o Ubuntu, zawiera m.in. najnowsze wersje VLC, i paru innych aplikacji
add-apt-repository ppa:nilarimogard/webupd8
## Terminator - bardzo dobra alternatywa dla standardowego terminalu
add-apt-repository ppa:gnome-terminator/ppa
## MESA drivers - najnowsze sterowniki MESA z poprawkami dla kart Intela
add-apt-repository ppa:cavedon/ppa
## AWN - najlepszy w chwili obecnej pasek
add-apt-repository ppa:awn-testing/ppa
## Geany - lajtowy edytor dla programisty do robienia "na szybko"
add-apt-repository ppa:geany-dev/ppa
## Psi-plus - poprawiony i dobajerzony klient Jabber
add-apt-repository ppa:zerkalica/psi-plus
## GSQL - rozwijająca się ale coraz fajniejsza pod względem funkcjonalności aplikacja do zarządzania bazami danych (MySQL, PostreSQL,Oracle)
add-apt-repository ppa:add-apt-repository ppa:halturin/gsql
## DeaDBeeF - alternatywny odtwarzacz muzyki
add-apt-repository ppa:alexey-smirnov/deadbeef &&
## Nautilus Elementary - poprawiony nautilus
add-apt-repository ppa:am-monkeyd/nautilus-elementary-ppa
```