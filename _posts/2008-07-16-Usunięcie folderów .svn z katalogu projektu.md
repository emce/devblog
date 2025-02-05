---
title: Usunięcie folderów .svn z katalogu projektu
date: 2008-07-16 09:12:54 +0100
categories: [linux]
tags: [linux,ubuntu,svn,project,version control]
image:
  path: /assets/img/ubuntu.jpg
  alt: usunięcie folderów .svn
author: michal_cwiklinski
toc: false
---

# Usunięcie folderów .svn z katalogu projektu

Korzystając z systemu kontroli wersji SVN zdarza się czasami, że trzeba wrzucić na serwer FTP bieżącą wersję, ale jest ona pełna katalogów tworzonych przez system (`.svn`). Bez problemu można je wtedy usunąć jednym poleceniem.

W wierszu poleceń, po przejściu do katalogu projektu, należy wpisać:
```bash
find . -name ".svn" -exec rm -rf {} ;
```

 I wszystkie foldery `.svn` usunięte.