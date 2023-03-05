---
title: Szybki chmod dla folderów
date: 2008-11-25 09:12:54 +0100
categories: [linux]
tags: [linux,ubuntu,access]
image:
  path: /assets/img/ubuntu.jpg
  alt: szybki chmod
author: michal_cwiklinski
toc: false
---

# Szybki chmod dla folderów

Chcesz zmienic uprawnienia dla podfolderów w danym folderze??? Nic prostszego!! Wystarczy skorzystać z polecenia `find`.

```bash
find . -type d -exec chmod 755 '{}' ;
```