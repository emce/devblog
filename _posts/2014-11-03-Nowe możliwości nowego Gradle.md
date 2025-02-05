---
title: Nowe możliwości nowego Gradle
date: 2014-11-03 09:12:54 +0100
categories: [android]
tags: [android studio,android,gradle,multidex]
image:
  path: /assets/img/android-gradle.png
  alt: gradle
author: michal_cwiklinski
toc: false
---

# Nowe możliwości nowego Gradle

Nowa wersja wtyczki Gradle została wydana 31 października, wraz z wersja 0.9 programu Android Studio, wprowadzając parę nowości.

Gradle może od teraz automatycznie usuwać nieużywane zasoby. Dla deweloperów to przydatna opcja, gdyż do tej pory trzeba było do tego uzywać dodatkowych narzędzi. Dodatkowo kolejnym zyskiem jest to, że Gradle usuwa niewykorzystane zasoby nie tylko z naszego kodu, ale co ważniejsze z bibliotek, których używamy. Funkcja `runProguard` została zastąpiona przez `minifyEnabled`, gdzie dodatkowo jeszcze mamy `shrinkResources`. 

Kolejną fajną rzeczą, którą niedawno odkryłem w najnowszej wersji Android Studio, jest opcja obsługi `multiDex`, czyli tworzenia więcej niż jednego pliku dex dla aplikacji. Jest to funkcjonalność bardzo przydatna dla wszystkich programistów borykających się z limitem 65 tysięcy metod, ograniczającym właśnie tworzenie pojedynczego pliku dex - wystarczy tylko użyć Goole Play services, Facebook SDK i problem gotowy. W sekcji `defaultConfig` naszego `build.gradle` dodajemy wpis `multiDexEnabled true`, do zależności zaś `compile 'com.android.support:multidex:1.0.0'`, oraz w albo w `AndroidManifest.xml` ustawimy jako klasę naszej aplikacji `MultiDexApplication`
```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.example.android.multidex.myapplication">
    <application
        ...
        android:name="android.support.multidex.MultiDexApplication">
        ...
    </application>
</manifest>
```
albo jeżeli mieliśmy klasę extendującą Application, zamieniamy Application na MultiDexApplication. Więcej info na ten temat [tutaj](https://developer.android.com/tools/building/multidex.html).

Voila!