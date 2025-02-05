---
title: Backportowe repozytorium teraz oficjalnym repozytorium Debiana
date: 2010-09-07 09:12:54 +0100
categories: [linux]
tags: [linux,debian,backports]
image:
  path: /assets/img/debian.jpg
  alt: debian
author: michal_cwiklinski
toc: false
---

# Backportowe repozytorium teraz oficjalnym repozytorium Debiana

Każda dystrybucja Linuksa musi liczyć się z tym, że bycie stabilną, a posiadanie najświeższych pakietów zazwyczaj nie idzie w parze. A już na pewno nie w przypadku Debiana, który jest bardzo stabilną dystrybucją, ale w którym pakiety są daleko w tyle z numerkami. zeby zainstalować najnowsze wersję, trzeba skorzystać z gałęzi testing, bądź wręcz unstable.

Użytkownicy Debiana, którzy chcieli być na bieżąco z aktualnymi wersjami oprogramowania, musieli korzystać z nieoficjalnych backportowych repozytoriów. Backportowe aplikacje są w miarę aktualne z tymi wydawanymi przez ich twórców, a instalacja ich nie wymaga aktualizacji większości składników systemu, a przede wszystkim aktualizacji systemu do wersji testowej bądź niestabilnej. Od wczoraj repozytorium backports.debian.org dla Debiana Lenny stało się oficjalnym repozytorium tej dystrybucji. Aby skorzystać z tego repozytorium, należy dodać do `/etc/apt/sources.list` wpis:
```bash
deb http://backports.debian.org/debian-backports lenny-backports main
```

Szczegółowe instrukcje [tutaj](http://backports.debian.org/Instructions/).