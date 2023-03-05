---
title: Otwieranie plików PDF w oknie przeglądarki Firefox
date: 2011-03-16 09:12:54 +0100
categories: [linux]
tags: [linux,firefox,pdf]
image:
  path: /assets/img/firefox.png
  alt: pdf w firefox
author: michal_cwiklinski
toc: false
---

# Otwieranie plików PDF w oknie przeglądarki Firefox

Pliki PDF są bardzo częstą formą zamieszczania informacji w Internecie. Aby przejrzeć treść takiej publikacji użytkownicy Linuksa do tej pory mieli dwa wyjścia - pierwsze - ściągnąć PDF na dysk i go przeczytać, drugie - zainstalować Google Chrome (nie Chromium!), który ma wbudowaną wtyczkę do przeglądania online tego typu plików. Co w przypadku Firefoxa?
Z pomocą przychodzi nam wtyczka _Mozplugger_, przy pomocy której możemy sprząc Firefoxa z Evince i przeglądać dokumenty PDF online w oknie przeglądarki. Procedura wygląda następująco:

1. Najpierw instalujemy mozpluggera:
```bash
apt-get install mozplugger
```
2. Następnie edytujemy plik konfiguracyjny tej wtyczki:
```bash
gedit ~/.mozilla/mozpluggerrc
```
3. a następnie wklejamy konfigurację:
```
application/pdf: pdf: PDF file
application/x-pdf: pdf: PDF file
text/pdf: pdf: PDF file
text/x-pdf: pdf: PDF file
application/x-postscript: ps: PostScript file
application/postscript: ps: PostScript file
   repeat noisy swallow(evince) fill: evince "$file"
```

Zapisujemy dane, restartujemy Firefoxa i voila!