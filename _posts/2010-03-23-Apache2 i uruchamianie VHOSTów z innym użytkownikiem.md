---
title: Apache2 i uruchamianie VHOSTów z innym użytkownikiem
date: 2010-03-23 09:12:54 +0100
categories: [linux]
tags: [linux,ubuntu,apache,apache2,vhost,mpm]
image:
  path: /assets/img/apache2.png
  alt: apache2
author: michal_cwiklinski
toc: false
---

# Apache2 i uruchamianie VHOSTów z innym użytkownikiem

Człowiek się uczy i uczy...

Zamiast bawić się w ustawianie uprawnień dla użytkowników poszczególnych serwisów, które są skonfigurowane w vhostach, wystarczy doinstalować jeden pakiet do apache2 i po sprawie.

Ten pakiet to `apache2-mpm-itk`. Instalacja jest bardzo prosta:
```bash
apt-get install apache2-mpm-itk
```

Potrzebna jest grupa, która będzie przypisana do zarządzania vhostem:
```bash
groupadd stronki
```
a także użytkownik w tej grupie:
```bash
useradd -s /bin/false -d /home/stronki -m -g stronki stronki_test
```

Na koniec oczywiście wpis w pliku z definicjami vhostów:
```bash
<IfModule mpm_itk_module>
AssignUserId stronki_test stronki
</IfModule>
```
restart Apache i gotowe.

P.S.
Czasami są problemy z sesją, więc trzeba ustawić ścieżkę do sesji na `/tmp`.