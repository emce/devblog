---
title: Generowanie zasobów graficznych w GIMPie
date: 2014-08-24 09:12:54 +0100
categories: [linux]
tags: [android,gimp,icons,scriptfu]
image:
  path: /assets/img/scriptfu.jpg
  alt: generowanie zasobów
author: michal_cwiklinski
toc: false
---

# Generowanie zasobów graficznych w GIMPie

Znalazłem ostatnio w sieci świetny i prosty Script-Fu dla GIMPa do generowania Androidowych resource'ów. Oprócz standardowych ikon launchera, generuje także konfigurowalne grafiki - wystarczy ustawić wartości dla gęstości mdpi, resztę sam przeliczy i wygeneruje.

Instalacja jest banalna - pobieramy plik (załączony w tym wpisie), rozpakowujemy go z prawami roota do
```bash
/usr/share/gimp/2.0/scripts/
```
lub
```bash
C:\Program Files\GIMP-2.0\share\gimp\2.0\scripts
```
i restartujemy GIMPa. Skrypt można znaleźć w `Script-Fu` -> `Android` -> `Save Android Icons`.  W pozycji `Android icons type` można znaleźć następujące typy generowanych zasobów:
- Launcher Icons
- Menu Icons
- Action Bar Icons
- Status Bar Icons
- Tab Icons Dialog Icons
- List View Icons
- Custom Icon

Jak widać wybór jest szeroki. Rozdzielczość zapisanych ikon będzie zależeć od tego, jaki typ ikony został wybrany. W przypadku wybrania opcji `Custom Icon` należy wprowadzić żądaną rozdzielczość dla ikony mdpi, jako wymiar bazowy ikon. Ikony będą skalowane według wytycznych:
- ikona ldpi będzie miała 0,75 szerokości i wysokości ikony mdpi
- ikona hdpi będzie 1,5 szerokości i wysokości ikony mdpi
- ikona xhdpi będzie 2,0 szerokości i wysokości ikony mdpi
- itd.

Dzięki opcjom z grupy `Save mode options` można wybrać opcje trybu zapisu ikon, gdzie można zapisać ikony pod tą samą nazwą, ale w różnych folderach ze standardowymi nazwami folderów androidowych (`drawable-ldpi`, `drawable-mdpi`, itd.) lub zapisać wszystkie ikony w tym samym folderze, ale z dodatkiem nazwy rozdzielczości na ​​końcu nazwy pliku. Po wybraniu pierwszej opcje należy upewnić się, że istnieją odpowiednie foldery(`drawable-ldpi`, `drawable-mdpi`, `drawable-hdpi`, itd) w wybranym folderze głównym, gdyż skrypt się może z lekka "wysypać". Opcja `Use android naming conventions` pozwala na generowanie ikon ze wspólnym przedrostkiem, zależnym od rodzaju wybranego w pierwszej opcji. 

Miłego korzystania.