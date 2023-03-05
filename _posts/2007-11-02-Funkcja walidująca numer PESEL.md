---
title: Funkcja walidująca numer PESEL
date: 2007-09-17 09:12:54 +0100
categories: [php]
tags: [php,validation,pesel]
image:
  path: /assets/img/php.jpg
  alt: Funkcja walidująca numer PESEL
author: michal_cwiklinski
toc: false
---

# Funkcja walidująca numer PESEL

Ostatnio przyszło mi stworzyć funkcję walidującą numer PESEL. Oczywiście najpierw pogrzebałem w encyklopedii (można ją znaleźć pod adresem www.google.pl), and viola!

Oto kod:
```php
function sprawdzPESEL($str)
{
    if (strlen($str) != 11 || !is_numeric($str)) //sprawdzamy czy podany numer ma 11 znaków
    {
        return false;
    }

    $arrSteps = array(1, 3, 7, 9, 1, 3, 7, 9, 1, 3); // tablica z odpowiednimi wagami
    $intSum = 0;
    for ($i = 0; $i < 10; $i++)
    {
        $intSum += $arrSteps[$i] * $str[$i]; //mnożymy każdy ze znaków przez wagę i sumujemy wszystko
    }
    $int = 10 - $intSum % 10; //obliczamy sumę kontrolną
    $intControlNr = ($int == 10)?0:$int;
    if ($intControlNr == $str[10]) //sprawdzamy czy taka sama suma kontrolna jest w ciągu
    {
        return true;
    }
    return false;
}
```