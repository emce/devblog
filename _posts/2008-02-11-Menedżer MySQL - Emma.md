---
title: Menedżer MySQL - Emma
date: 2008-02-11 09:12:54 +0100
categories: [linux]
tags: [linux,ubuntu,mysql,db manager]
image:
  path: /assets/img/emma.jpg
  alt: emma
author: michal_cwiklinski
toc: false
---

# Menedżer MySQL - Emma

Prześledziwszy niejeden wątek i niejedną dyskusję o menedżerze MySQL, po wielu testach i próbach do zaawansowanego zarządzania bazami MySQL dalej używamSQLyog'a, ale do drobnych "robótek" wyszukałem Emmę. Emma to menedżer MySQL napisany w Pythonie, jest następcą programu _yamysqlfront_.

Żeby sobie ułatwić sprawę, i nie kompilować programu - najpierw przeszukałem repozytorium Debiana - i - o dziwo - jest!!! No to:

```bash
apt-get install emma
```

i jest!.

Jak już wcześniej wspomniałem, program jest bardzo prosty, ale do prostego zarządzania (add, alter, select, insert, del, itd.) nadaje się znakomicie. Jest szybki, prosty w obsłudze, podczas pracy operuje się tylko w jednym oknie (nie trzeba otwierać dodatkowych, żeby np. wylistować tabelę).


Obecnie najbardziej aktualna wersja to 0.6, mam nadzieję, że autor jeszcze trochę nad tym programem popracuje.


[Strona domowa](http://www.fastflo.de/projects/emma)
[Download (60kB)](http://www.fastflo.de/files/emma/downloads/python_src/emma-0.6.tar.gz)