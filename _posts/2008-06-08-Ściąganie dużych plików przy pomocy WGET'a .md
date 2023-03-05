---
title: Ściąganie dużych plików przy pomocy WGET'a (+ praca w tle)
date: 2008-06-08 09:12:54 +0100
categories: [linux]
tags: [linux,ubuntu,wget]
image:
  path: /assets/img/ubuntu.jpg
  alt: ściąganie dużych plików
author: michal_cwiklinski
toc: false
---

# Ściąganie dużych plików przy pomocy WGET'a (+ praca w tle)

Ściąganie dużych plików (mam oczywiście na myśli obrazy ISO dystrybucji linuksowych ;) ) często odbywa się kosztem "czekania aż do ściągnięcia". Jeżeli nie chcemy czekać, to zazwyczaj instalujemy jakiegoś "download managera", coby ściągnął nam pożądany plik. Po co? Przecież mamy `wget`'a...!

A sprawa jest prosta: jedno polecenie, odpowiednie przełączniki, i ściągamy:

```bash
wget -bqc http://sciezka.do.pliku/obraz.iso
```
gdzie:

- `-b`: uruchomienie w tle zaraz po starcie. Jeśli nie zostanie określony plik wyjściowy za pośrednictwem -o, wyjście jest przekierowywane do wget-log
- `-q`: oszczędza miejsce na dysku
- `-c`: wznawianie pobierania tzn. ponawianie przy zerwaniu połączenia, itp. Przydatne, gdy chcesz zakończyć pobieranie przy pomocy `wget`a lub innego programu

Można także użyć polecenia `-nohup` do wykonania poleceń po wyjeździe z powłoki: 
```bash
nohup wget http://sciezka.do.pliku/obraz.iso &
```