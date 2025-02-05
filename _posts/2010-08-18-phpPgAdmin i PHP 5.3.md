---
title: phpPgAdmin i PHP 5.3
date: 2010-08-18 09:12:54 +0100
categories: [php]
tags: [php,postgresql,database,phppgadmin]
image:
  path: /assets/img/postgresql.png
  alt: phppgadmin i php
author: michal_cwiklinski
toc: false
---

# phpPgAdmin i PHP 5.3

Przejście na PHP 5.3 na większości serwerów WWW jest chyba coraz bardziej "normalne". Ale takie przejście jak zwykle nie odbywa sie prosto i bez "bólu".

W moim przypadku wystąpiły problemy z phpPgAdminem. Problemem jest oczywiście użycie funkcji PHP, które z wersji 5.3 są przestarzałe (deprecated). Otóż wg twórców tej fajnej poniekąd aplikacji:
```
There's no plans yet to support PHP 5.3 under the 4.2 branch of PPA as we
are focusing on PPA 5.0 fixes and release.
```
i wersja 4.2.3 nie zostanie już poprawiona.

Zatem, trzeba to zrobić ręcznie.

Żeby phpPgAdmin śmigał w miarę normalnie, wystarczy zrobić dwie rzeczy:

1. Pierwszy błąd to:
```
PHP Deprecated: Assigning the return value of new by reference is deprecated in /usr/share/phpPgAdmin/classes/Misc.php on line 342
```
Tutaj wystarczy mała modyfikacja pliku `/usr/share/phpPgAdmin/classes/Misc.php`, w którym zmieniamy
```php
$data =&new $_type($_connection->conn);
```
na
```php
$data =new $_type($_connection->conn);
```
czyli wyrzucamy przestarzałą referencję.

2. Druga sprawa to upierdliwy komunikat o strefie czasowej - to załatawiamy wpisem do `php.ini`:
```bash
date.timezone = 'Europe/Warsaw'
```

Voila!