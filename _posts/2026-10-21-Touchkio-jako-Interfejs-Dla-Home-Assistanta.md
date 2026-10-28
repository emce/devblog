---
title: 🇵🇱 Touchkio jako interfejs dla Home Assistant
date: 2026-10-21 09:00:00 +0100
categories: [smarthome]
tags: [smarthome,integration,home-assistant,frontend,linux]
image:
  path: /assets/img/touchkio_display.png
  alt: Touchkio działający jako interfejs dla Home Assistant
author: michal_cwiklinski
toc: false
---

Korzystam z Home Assistant już od kilku lat. Domyślny interfejs Lovelace jest w porządku – sprawdza się na telefonie, laptopie czy tablecie zamontowanym na ścianie. Jednak w salonie mam również dedykowany ekran, który działa non-stop. Wyświetlanie na nim pełnego panelu Home Assistant ze wszystkimi dostępnymi widżetami to przesada: zbyt wiele menu, zbędne elementy interfejsu, a przy pełnym powiększeniu na małym ekranie całość wygląda po prostu niezbyt estetycznie.

Z pomocą przychodzi [Touchkio](https://github.com/leukipp/touchkio). To aplikacja typu „kiosk” oparta na technologii Electron, która prezentuje Home Assistant w przejrzystym, zoptymalizowanym pod kątem ekranów dotykowych oknie. Narzędzie obsługuje logowanie, zapamiętuje dane dostępowe, a w połączeniu z rozszerzeniem „Kiosk Mode” (dostępnym przez HACS) pozwala uzyskać widok pełnoekranowy. Co więcej, Touchkio udostępnia parametry samego urządzenia – takie jak użycie procesora, pamięci, temperaturę czy stan zasilania ekranu – przesyłając je do Home Assistant za pośrednictwem protokołu MQTT.
![](/assets/img/touchkio_mqtt.png){: .right .MQTT Panel}

## Czego to jest?

Touchkio to plik `.deb` (dla Raspberry Pi i innych systemów opartych na Debianie) lub `.zip`. Nie uruchamiasz go w trybie headless (bez grafiki) — potrzebne jest środowisko graficzne (desktop) na Raspberry Pi OS (nie "Lite"). Aplikacja ładuje Twoją stronę Home Assistant w trybie kiosk i dodaje wszystkie funkcje niezbędne do wyświetlacza ściennego: pamiętanie logowania, nawigację, kontrolę jasności, klawisze na eknie.

## Hardware, którego używam

| Komponent | Co mam | Uwagi |
|-----------|--------|-------|
| **Płyta** | Raspberry Pi 4 Model B, 4 GB | Pi 3 też działa, ale Electron jest "ciężki"; 4 GB zalecane |
| **Wyświetlacz** | Waveshare 7" HDMI touchscreen (1024×600) | DSI lub HDMI dotyk; USB-HID do dotyku |
| **System** | Raspberry Pi OS (64-bit) **z desktopem** | labwc (Wayland) zalecane; X11 też działa |
| **Zasilanie** | Oficjalne 5V 3A USB-C | Niskie napięcie psuje dotyk |
| **Obudowa** | Druk 3D z mocowaniem VESA | Utrzymuje porządek z kablami |

**Minimum do startu**: Pi 4 + monitor HDMI/DSI z dotykiem + Raspberry Pi OS z desktopem (nie Lite). Electron potrzebuje compositora. Pi 3 *może* działać, ale będzie przykry — Electron + Chromium zjada pamięć RAM.

## Setup (co faktycznie zadziałało)

Oficjalna dokumentacja zakłada Raspberry Pi OS z desktopem (labwc preferowane). Oto dokładna sekwencja:

```bash
# 1. Raspberry Pi OS (64-bit) Z DESKTOPEM — nie Lite
#    Włącz SSH, ustaw lokalizację, hostname itp. przez raspi-config

# 2. Aktualizacja
sudo apt update && sudo apt upgrade -y

# 3. Instalacja Touchkio .deb (sprawdź GitHub releases za najnowszy adres URL)
wget https://github.com/leukipp/touchkio/releases/latest/download/touchkio_1.0.0_arm64.deb
sudo apt install ./touchkio_1.0.0_arm64.deb

# 4. Pierwsze uruchomienie — uruchamia kreator konfiguracyjny
touchkio
#   - Wpisz adres Home Assistant (http://homeassistant.local:8123)
#   - Wpisz dane brokera MQTT (opcjonalnie, ale polecam)
#   - Zaloguj się do Home Assistant przez UI, które się otworzy
#   - Dane zapisane do ~/.config/touchkio/

# 5. Włączenie auto-start (usługa systemd użytkownika)
systemctl --user enable touchkio
systemctl --user start touchkio

# 6. Enable linger, żeby usługa startowała przy restarcie bez logowania
sudo loginctl enable-linger pi
```

To tyle. Przy starcie startuje się desktop (labwc), Touchkio uruchamia się automatycznie w trybie kiosk i jesteś na dashboardzie Home Assistant.

## MQTT (dlaczego to przydatne)

Po skonfigurowaniu z MQTT Touchkio publikuje te sensory do Home Assistant:

| Sensor | Użycie |
|--------|--------|
| `sensor.touchkio_cpu_usage` | Alarm przy przegrzewaniu/throttlingu Pi |
| `sensor.touchkio_memory_usage` | Wyłapanie wycieków pamięci |
| `sensor.touchkio_temperature` | Monitorowanie termicznego throttlingu |
| `sensor.touchkio_display_brightness` | Zna poziom jasności |
| `sensor.touchkio_display_power` | Czy ekran jest włączony/wyłączony? |
| `sensor.touchkio_battery` | Dla konfiguracji UPS/HAT |
| `sensor.touchkio_network_*` | Adres IP, status interfejsu |
| `sensor.touchkio_upgrades` | Dostępne aktualizacje apt |

I akceptuje te polecenia przez MQTT:

```json
// Wyłącz ekran
{ "command": "display_power", "value": "off" }

// Ustaw jasność 0-100
{ "command": "display_brightness", "value": 50 }

// Przełącz klawiszową klawiaturę na ekranie
{ "command": "keyboard_toggle" }

// Nawiguj do konkretnego dashboardu
{ "command": "navigate", "url": "/dashboard/energy" }

// Zrestartuj Pi
{ "command": "reboot" }
```

Więc Twoje automatyzacje Home Assistant mogą przyciemnić wyświetlację w korytarzu o nocy, włączyć ją na ruch, zdalnie zrestartować uwięzionego kioska lub przełączać podglądy w zależności od godziny.

## Praktyczne scenariusze użycia

### Wracasz do domu z zakupami w rękach
Zakupy w obu rękach. Wyświetlacz w przedpokoju już świeci — czujnik ruchu uruchomił Home Assistant poprzez MQTT `display_power: on`. Klikasz "Kitchen outlets" na ekranie przy drzwiach — podszafkowe oświetlenie się zapala. Bez telefonu, bez komendy głosowej. Po drodze do kuchni.

### Wieczór filmowy (jeden klik)
Boczny panel pozwala mi przewijać się między dashboardami. Mam ustawiony dashboard "Scen" z dużymi kafelekiem. Klikam "Movie night" — uruchamia scenę Home Assistant: przyciemnia światła do 15%, włącza podświetlenie za telewizorem, ustawia wejście TV na HDMI 2, opuszcza rolety. Jeden klik z kuchni, gdy biorę napoje.

### Sprawdzenie przed snem
Stoisz w przedpokoju i patrzysz na ekran. Kitchen outlets — wyłączone. Living room TV — wyłączone. Front door — zdjęcie z kamery pokazuje światło na ganku, nikogo. Klikasz kafelek "Bedtime" — zabija wszystkie przełączniki, uzbraja alarm, termostat na tryb nocny. Następnie boczny panel → "Display off" — ekran gaśnie.

### Monitorowanie poboru energii
Dashboard "System" pokazuje własne Pi CPU, RAM, temperaturę — plus pobór energii z całego domu. Gdy pompa ciepła startuje, widzę 2,8 kW na ścianie. Gdy suszarka pracuje — 2,1 kW. To nie jest szczegółowy dashboard energii — tylko liczba. Ale widoczna na ścianie sprawia, że faktycznie zauważam, kiedy coś pobiera energię, której nie powinno.

### Tryb gości
Znajomi zostają na noc. Nie dajesz im aplikacji Home Assistant. Mówisz: "Ekran przy drzwiach steruje światłem." Rozumieją to w 10 sekund. Boczny panel jest ukryty do momentu przesunięcia palcem — domyślnie czysty.

### Zdalne naprawianie
Ekran zawiesza się? SSH i `systemctl --user restart touchkio`. Albo wyślij polecenie MQTT `reboot` z Home Assistant. Albo sprawdź `sensor.touchkio_memory_usage` — jeśli wynosi 95%, wiesz dlaczego.

## Co brakuje (i jak sobie radzę)

| Brak | Obejście |
|------|----------|
| Brak native auth w kiosku — każdy z dostępem fizycznym może go używać | Zamontuj wysoko, albo w pokoju, do którego goście nie mają dostępu |
| Brak wielu profili użytkowników w samym kiosku | To wspólny wyświetlacz domowy — o to chodzi |
| Electron = cięższe niż "raw" binarny (~300-500 MB RAM) | Pi 4 4 GB radzi sobie; Pi 3 się przytula |
| Zmiana konfiguracji wymaga restartu aplikacji | `systemctl --user restart touchkio` trwa 3 sekundy |
| Pierwsze logowanie potrzebuje klawiatury/myszy/VNC | Jeden raz tylko; po tym dane są zapisane w pamięci |

## Problemy, które napotkałem

- **Współrzędne dotyku przesunięte**: Raspberry Pi OS ma `xinput_calibrator` (X11) lub konfigurację libinput (Wayland). Uruchom raz, zapisuje trwale.
- **Ekran nie śpi**: Sprawdź sensor MQTT `display_power` — jeśli jest "zaklęty" w "on", sprawdź połączenie z brokerem MQTT.
- **Electron GPU issues na Wayland**: Dodaj `--enable-features=UseOzonePlatform --ozone-platform=wayland` do flag uruchamiania (w `.config/touchkio/config.json`).
- **MQTT rozłącza się**: Ustaw `mqtt.keepalive: 30` w config; broker MQTT Home Assistant to obsługuje.
- **Dźwięk nie działa**: PipeWire/PulseAudio działa "out of the box" na Pi OS desktop. Głośność kontrolowana przez MQTT `volume_set` command.
- **Aktualizacje**: `.deb` przez `apt` — `sudo apt update && sudo apt install touchkio` (zachowuje konfigurację).

## Czy to ma sens?

Jeśli masz Pi 4 i ekran dotykowy (lub monitor + mysz) — tak. Cały setup kosztował mnie ok. 120 € (Pi 4 4GB + Waveshare 7" dotyk + obudowa + zasilacz). Komercyjny smart display jak Google Nest Hub Max to 200 €+ i zamyka Cię w ich ekosystemie. To działa lokalnie, MQTT w obie strony, a urządzenie jest Twoje.

Przeciążenie Electrona jest prawdą — ok. 400 MB RAM vs ok. 30 MB dla "raw" binarnego Go — ale na Pi 4 z 4 GB to nie problem. Za to dostajesz kiosk, który *czuje się* jak urządzenie użytkowe. Logowanie persystuje. Ekran śpi/wstawia się. Jasność kontrolna. Czujniki urządzenia w HA. Boczny panel nawigacji bez wychodzenia z pełnego ekranu.

Jeśli już jesteś głęboko w Home Assistant i chcesz dedykowany wyświetlacz na ścianie, który zachowuje się jak termosostat — zawsze włączony, zawsze gotowy, bez tarć — Touchkio to najbardziej dopracowany "owijak", jaki znalazłem. To nie jest budowniczy dashboardów. To Home Assistant, urobiony kiosk-native.
