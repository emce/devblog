---
title: Nokia PC Suite dla linuxa
date: 2008-04-18 09:12:54 +0100
categories: [linux]
tags: [linux,nokia,phone]
image:
  path: /assets/img/debian.jpg
  alt: nokia pc suite
author: michal_cwiklinski
toc: false
---

# Nokia PC Suite dla linuxa

W dobie "szukania zamienników" przepis na Nokia PC Suite dla Debiana - czyli jak podłączyć pamięć telefonu Nokia przez kabelek USB.

Kilka prostych kroków:

1. Ściągamy bardzo prosty i fajny program ObexTool: 
```bash
wget http://garr.dl.sourceforge.net/sourceforge/obextool/obextool-0.33.tar.gz
```
którego na razie nie ma w repozytorium pakietów Debiana (jest za to w repozytoriach Ubuntu).

2. Rozpakowujemy go gdzieś, skąd będzie można go odpalić (np,: `/usr/share/obextool`);

3.Instalujemy niezbędne pakiety:
```bash
apt-get install obexftp obexfs openobex-apps tk8.5 bwidget tklib
```

4. Teraz podłączamy nasz telefonik za pomocą kabelka USB, i przy pomocy narzędzia obexftp wykrywamy "jego obecność":
```bash
obexftp -u
```

5. Ustanawiamy testowe połączenie wydając polecenie: 
```bash
obexftp -u 1 -l
```
6. Teraz wystarczy na Desktopie stworzyć nowy skrót, w wierszu aplikacji wpisując: `/usr/share/obextool/obextool.tk --obexcmd "obexftp -u 1"` - i do dzieła!!!

Powodzenia!