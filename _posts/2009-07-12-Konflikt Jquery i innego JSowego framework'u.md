---
title: Konflikt Jquery i innego JSowego framework'u
date: 2009-07-12 09:12:54 +0100
categories: [javascript]
tags: [javascript,jquery,framework]
image:
  path: /assets/img/code.jpg
  alt: konflikt jquery
author: michal_cwiklinski
toc: false
---

# Konflikt Jquery i innego JSowego framework'u

Zdarza się - ja przy pracy z _SilverStripe_'m dopinałem _Jquery_ do istniejącego już w tym CMSie _Prototype_'a. No i pojawił się problem - gdy na jednej ze stron wchodziła funkcja prototype'owa - funkcje _Jquery_ nie działały. Jednym słowem - konflikt.

Ale okazuje się, że _Jquery_ przewiduje takie sytuacje. Istnieje w tym framework'u tryb _noConflict_. Wykorzystując go, definiujemy sobie nową zmienną dla klasy _Jquery_, np.: `J`:
```javascript
var J = jQuery.noConflict();
```

Wystarczy przepisać teraz wszzystkie Jquerowe `$` na `J` i funkcje współdziałają!