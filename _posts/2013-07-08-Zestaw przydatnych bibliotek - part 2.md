---
title: Zestaw przydatnych bibliotek - part 2
date: 2013-07-08 09:12:54 +0100
categories: [android]
tags: [android,event bus,google maps]
image:
  path: /assets/img/android-libraries.png
  alt: przydatne biblioteki
author: michal_cwiklinski
toc: false
---

# Zestaw przydatnych bibliotek - part 2

Kontynuacja listy przydatnych bibliotek dla programisty Androida. 

**EventBus** - przeczytałem gdzieś bardzie ciekawe tłumaczenie mechanizmu "event bus" - magistrala zdarzeń. EventBus jest Androidową zoptymalizowanych mechanizmem publikowania/subskrybowania zdarzeń. Typowym zastosowaniem tego mechanizmu jest łączenie aktywności (Activities), fragmentów (Fragments) i wątków (przede wszystkim AsyncTasks). EventBus oddziela od siebie całkowicie producentów i subskrybentów zdarzeń, bez użycia choćby jednego interfejsu. Implementacja i API Subskrybenci muszą tylko zaimplementować metody obsługujące konkretne zdarzenia, oraz zarejestrować się w EventBusie, zaś producenci zdarzeń po prostu jest publikują. Wysłane zdarzenia są dopasowywane na podstawie typu (klasy) zdarzenia. Przykładowa implementacja metody obsługującej zdarzenie: 
```java
public void onEvent(AnyEventType event) {}
```
Rejestracja subskrybenta: 
```java
eventBus.register(this);
```
Publikacja zdarzenia:
```java
eventBus.post(event);
```
Wyrejestrowanie subskrybenta:
```java
eventBus.unregister(this);
```
[GitHub](http://greenrobot.github.io/EventBus/)

**Android Query** - biblioteka ta ma tyle zastosowań, że aż nie chce mi się opisywać - wystarczy wejść na stronę projektu, i znaleźć to zastosowanie, które jest w danej chwili potrzebne. Nadmienię tylko, że bardzo pomaga w pisaniu małej ilości kodu. 
[Strona projektu](https://code.google.com/p/android-query/)

**Android Maps Extensions** - biblioteka rozszerzająca możliwości Android Google Maps API v2.
Można się długo rozpisywać na jej temat, ale wystarczy wspomnień możliwość klastrowania markerów, czy "dołączania do markerów dodatkowych informacji (metoda `marker.setData(Object object)`). 
[Strona projektu](https://code.google.com/p/android-maps-extensions/)