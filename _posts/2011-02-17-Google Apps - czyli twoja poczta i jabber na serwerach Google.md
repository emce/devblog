---
title: Google Apps - czyli twoja poczta i jabber na serwerach Google
date: 2011-02-17 09:12:54 +0100
categories: [internet]
tags: [internet,google,google apps,email,jabber]
image:
  path: /assets/img/google-apps.jpg
  alt: google apps
author: michal_cwiklinski
toc: false
---

# Google Apps - czyli twoja poczta i jabber na serwerach Google

Usługa ta jest dostępna od dawna, choć od niedawna dopiero zaczynam ją doceniać. Postawiłem przemigrować wszystkie moje i "moje" serwisy na Google. Oczywiście pisząc serwisy mam na myśli konta pocztowe. Bo po co się bawić w konfigurację serwera pocztowe, filtrów antyspamowych, jak Google zrobi to za mnie??

1. **APLIKACJA** - aby całość wystartowała, musimy najpierw dodać naszą domenę do systemu aplikacji Google. Pod adresem [google.com/a/cpanel/domain/new](https://www.google.com/a/cpanel/domain/new) definiujemy naszą domenę, następnie ją weryfikujemy, i możemy ustawiać pocztę i jabbera.
2. **POCZTA** - sprawa jest prosta - dodajemy odpowiednie wpisy do DNS, a następnie w panelu administracyjnym Google definiujemy sobie konta użytkowników. Dodatkowo istnieje możliwość zaimportowania wiadomości (tylko należy to zrobić przed rozpropagowaniem DNS'ów, bądź gdy możemy się z naszym starym serwerem IMAP połączyć po adresie IP). Dobrą sprawą jest też zrobienie sobie przekierowania poprzez rekord CNAME w DNS'ie z adresu w naszej domenie na adres GMaila naszej aplikacji, np.: z [poczta.cwiklinski.it](https://poczta.cwiklinski.it).
3. **JABBER** - tutaj podobnie jak z pocztą - wystarczy dodać parę rekordów SRV do naszego DNS'a (problemem może być fakt, że nasz hosting provider nie udostępnia możliwości dodawania tego typu rekordów), a następnie skonfigurowanie kont w panelu zarządzania aplikacją - wystarczy włączyć użytkownikom możliwość korzystania z Google Talk (patrz screen poniżej). Mała wskazówka - nie wszystkie klienty jabbera prawidłowo łączą się z kontami "aplikacyjnymi" Google (z normalnymi nie ma problemów) - należy wtedy ręcznie ustawić nazwę hosta na _talk.google.com_.