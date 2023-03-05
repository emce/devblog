---
title: Idzie ku lepszemu - nowe GUI do baz danych
date: 2010-03-25 09:12:54 +0100
categories: [database]
tags: [database,linux,ubuntu,gui,postgresql]
image:
  path: /assets/img/postgresql.png
  alt: nowe gui do baz danych
author: michal_cwiklinski
toc: false
---

# Idzie ku lepszemu - nowe GUI do baz danych

Krążąc po zasobach Softpedii znalazłem dobrze zapowiadający się projekt GUI do obslugi baz danych, "produkowany" przez rosyjskich programistów. GSQL jest jeszcze w fazie testów, ale patrząc po screenach z ostatnich buildów zapowiada się ciekawie...

W chwili obecnej programiści do obsługi MySQL dołożyli PostgreSQLa (co osobiście mnie bardzo cieszy), a także starają się zintegrować (mocno) całą aplikację ze środowiskiem (Gnome w tym przypadku).

### Nowości:
- PostgreSQL (eksperymentalne)
- plugin tunelowania (wymagane libssh 0.4.*)

### Założenia na przyszłość: 
- Nowy model struktury drzewa obiektów bazy danych. Dane będą przetrzymywane w pliku XML.
- Dokowane elementy interfejsu
- Migracja do Gnome 3
- Nowe API będzie bardziej użyteczniejsze i znacznie uprości opracowywanie własnych silników i wtyczek.

Strona projektu: [GSQL](http://code.google.com/p/gsql/)

AKTUALIZACJA:
Repozytorium dla Ubuntu:
```bash
add-apt-repository ppa:add-apt-repository ppa:halturin/gsql
```