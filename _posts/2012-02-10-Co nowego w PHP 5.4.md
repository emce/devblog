---
title: Co nowego w PHP 5.4?
date: 2007-09-17 09:12:54 +0100
categories: [php]
tags: [php]
image:
  path: /assets/img/php.jpg
  alt: php
author: michal_cwiklinski
toc: false
---

# Co nowego w PHP 5.4?

PHP w wersji 5.4 znalazło się niedawno w w dystrybucji unstable Debiana. Czas zwrócić więc trochę więcej uwagi na to, co zmienia się w tej wersji.

- tryby safe_mode oraz register_globals zostały całkowicie usunięte
- domyślnym kodowaniem jest utf8
- `magic_quotes` jest przestarzałe (deprecated)
- poprawione rozszerzenie obsługi sesji
    - możliwość obsługi postępu wysyłania (odpowiednia konfiguracja pliku php.ini)
    - możliwość sprawdzenia stanu sesji - funkcja `session_status()`
    - obiektowe handlery sesji
- wbudowany serwer www
    - mały, na razie tylko do zastosowań deweloperskich (`php -S localhost:8080`)
- trzy nowe, zarezerwowane słowa
    - `callable` - nowy typ dla zmiennych,
    - `trait` (cecha) - specjalne konstrukcje podobne do klas, ale posiadające jedynie zestaw funkcji, które można następnie „wstrzyknąć” na etapie kompilacji do kodu każdej innej dowolnej klasy - coś jak wielokrotne dziedziczenie w C++. Po prostu klasa, która posiada (use - używa) danej cechy, może korzystać z jej funkcji(jednej lub wielu) 
```php
trait Konsumpcja {
    public function jedz{ ... }
}
class Paczek {
    use Konsumpcja ;
    // ...
}
class Kanapka extends Sniadanie {
    use Konsumpcja ;
    // ...
}
// metoda jest teraz dostępna dla obu klas
InstancjaPaczek ::jedz();
InstancjaKanapka::jedz();
```
    - `insteadof` - słowo kluczowe, które może przydać się, gdy stosujemy np.: cechy z funkcjami o tej samej nazwie
```php
class MyClass{ 
    use Cecha1, Cecha2 { 
        Cecha1:metoda insteadof Cecha2; 
        Cecha2:metoda as metodainna; 
    } 
}
```
- inne
    - Type hint dla typów prostych 
```php
class Foo
{
    public function bar(int $i)
    {
        // kod
    }
}
```
    - możliwość korzystania ze zmiennej $this w closure (funkcja anonimowa)
    - krótka forma definicji tablic `$tablica = ['jablko', '5.4', 'grucha', '0.0001'];`
    - dostęp do tablicy z poziomu funkcji/metody `echo funkcja()[2];`
    - możliwość wywołania callbacków jak funkcji 
```php
class Paczek {
    function zjedz(){
        echo 'paczek bedzie skonsumowany';
    }
}
$funkcja = array('Paczek', 'zjedz');
$funkcja();
```
    - binarna notacja dla liczb rzeczywistych 
```php
$bin = 0b0101010;
print_r($bin);
/*
Wynik: 42
 */
```
    - funkcja http_response_code zwracająca kod nagłówka 
```php
echo(http_response_code());
/**
Wynik: 200
 */
// Ustawianie kodu naglowka
http_response_code(404);
// Pobieranie kodu naglowka
echo(http_response_code());
/**
Wynik: 404
 */
```
    - obsługa wywołania własnej metody obiektu przy jego inicjalizacji
```php
class Paczek{
    function zjedz(){
        echo 'paczek bedzie skonsumowany';
    }
}
//PHP 5.3
$mniam = new Paczek();
$mnian->zjedz();
//PHP 5.4
(new Paczek)->zjedz(); 
```
    - funkcja `class_uses` zwraca używane przed daną klasę cechy 
```php
trait foo {}
trait log {}
trait foobar {}
class bar {
    use foo;
    use foobar;
}
print_r(class_uses(new bar));
print_r(class_uses('bar'));
/**
 * Result:
 *
Array
(
    [foo] => foo
    [foobar] => foobar
)
Array
(
    [foo] => foo
    [foobar] => foobar
)
*/
```
    - dodana opcja `JSON_PRETTY_PRINT`
```php
$obj = new stdClass;
$obj->field1 = "hello";
$obj->field2 = "world";
$obj->field3 = array("key" => "value", "level2" => array(1,2,3));
echo json_encode($obj);
echo json_encode($obj, JSON_PRETTY_PRINT);
/*output:
{"field1":"hello","field2":"world","field3":{"key":"value","level2":[1,2,3]}}
{
    "field1": "hello",
    "field2": "world",
    "field3": {
        "key": "value",
        "level2": [
            1,
            2,
            3
        ]
    }
}
 */
```
- obsługa iteratorów w MySQLi
```php
$mysqli = new mysqli('localhost', 'root', 'xxxx', 'piekarnia');
$result = $mysqli->query("SELECT * FROM piekarze LIMIT 10");
foreach($result as $piekarz){
    echo $piekarz['name'];
}
$mysqli->close();
```

- i wiele wiele innych...