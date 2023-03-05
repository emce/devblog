---
title: Montowanie obrazów dysków (ISO, IMG, BIN, MDF i NRG)
date: 2008-12-30 09:12:54 +0100
categories: [linux]
tags: [linux,ubuntu,mounting,images,iso,mdf]
image:
  path: /assets/img/ubuntu.jpg
  alt: montowanie obrazów dysków
author: michal_cwiklinski
toc: false
---

# Montowanie obrazów dysków (ISO, IMG, BIN, MDF i NRG)

Brakuje czasami w Linux'ie narzędzi, które chętnie przenieślibyśmy z Windowsa. Takim narzędziem jest np.: _Daemon Tools_, dzięki któremu można zamontować dowolny obraz płyty. Ale można znaleźć odpowiednik tego programu.

Tym programem jest _Furius ISO Mount_. Program jest bardzo prosty w użyciu, wystarczy wskazać plik, i kliknąć przycisk _Zamontuj_, a obraz zostanie zamontowany w naszym folderze domowym.

[Strona programu](http://www.marcus-furius.com/?page_id=14)
[Plik *.deb](http://www.marcus-furius.com/files/FuriusIsoMount/pyfuriusisomount/furiusisomount_0.11.1.0-1_i386.deb)

Instalujemy najpierw plik *.deb:
```bash
dpkg -i furiusisomount*
```
a później łatamy zależności:
```bash
apt-get -f install
```