---
title: 🇵🇱 Tablet jako centrum dowodzenia - Fully Kiosk i Home Assistant
date: 2025-08-24 08:29:04 +0100
categories: [smarthome]
tags: [smarthome,tablet,control center,dashboard,home-assistant]
image:
  path: /assets/img/fully-kiosk-ha.jpg
  alt: fully_kiosk
author: michal_cwiklinski
toc: false
---

# Twój tablet jako centrum dowodzenia: Fully Kiosk i Home Assistant

Chcesz stworzyć intuicyjny i zawsze aktywny panel do sterowania swoim inteligentnym domem? Zamiast kupować drogie, dedykowane urządzenia, możesz wykorzystać stary tablet z Androidem. Dzięki aplikacji **Fully Kiosk Browser** oraz integracji z **Home Assistant** stworzysz w pełni funkcjonalny, a do tego estetyczny system, który zawsze jest pod ręką.

### Krok 1: Instalacja Fully Kiosk Browser

Pierwszym krokiem jest pobranie i zainstalowanie aplikacji Fully Kiosk Browser na tablecie. Możesz to zrobić bezpośrednio ze sklepu **Google Play**. Wyszukaj "Fully Kiosk Browser" i zainstaluj aplikację.

Aplikacja ma darmową wersję, ale do pełnej funkcjonalności, takiej jak zdalny dostęp czy sterowanie czujnikami, potrzebna jest płatna licencja. Zdecydowanie warto ją wykupić, aby w pełni wykorzystać potencjał programu.

### Krok 2: Podstawowa konfiguracja Fully Kiosk

Po zainstalowaniu i uruchomieniu aplikacji, zobaczysz okno startowe. Na początku skonfiguruj najważniejsze opcje:

1.  **Ustawienie strony startowej**: W ustawieniach Fully Kiosk (dotknij ikony hamburgera w lewym górnym rogu), przejdź do **Web Content Settings** i w polu **Start URL** wpisz adres URL swojej strony z Home Assistant, np.: `http://127.0.0.1:8123/lovelace/home`.
2.  **Tryb pełnoekranowy**: Aby panel wyglądał profesjonalnie, włącz tryb pełnoekranowy, który ukryje górny pasek stanu i dolne przyciski nawigacyjne Androida. Znajdziesz to w **Kiosk Mode & Security**.
3.  **Wygaszanie i wybudzanie ekranu**: Fully Kiosk może zarządzać ekranem tabletu. W ustawieniach **Device Management** możesz skonfigurować automatyczne wyłączanie ekranu po braku aktywności (np. po 10 minutach) oraz wybudzanie go, gdy czujnik ruchu (jeśli tablet ma taką opcję) wykryje osobę w pobliżu.

### Krok 3: Integracja z Home Assistant

Aby Fully Kiosk mógł komunikować się z Home Assistant, musisz zainstalować integrację w HA.

1.  **W Home Assistant** przejdź do **Ustawienia > Urządzenia i usługi > Dodaj integrację**.
2.  Wyszukaj **"Fully Kiosk Browser"**.
3.  Podczas konfiguracji zostaniesz poproszony o podanie **adresu IP tabletu** oraz **hasła zdalnego dostępu**, które ustawisz w Fully Kiosk.
    * **W aplikacji Fully Kiosk** przejdź do **Remote Administration (ADMIN) > Enable Remote Administration**. Ustaw hasło i zapamiętaj adres IP tabletu.
4.  Po pomyślnym połączeniu Home Assistant wykryje tablet jako nowe urządzenie, a w panelu pojawi się nowa encja, np. `media_player.nazwa_tabletu`, oraz encje czujników, np. `sensor.nazwa_tabletu_battery_level`.

### Krok 4: Wykorzystanie w automatyzacjach

Po integracji z HA możesz tworzyć automatyzacje, które przeniosą Twój panel na wyższy poziom.

* **Wybudzanie ekranu**: Stwórz automatyzację, która będzie wybudzać ekran, gdy czujnik ruchu (np. z Home Assistant, Zigbee lub Z-Wave) wykryje ruch w pomieszczeniu.
    * **Trigger**: `when motion sensor detects motion`
    * **Action**: `call service fully_kiosk_browser.screen_on`
* **Zmiana widoku**: Możesz automatycznie przełączać widok na panelu, np. gdy włączysz alarm, Home Assistant może przełączyć widok na kamerę monitorującą.
    * **Trigger**: `when alarm is turned on`
    * **Action**: `call service fully_kiosk_browser.load_url`, a jako URL podaj adres widoku z kamerą.

Wykorzystując te funkcje, Twój panel stanie się nie tylko statycznym wyświetlaczem, ale interaktywnym centrum sterowania, które **reaguje na to, co dzieje się w Twoim domu**.

Instalacja i konfiguracja Fully Kiosk Browser to prosty, ale niezwykle efektywny sposób na stworzenie profesjonalnego panelu do sterowania inteligentnym domem.
