---
title: Liczba porządkowa pozycji na liście
date: 2009-08-17 09:12:54 +0100
categories: [database]
tags: [database,postgresql]
image:
  path: /assets/img/postgresql.png
  alt: liczba porządkowa
author: michal_cwiklinski
toc: false
---

# Liczba porządkowa pozycji na liście

Super szybkie rozwiązanie sytuacji, w której chcemy uzyskać liczbę porządkową (pozycję) pozycji na liście (czyli row'a w zapytaniu). W bazie istnieje tabelka, w której znajdują się np.: ceny artykułu w różnych sklepach. Chcąc dowiedzieć się, jaką dany sklep zajmuje pozycję względem cen, normalnie trzeba by było pobrać całą listę i przerabiać ją w PHP'ie, albo robić skomplikowane zapytanie. Ale jest inne rozwiązanie.

Z pomocą przychodzi Oracle'owy `rownum`:

```sql
SELECT rownum FROM ( 
SELECT @rownum:=@rownum+1 rownum, 
t.* 
FROM 
(SELECT @rownum:=0) r, 
shopStats t 
ORDER BY t.lngAvgGrade) t1 
WHERE t1.id=132
```