---
title: Komunikacja obustronna PHP EKG2
date: 2010-11-05 09:12:54 +0100
categories: [php]
tags: [php,ekg2]
image:
  path: /assets/img/php.jpg
  alt: php ekg2
author: michal_cwiklinski
toc: false
---

# Komunikacja obustronna PHP EKG2

Pisząc różnego rodzaju serwisy, posiadające różne funkcjonalności, zdarzyło mi się pisać funkcjonalności związane z powiadamianiem użytkowników o wydarzeniach poprzez komunikatory. Oczywiście większość tego rodzaju komunikacji (strona WWW/skrypt PHP => komunikator) można załatwić klasami, które można napisać bądź znaleźć, ale komunikacja w drugą stronę jest już trochę trudniejsza. Ponieważ większość środowisk, w których pracują serwery WWW/kompilatory PHP, to środowiska linuksowe, można pokusić się o podpięcie to powyższej pracy konsolowego komunikatora, czyli EKG2.

Nie będę się rozpisywał o instalowaniu EKG2, gdyż to temat rzeka. Mogę tylko napisać, że w większości linuksowych dystrybucji serwerowych pakiet EKG2 jest dostępny w repozytorium. Ja osobiście korzystam z Debiana, gdzie EKG2 w najnowszej wersji jest dostępne w repozytorium w wersji _experimental_.

Do uruchomienia programu i jego działania w tle wykorzystuje oczywiście program screen.
```bash
screen ekg2
```

Po uruchomieniu musimy skonfigurować odpowiednio program. W zależności, którego protokołu chcemy używać (a parę mamy do wyboru: Gadu-Gadu, Jabber, Tlen, IRC) konfigurujemy konto. W przypadku najpopularniejszego w Polsce protokołu gg konfiguracja wygląda następująco:

- definiujemy numer GG:
```bash
/session -a gg:111111
```
- ustawiamy hasło:
```bash
/session password tajne_hasło
```
- zapisujemy ustawienia:
```bash
/save
```
- i teraz wystarczy połączyć się z serwerem:
```bash
connect
```

Aby połączyć nasz komunikator ze skryptami PHP musimy wykorzystać dwa pluginy dostępne w EKG2, a mianowicie obsługę skryptów Pythona (python) i obsługę zdalnej kontroli/potoków (rc).
Do komunikacji między komunikatorem a skryptami będziemy wykorzystywać plugin rc (Remote Control), a dokładnie jego obsługę potoków. Informacje będziemy przekazywać zapisując je bezpośrednio do pseudopliku (tzw. pipe), który konfigurujemy przy ładowaniu wtyczki.
Następnie musimy stworzyć sobie bardzo prosty skrypt napisany w Pythonie, który będzie odpalał skrypt shellowy, gdzie myślę że większość PHP'owców czuję się lepiej niż w Pythonie (mówię za siebie).
Oto używany przeze mnie skrypt:

```python
#-*- coding: utf-8 -*-
import ekg
import string
import os
wersja="0.02"
#Gdy ladujemy skrypciora
def init():
ekg.printf("generic","Zaladowano Bota v %s"%wersja)
return 1
#Gdy wywalamy skrypciora
def deinit():
ekg.printf("generic","Wywalono Bota ")
return 1
def message_handler(session, uid, type, text, sent_time, ignore_level):
gg=string.replace(uid,'gg:','')
script="/sciezka_do_skryptu_shellowego/bot.sh "+gg+" '"+text+"'"
os.system(script)
ekg.printf("generic","status zapisania: "+script
)
ekg.handler_bind('protocol-message-received', message_handler)
```

Skrypt ten zapisujemy/kopiujemy do katalogu konfiguracyjnego EKG2 i podkatalogu ze skryptami (`~/.ekg2/scripts`). Teraz wystarczy załadować wtyczki i skrypt, wykonując kolejno polecenia:
```bash
/plugin +rc
/set rc:remote_control pipe:/sciezka_do_pipe'a/pipe.ekg2
/plugin +python
/python:load bot
/save
```

Teraz wystarczy obsłużyć zapisywanie do pipe'a w PHP - i mamy obustronną komunikację!