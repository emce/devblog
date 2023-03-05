---
title: Zdublowane wpisy
date: 2007-11-12 09:12:54 +0100
categories: [database]
tags: [database,mysql,duplicates]
image:
  path: /assets/img/mysql-ubuntu.jpg
  alt: Zdublowane wpisy
author: michal_cwiklinski
toc: false
---

# Zdublowane wpisy

Jak wyświetlić lub pozbyć się zdublowanych wpisów w tabeli? Nic prostszego! Jedno zapytanie i mamy listę "dubli"...

```sql
SELECT * FROM `nazwa_tabeli`
    GROUP BY nazwa_kolumny 
    HAVING COUNT(nazwa_kolumny) > 1
```