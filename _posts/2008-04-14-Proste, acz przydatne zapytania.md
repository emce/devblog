---
title: Proste, acz przydatne zapytania...
date: 2008-04-14 09:12:54 +0100
categories: [database]
tags: [database,mysql,date]
image:
  path: /assets/img/start-blogowania.jpg
  alt: start blogowania
author: michal_cwiklinski
toc: false
---

# Proste, acz przydatne zapytania...

Kilka prostych, ale czasami przydatnych (trudnych do znalezienia) zapytań SQL-owych, które mogą czasami zaoszczędzić parę minutek i naszych neuronów...

1. Ile lat minęło od jednej daty do drugiej (i nie na zasadzie odejmowania liczb "roku"):
```sql
SELECT DATEDIFF(data_1,data_2)/365 FROM tabela
```