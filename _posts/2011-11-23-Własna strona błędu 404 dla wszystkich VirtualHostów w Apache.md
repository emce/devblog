---
title: Własna strona błędu 404 dla wszystkich VirtualHostów w Apache
date: 2011-11-23 09:12:54 +0100
categories: [linux]
tags: [linux,apache2,vhost]
image:
  path: /assets/img/404.png
  alt: strona 404
author: michal_cwiklinski
toc: false
---

# Własna strona błędu 404 dla wszystkich VirtualHostów w Apache

Małe problemy potrafią generować duże jednostki czasowe potrzebne na ich rozwiązanie. Jak ustawić własną stronę błedu 404 dla wszystkich VirtualHostów, które masz na serwerze?? Prosto - nie da się. Deklaracja ErrorDocument przyjmuje tylko ścieżki relatywne do RootDocument - czyli nie wychodzące poza katalog VirtualHosta. Co zrobić?

Rozwiązanie jest trywialne, ale trzeba było się naszukać - wystarczy dodać alias, a później jako ścieżkę podać ten alias:

```bash
Alias /404 /sciezka/do/pliku/404.html
ErrorDocument 404 /404
```