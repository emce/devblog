---
title: Conky i interfejsy sieciowe
date: 2010-06-15 09:12:54 +0100
categories: [linux]
tags: [linux,ubuntu,desktop,conky,ethernet,eth]
image:
  path: /assets/img/conky.png
  alt: conky i interfejsy sieciowe
author: michal_cwiklinski
toc: false
---

# Conky i interfejsy sieciowe

Conky jest fajnym programem informacyjno - statystycznym dla pulpitów linuksowych. Wyświetla informacje o zasobach systemowych, wolnym miejscu, jak również o sprzęcie. Kilka minut szukania w Google'ach, i można dobrać sobie odpowiednią konfigurację. Ale czasami pojawiają się problemy z dostosowywaniem tej konfiguracji.

Korzystając z laptopa mam do dyspozycji dwa interfejsy sieciowe - _eth0_ i _wlan0_ (kabelkowy i bezprzewodowy). Domyślnie w konfiguracji Conky jest metoda _if_up_ dla interfejsów, ale nie działa ona do końca tak, jakbym tego chciał. Na moim pulpicie pokazują się oba interfejsy, pomimo tego, że tylko jeden jest połączony. Dzieje sie tak dlatego, że Conky nie ma zdefiniowanego ściślejszego walidowania stanu interfejsu. Służy do tego dodatkowa zmienna _if_up_strictness_, którą wystarczy ustawić na odpowiednia wartość, aby walidacja interfejsów była prawidłowa.