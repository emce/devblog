---
title: Instalacja SDK w wierszu poleceń
date: 2012-08-28 09:12:54 +0100
categories: [android]
tags: [android,android sdk,terminal,command line]
image:
  path: /assets/img/android-sdk.png
  alt: android sdk
author: michal_cwiklinski
toc: false
---

# Instalacja SDK w wierszu poleceń

W przypadku wydawania swojego produktu przy pomocy takich narzędzi, jakim jest np.: Jenkins, trzeba sobie odpowiednio przygotować środowisko. W moim przypadku, budując w Jenkinsie pliki *.apk, wykorzystuję zarówno repozytorium GIT'a, Jave Sun'a/Oracle'a, Maven'a, no i oczywiście Android SDK. 

Jakkolwiek instalacja GIT'a i Maven'a na serwerach jest prosta (binraki są zazwyczaj dostępne w repozytoriach), tak instalacja Android SDK może sprawić trochę kłopotu. Normalnie Android SDK instalujemy wykorzystując wygodne GUI. Ale sprawę w moim przypadku komplikował fakt, że na serwerze nie ma środowiska graficznego. Można takie zainstalować, ale pytanie: po co? Instalator SDK przewiduje bowiem tryb CLI (konsolowy). Kilka prostych poleceń i SDK zainstalowane. I to te wersje, które chcemy. 

Ale po kolei:
1. Ściągamy instalator [stąd](http://dl.google.com/android/android-sdk_r20.0.3-linux.tgz), i rozpakowujemy go w wybrane miejsce.
2. Wchodzimy do katalogu tools, i następne odpalamy polecenie: `./android list sdk --no-ui`. Wyświetla się lista wszystkich dostępnych pakietów. Oczywiście w tym momencie możemy zainstalować wszystko, ale będzie wymagało to podania między innymi danych dostępowych do witryn deweloperskich firm HTC i Motorola, no i całość zajmie trochę miejsca.
3. Aby zainstalować tylko wybrane pakiety, trzeba z listy wybrać ich pozycję, a następnie wpisać w następującym poleceniu: `./android update sdk --filter 1,2,3,4,5,6,7,8,9,10,11,12,27,28,32,33,26,38,40,57,61,62,64 --no-ui`
4. Ustawiamy sobie zmienną środowiskową `ANDROID_HOME`, i voila!