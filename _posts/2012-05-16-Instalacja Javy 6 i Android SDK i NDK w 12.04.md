---
title: Instalacja Javy 6 i Android SDK/NDK w 12.04
date: 2012-05-16 09:12:54 +0100
categories: [android]
tags: [linux,ubuntu,android,android sdk,java,gist]
image:
  path: /assets/img/android-sdk.png
  alt: android sdk
author: michal_cwiklinski
toc: false
---

# Instalacja Javy 6 i Android SDK/NDK (skrypty) w 12.04

Odkąd Canonical "wypięło się" na użytkowników Ubuntu chcących zainstalować sobie ostatnią "Sun'owską" Javę na najnowszej wersji Ubuntu (potrafili nawet wyłączyć Launchpadowe PPA z paczkami `sun-java-6`), trzeba było albo instalować i konfigurować je samodzielnie, albo zainstalować przez udostępnione skrypty najnowszą Javę, te Oracle'ową.

Na szczęście są zapaleńcy, którzy potrafili zaradzić temu w bardzo prosty sposób: użytkownik _flexion_ spreparował skrypt, który tworzy lokalne repozytorium z paczkami `sun-java6`. Skrypt dostępny jest [tutaj](https://github.com/flexiondotorg/oab-java6/blob/master/oab-java6.sh).

Kolejny bardzo fajny i przydatny skrypt to [gist](https://gist.github.com/2016019) do instalacji SDK i NDK dla Androida. Wystarczy odpalić skrypt i wybrać odpowiednie opcje.