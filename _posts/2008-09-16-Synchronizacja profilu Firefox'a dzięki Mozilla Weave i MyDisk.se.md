---
title: Synchronizacja profilu Firefox'a (Iceweasel'a) dzięki Mozilla Weave i MyDisk.se
date: 2008-09-16 09:12:54 +0100
categories: [linux]
tags: [linux,ubuntu,firefox]
image:
  path: /assets/img/iceweasel.jpg
  alt: iceweasel
author: michal_cwiklinski
toc: false
---

# Synchronizacja profilu Firefox'a (Iceweasel'a) dzięki Mozilla Weave i MyDisk.se

Problemem było zawsze przenoszenie danych przeglądarki (w tym przypadku Firefoxa-Iceweasela), czemu najpierw próbowała zaradzić wtyczka od Google - w chwili obecnej projekt zarzucony - a teraz wydany przez Mozillę projekt Mozilla Weave. Ale oparta o serwery WebDav Mozilli usługa bardzo często zawodzi (padają serwery). Trzeba zatem wykombinować coś innego.

Z pomocą przychodzi MyDisk.se.

Oto procedura postępowania:

1. Najpierw należy założyć konto na MyDisk.se.
2. Następnie połączyć się z serwerem, i założyć nowy katalog, nadając mu nazwę weave.
3. Następnie należy zainstalować wtyczkę Mozilla Weave.
4. Otwieramy Preferencje wtyczki, wybieramy zakładkę Advanced, i w pole adresu wpisujemy https://mydisk.se/username/weave/, gdzie username to nasz nick (użyty przy rejestracji konta na MyDisk.se.
5. Następnie z opcji wtyczki należy wybrać _Sign in_, wpisać login i hasło z konta na MyDisk.se, jako frazę szyfrującą podać dowolny ciąg znaków (najlepiej inny niż hasło) i zalogować się.
6. Teraz powinna nastąpić synchronizacja (jeżeli nie, to należy wybrać opcję _Sync now_).

Konto na MyDisk.se można wykorzystać jako zdalny dysk - w Linuxie sprawa jest prosta:
1. Należy zainstalować obsługę systemu plików `davfs`
```bash
apt-get install davfs2
```
2. Następnie należy dodać wpis do fstab:
```bash
http://mydisk.se/user/weave /mnt/mydisk user,auto 0 0
```
dla użytkowników, którzy będą montować zasób dodajemy dane niezbędne do logowania:
```bash
touch ~/.davfs2/secrets
```
w pliku secrets należy wpisać:
```bash
http://mydisk.se/user/weave user hasło
```

Gotowe!