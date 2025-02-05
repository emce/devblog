---
title: Graficzne projektowanie baz danych
date: 2009-01-11 09:12:54 +0100
categories: [database]
tags: [database,architecture,mysql]
image:
  path: /assets/img/start-blogowania.jpg
  alt: start blogowania
author: michal_cwiklinski
toc: false
---

# Graficzne projektowanie baz danych

Pomimo obfitości oprogramowania na Linuksa, istnieją jeszcze "nisze" programistyczne. Graficzne projektowanie relacyjnych baz danych w Windowsie posiadało wsparcie w kilku, jak nie kilkunastu programach. A Linux??

Tutaj nie było aż tak tragicznie, bo był DBDesigner, ale samo przeportowanie go z rpma do środowiska Debiana bądź Ubuntu to było nie lada wyzwanie, choćby ze względu na szybkość zmian w bibliotece do obsługi połączeń i zapytań SQL. Niedawno na rynku pojawił się następca DBDesignera 4, zwany DBDesigner'em 5 - [MySQL WorkBench](http://dev.mysql.com/workbench/). W chwili obecnej wersja stabilna to 5.0.29, a w wersji alpha to 5.1.7.

Jak wygląda instalacja?? Na samym początku instalujemy biblioteki `ctemplate` by Google:
```bash
http://code.google.com/p/google-ctemplate/downloads/list
```
a następnie instalujemy:
```bash
dpkg-i libctemplate*
```

Następnie dodajemy do `sources.list` wpisy:
```bash
deb ftp://ftp.mysql.com/pub/mysql/download/gui-tools/ubuntu/ binary/
deb-src ftp://ftp.mysql.com/pub/mysql/download/gui-tools/ubuntu/ source/
```

A teraz wystarczy normalnie wydać polecenia:
```bash
apt-get update
apt-get install mysql-workbench-oss
```

Po zaprojektowaniu bazy eksportujemy cały widok do pliku *.sql - i gotowe!