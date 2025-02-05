---
title: Skrypt do ściągania z Rapidshare.com (Premium)
date: 2008-01-02 09:12:54 +0100
categories: [linux]
tags: [linux,rapidshare,download]
image:
  path: /assets/img/ubuntu.jpg
  alt: skrypt Rapidshare
author: michal_cwiklinski
toc: false
---

# Skrypt do ściągania z Rapidshare.com (Premium)

Szukałem czegoś fajnego, co by nie siedzieć i nie patrzeć, jak mi się powoli (oczywiście ze względu na ilośc, a nie prędkość łącza) pliki ściągają, tylko zapuścić na serwerze i niech się nocami podczas mojego snu ściąga - no i znalazłem! Na jednej z czeskich linuksowych stron ktoś umieścił skrypcik wykorzystujący WGet'a - trochę niebezpieczny, bo login i hasło do konta Premium wysyła w GET'cie, ale co tam - lenistwo przewyższyło rozsądek...

Skrypt został tak przygotowany, że można dodawać kilka linków do ściągnięcia na raz (ja testował ponad 50 linków i działało). Oto kod powyższego skryptu:

```bash
#!/bin/bash
# (v0.1)
#
# Przykład:
# ./rdown link1 link2...
# 

USERD=""  # tutaj katalog, gdzie plik ma zostać umieszczony
PUSER=""   # nazwa użytkownika Premium Rapidshare
PPASS=""   # hasło użytkownika Premium Rapidshare

echo "Plików dodanych do ściągania: $# "

if [ "$1" == "" ]; then
    echo "Nie dodano nic do ściągania"
    exit 1
fi
if [ -r "$1" ]; then
    for link in `cat $1`; do
	wget --http-user=$PUSER --http-passwd=$PPASS $1 -P $USERD
    done
else
    while [ "$1" != "" ]; do
	wget --http-user=$PUSER --http-passwd=$PPASS $1 -P $USERD
	shift
    done
fi
```

Dodając do tego odpowiedni skypt PHP można sobie na swoim domowym serwerku ustawić "maszynę do ściągania".