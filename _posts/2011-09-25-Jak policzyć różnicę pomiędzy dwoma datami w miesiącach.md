---
title: Jak policzyć różnicę pomiędzy dwoma datami w miesiącach?
date: 2011-09-25 09:12:54 +0100
categories: [database]
tags: [database,postgresql,date]
image:
  path: /assets/img/postgresql.png
  alt: różnica między datami
author: michal_cwiklinski
toc: false
---

# Jak policzyć różnicę pomiędzy dwoma datami w miesiącach?

```sql
SELECT (date_part('year', age(data_koniec, data_poczatek)) * 12) + date_part('month', age(data_koniec, data_poczatek)) AS miesiace FROM jakas_tabela; 
```