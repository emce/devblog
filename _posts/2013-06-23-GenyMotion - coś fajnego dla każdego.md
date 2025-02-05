---
title: GenyMotion - coś fajnego dla każdego (Android developera)
date: 2013-06-23 09:12:54 +0100
categories: [android]
tags: [android,genymotion,unit testing,emulator]
image:
  path: /assets/img/genymotion-adb.jpg
  alt: genymotion
author: michal_cwiklinski
toc: false
---

# GenyMotion - coś fajnego dla każdego (Android developera)

Każdy developer Androida w pewnym momencie boryka się z problemem testowania kodu,  który napisał. Oczywiście, rozwiązań jest parę: emulator (wolny, ale mamy do dyspozycji dużo opcji pozwalających na dogłębne testowania funkcjonalności z kodu), realne urządzenia (tylko nie każdy ma dostęp do wielu urządzeń o różnej konfiguracji), oraz maszyny wirtualne (mam na myśli najpopularniejszy VirtualBox z obrazami projektu Android-X86 - szybkie, ale ograniczone w opcjach testowania). Nie chcę się za bardzo rozpisywać, ale odkryłem jakiś czas temu projekt AndroVM. Jest to nakładka na VirtualBoxa, emulująca bardzo sprawnie OpenGL'a, co z kolei pozwala na korzystanie i testowanie większej ilości opcji. Projekt rozwijał się na tyle dynamicznie, że jego twórca został zatrudniony przez firmę GenyMobile, i tak powstał produkt pod nazwą GenyMotion. Na razie projekt jest jeszcze w fazie rozwoju, opublikowano dopiero pierwszą betę, ale już ta wersja testowa pokazuje, czy ten projekt może się stać. Do dyspozycji dostajemy kilka obrazów systemu dla różnych urządzeń, przygotowanych oczywiście przez twórcę aplikacji, i gotowych do ściągnięcia po uruchomieniu programu.

Po ściągnięciu i uruchomieniu maszyny mamy do dyspozycji na razie dwie główne opcje, które pozwalają na testowanie naszych aplikacji: GPS i bateria.

Nie ma co tłumaczyć - trzeba spróbować. Dodatkowo - niedawno w repozytoriach wtyczek dla IntelliJ i Android Studio znalazłem wtyczkę dla [GenyMotion](http://plugins.jetbrains.com/plugin/7269?pr=idea) - więc kolejny duży plus.

[Strona domowa projektu](http://www.genymotion.com)