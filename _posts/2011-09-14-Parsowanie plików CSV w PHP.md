---
title: Parsowanie plików CSV w PHP
date: 2011-09-14 09:12:54 +0100
categories: [php]
tags: [php,csv]
image:
  path: /assets/img/php.jpg
  alt: parsowanie csv
author: michal_cwiklinski
toc: false
---

# Parsowanie plików CSV w PHP

Dla każdego programisty przychodzi kiedyś czas, że dostaje do przeparsowania jakiś łądowany na serwer plik. Ostatnio są to XML'e, ale dalej bardzo często używane są pliki CSV. Oczywiście do tych ostatnich mamy w PHP natywną funkcję fgetcsv, ale nie jest ona doskonała.

Otóż - cytuję - 
```
Ustawienia lokale są brane pod uwagę przez tę funkcję. Jeśli LANG jest ustawione na np. en_US.UTF-8, pliki z jedno bajtowym kodowaniem zostaną nieprawidłowo odczytane przez funkcję.
```

Dlatego w pewnym sensie jesteśmy ograniczeni. Rozwiązanie - jest. Z pomocą przychodzi PHP 5.3 i obiekty SPL. Klasa `SplFileObject` ma bardzie wiele funkcji, a między innymi taką, która parsuje pliki CSV.

```php```
$file = new SplFileObject('/link/do/pliku.csv');
$file->setFlags(SplFileObject::READ_CSV | SplFileObject::SKIP_EMPTY | SplFileObject::DROP_NEW_LINE);
foreach($file as $row) { 
       /* tutaj robimy, co trzeba */
}
```

Jakież proste, aż przydatne...