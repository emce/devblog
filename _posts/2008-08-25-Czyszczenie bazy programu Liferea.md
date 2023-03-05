---
title: Czyszczenie bazy programu Liferea
date: 2008-08-25 09:12:54 +0100
categories: [linux]
tags: [linux,ubuntu,rss,cache]
image:
  path: /assets/img/ubuntu.jpg
  alt: czyszczenie bazy
author: michal_cwiklinski
toc: false
---

# Czyszczenie bazy programu Liferea

Pracując w branży IT, każdy stara się być "na bieżąco" ze wszystkimi nowinkami z tej branży. A ja, jak każdy "nabieżącouser" korzystam z czytnika RSS. I jak każdy użytkownik (a przynajmniej większość) Linuksa korzystam z _Liferea_. Ponieważ Linuksa nie trzeba praktycznie przeinstalowywać (w przeciwieństwie do W.), baza newsów w Liferea w pewnym momencie puchnie, a program zaczyna "mulić". Co robić?

Liferea od wersji 1.4 została oparta o bazę danych _SQLite_. I tu z pomocą przychodzą nam narzedzia do tej właśnie bazy danych. Jeżeli nie mamy ich jeszcze zainstalowanych, to nic trudnego:

```bash
apt-get install sqlite3
```
i narzędzie jest zainstalowane. Pozostaje tylko wyczyścić bazę danych Liferea:
```bash
sqlite3 ~/.liferea_1.4/liferea.db vacuum
```
i gotowe.