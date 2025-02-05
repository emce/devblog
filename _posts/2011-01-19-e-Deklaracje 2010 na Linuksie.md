---
title: e-Deklaracje 2010 na Linuksie
date: 2011-01-19 09:12:54 +0100
categories: [linux]
tags: [linux,debian,adobe air]
image:
  path: /assets/img/debian.jpg
  alt: e-deklaracje na linuxie
author: michal_cwiklinski
toc: false
---

# e-Deklaracje 2010 na Linuksie

Nadchodzi czas, kiedy z trwogą sięgamy po wszystkie PITY-11, jakie mamy, aby rozliczyć się z fiskusem. Nasz rząd poszedł nam w końcu na rękę (po latach wojowania) i w tym roku można rozliczyć się przez Internet i to bez podpisu cyfrowego. Ministerstwo Finansów opublikowało na swojej stronie (dostępny również wcześniej) program na platformę Adobe AIR (więc niby system independent), dzięki któremu możemy rozliczyć się przez Internet, i to nawet wspólnie z małżonkiem, Oczywiście sztuka nie jest aż taka prosta, jakby się wydawała, bo choć pod Windowsem sprawa jest prosta, to pod Linuksem jest trochę trudniej...Podstawowym wymaganie programu e-Deklaracje oprócz oczywiście środowiska Adobe AIR jest posiadanie Adobe Readera w wersji >8.1.3. Musi on mieć odpowiednią wtyczkę, gdyż formularze PIT dostępne w e-Deklaracjach wypełniamy właśnie w Adobe Reader'ze. Opiszę moją sytuację bazując na Debianie, gdyż z takiego systemu aktualnie korzystam.

Otóż: mam zainstalowanego Adobe Readera z repozytorium debian-multimedia. I niby wszystko jest ok. Adobe AIR po wielu bojach (korzystam z Debiana na architekturze AMD64) został zainstalowany, aplikacja e-Deklaracje też. No to do dzieła. Ale, ale - po otwarciu programu i wybraniu formularza dostaję komunikat, że nie mam Adobe Reader'a. Jak to - przecież mam! Ale programiści e-Deklaracji założyli, że Adobe Readera będziemy instalować po pobraniu ze strony Adobe, i szukają go w /opt/Adobe/Reader9, podczas gdy on sobie grzecznie siedzi w /usr/lib/Adobe/Reader9. Rozwiązanie? Proste:
```bash
ln -s /usr/lib/Adobe/Reader9 /opt/Adobe/Reader9
```
No dobra - restart aplikacji, odpalam formularz PIT-37 - komunikat - masz za niską wersję Adobe Readera. Hola hola - przecież mam wyższą niż wymagana! Po kilku minutach szperania - mam - nie zainstalowałem plugin'ów do Adobe Readera. Rozwiązanie:
```bash
apt-get install acroread-plugins
```
I voila - wszystko działa!
No to do wypełniania formularzy!
[Adobe AIR](http://get.adobe.com/air/)
[e-Deklaracje](http://www.e-deklaracje.gov.pl/)
[Debian Multimedia](http://debian-multimedia.org/)

P.S. I tak nie mam się z czego cieszyć, bo wszystkie dodatki typu PIT-O trzeba zanieść do Urzędu.