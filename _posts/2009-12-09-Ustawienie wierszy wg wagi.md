---
title: Ustawienie wierszy wg wagi
date: 2009-12-09 09:12:54 +0100
categories: [database]
tags: [database,mysql,postgresql,sql]
image:
  path: /assets/img/mysql-ubuntu.jpg
  alt: wiersze wg wagi
author: michal_cwiklinski
toc: false
---

# Ustawienie wierszy wg wagi

Zapytanie, które pozwala ustawić pobierane wiersze w kolejności i z przypisanym licznikiem wg wybranego pola (wykorzystanie Oracle'owego `ROWNUM`).

```sql
SELECT @rownum:=@rownum+1 rownum, t.*FROM (SELECT @rownum:=0) r, mytable t;
```