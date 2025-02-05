---
title: Przesiadka na Kohanę 3.0
date: 2009-08-13 09:12:54 +0100
categories: [php]
tags: [php,kohanaphp,framework]
image:
  path: /assets/img/php.jpg
  alt: kohanaphp 3.0
author: michal_cwiklinski
toc: true
---

# Przesiadka na Kohanę 3.0

Jeżeli ktoś śledzi na bieżąco zmiany we framework'u Kohana przez ostatnie pół roku, zdążył zapewne zauważyć, że projekt ciągle się rozwija. Ciągle wydawane są nowe wersję, najnowsza jest już na horyzoncie. Jednak dużym skokiem jest ciągle rozwijana wersja 3 framework'a. Architektura w tej wersji została zmieniona, co skutkuje faktem, że przeniesienie aplikacji z wersji 2.x do 3 będzie wymagało przepisywania kodu. Jakich zmian dokonano w wersji 3?

## MVC vs HMVC
Kohana3 została napisana w zupełnie innej architekturze. Model HMVC (Hierarchical-Model-View-Controller) jest bardzo podobny do swojego poprzednika, ale oczywiście jako nowszy wprowadza kilka ulepszeń i nowości.
Poprzednie wersje Kohany przetwarzały zapytanie poprzez router, przekierowując je do odpowiedniego Kontrolera i Metody, przekazując również dodatkowe parametry. Kontroler przetwarzał zapytanie, i wysyłał odpowiedź do klienta, wszystko odbywało się w bardzo prosty liniowy sposób.
W modelu HMVC pojedynczy zapytanie może utworzyć kilka innych zapytań w ramach systemu, przy wykorzystaniu klasy Request. Dzięki temu programista ma ogromne możliwości, zapewniając aplikacji wysoce modularną budowę. Wykorzystanie całego potencjału HMVC wymaga zrozumienia jego [budowy](http://en.wikipedia.org/wiki/Presentation-abstraction-control).
Warto tutaj nadmienić, że wykorzystanie HMVC w Kohanie3 jest całkowicie opcjonalne, i można również jak dotychczas korzystać z tradycyjnego modelu MVC.

## Bootstrap, konfiguracja i zdarzenia
Bootstrap w Kohanie został uwolniony z folderu systemowego. Powodem tego było to, że plik ten konfiguruje większość głównych ustawień aplikacji, mając na uwadze to, że poprzednio były one ustawiane w głównym pliku konfiguracyjnym. Użytkownicy Zend Framework'a znają już takie praktyki.
Bootstrap jest odpowiedzialny za ustawienie środowiska Kohany, zdefiniowanie domyślnego auto-loadera, ustawienie cache'owania, ustawienie kodowania, profilowanie, logowanie i ładowanie sterowników oraz modułów, routing i w końcu wykonanie zapytania.
Programiści mogą dodawać własny kod do bootstrapa, modyfikując główne operacje systemowe. To zapewnia bardzo elastyczne warstwę konfiguracji dla aplikacji, otwierając dla modyfikacji proces inicjalizacji całego systemu. Dodatkowo, każdy moduł aplikacji ma swojego bootstrapa, umożliwiają modułom rekonfigurację systemu wg potrzeb.

## Struktura klas
Kohana3 wprowadza odświeżoną strukturę klas, która powoduje, że przenosząc aplikację z wersji 2.x trzeba będzie wprowadzić kilka modyfikacji. Stare katalogi libraries, helpers i controllers znikają. Zastępuję je folder classes.
Zmieniony został również sposób, w jaki Kohana autoładuje pliki. Wszystkie klasy znajdują się w nowym folderze classes w strukturze systemu, aplikacji i modułów. Została użyta nowa konwencja nazewnicza, która uproszcza identyfikację i umiejscowienie klas. Wszystkie klasy używają podkreślenie _ rozdzielającego poszczególne człony nazwy tak jak to było w wersji poprzedniej, ale łatwo zauważyć różnicę.
Teraz nazwy klas są definiowane przez ich lokalizację, a nie przez typ. Dla przykładu, dawny _Welcome_Controller_ stał się teraz _Controller_Welcome_. Dzieje się tak dlatego, że kontroler Welcome znajduje się w folderze classes/controller w strukturze aplikacji (/application/classes/controller/welcome.php). Główne klasy Kohany mają przedrostek Kohana (/system/classes/kohana/validate.php). Pozostały również poprzednie klasy, więc można używać zamiennie _Validate::factory()_, jak i _Kohana_Validate::factory()_ (to pierwsze jest oczywiście krótsze i szybsze do użycia). Ta praktyka jest również znana użytkwonikom Zend Framework'a. 
Definicja bibliotek i helperów nie została porzucona, ale oba rodzaje klas zostały ujednolicone. Dobrym tego przykładem jest biblioteka _Validation_ (/system/libraries/Validation.php) i helper _Valid_ (/system/helpers/valid.php). Zastępuje je teraz nowa klasa walidacji (/system/classes/validate.php), która zawiera metody z obu powyższych klas.
Kontrolery aplikacji znajdują się teraz w folderze kontrolerów (/application/classes/controller).

Na podstawie [Moving to Kohana 3 - Part One](http://sam.clark.name/2009/08/11/moving-to-kohana3-part-one/)