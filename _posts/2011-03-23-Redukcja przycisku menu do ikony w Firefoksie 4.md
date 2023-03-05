---
title: Redukcja przycisku menu do ikony w Firefoksie 4
date: 2011-03-23 09:12:54 +0100
categories: [linux]
tags: [linux,firefox]
image:
  path: /assets/img/firefox.png
  alt: start blogowania
author: michal_cwiklinski
toc: false
---

# Redukcja przycisku menu do ikony w Firefoksie 4

Firefox 4 jest wyposażony w wiele nowych funkcji, jedną z nich jest wprowadzenie przycisku menu umieszczonego po lewej stronie paska z kartami. Przycisk ten pojawai się, gdy tradycyjny pasek menu zostaje ukryty. Na mój gust jest on zbyt szeroki i brzydki, ale na szczęście można zredukować go do ikony przy pomocy stosunkowo łatwego hacka i nie tracąc przy tym żadnych funkcjonalności. Jak to zrobić?

Sprawa jest prosta: tworzymy lub edytujemy plik userChrome.css, dodając kod:
```css
#appmenu-toolbar-button {
    list-style-image: url("chrome://branding/content/icon16.png");
}
#appmenu-toolbar-button > .toolbarbutton-text,
#appmenu-toolbar-button > .toolbarbutton-menu-dropmarker {
    display: none !important;
}
```