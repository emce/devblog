---
title: Zestaw przydatnych bibliotek
date: 2013-02-04 09:12:54 +0100
categories: [android]
tags: [android,library,event bus,dependency injection,unit testing]
image:
  path: /assets/img/android-libraries.png
  alt: przydatne biblioteki
author: michal_cwiklinski
toc: false
---

# Zestaw przydatnych bibliotek

Kilka propozycji, z którymi ostatnio zdarzyło mi się pracować:

**MenuDrawer** - biblioteka dla wysuwanego menu, która pozwala użytkownikom na przemiszczanie sie pomiedzy widokami w aplikacji. Menu pokazuje się po przeciągnięciu ekranu "za brzeg", albo po kliknięciu ikony w Action Bar'ze. 
[GitHub](https://github.com/SimonVT/android-menudrawer)
[Aplikacja - przykład](http://simonvt.github.com/android-menudrawer/android-menudrawer-sample-2.0.0.apk)

**Otto** - bardzo użyteczna biblioteka, napisana na bazie Guava event bus, pozwalająca na oddzielenie różnych części aplikacji, a jednocześnie pozwalająca im komunikować się efektywnie. Działa na zasadzie subskrypcji i produkcji "eventów". Po przykłady odsyłam do strony. 
[GitHub](http://github.com/square/otto)

**Crouton** - biblioteka stanowiąca alternatywę dla natywnych "toastów" - pozwala na wyświetlanie w dowolnym miejscu, wyświetlanie dowolnej liczby oraz wyrównywanie komunikatów do konkretnych elementów widoku, oraz stylowanie tychże komunikatów. 
[GitHub](https://github.com/keyboardsurfer/Crouton)
[Aplikacja - przykład](http://play.google.com/store/apps/details?id=de.keyboardsurfer.app.demo.crouton)

**Dagger** - biblioteka do obsługi wstrzykiwania zależności - alternatywa dla Guice'a. 
[GitHub](https://github.com/square/dagger)

**FEST Android** - kolejny i ostatni prezentowany tutaj wytwór Jake'a Whartona i ekipy ze Square - biblioteka bardzo przydatna przy pisaniu testów jednostkowych w Androidzie, pozwalająca szybko i deklaratywnie przetestować właściwości w różnych klasach aplikacji - dzięki specjalnym asercjom. 
[GitHub](https://github.com/square/fest-android)

Happy using!