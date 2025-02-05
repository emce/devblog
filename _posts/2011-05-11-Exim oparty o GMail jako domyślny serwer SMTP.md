---
title: Exim oparty o GMail jako domyślny serwer SMTP
date: 2011-05-11 09:12:54 +0100
categories: [linux]
tags: [linux,smtp,exim,gmail]
image:
  path: /assets/img/php-mailing.jpg
  alt: exim oparty o gmail
author: michal_cwiklinski
toc: false
---

# Exim oparty o GMail jako domyślny serwer SMTP

Wysyłanie maili jest jedną z podstawowych funkcjonalności wszelakiej maści aplikacji, pisanych w różnych językach. W przypadku PHP również. Oczywiście kodopisanie zazwyczaj odbywa się nie na serwerze, na którym domyślnie aplikacja będzie się znajdować, a na komputerze programisty. Stawiamy Linuksa, Apacze z PHP, i jazda. A co z mailem?

Oczywiście można pokusić się o postawienie całego serwera poczty, ale pytanie - po co? Bo chcemy przetestować wydajność naszego leciwego laptopa?

Najprostszym rozwiązaniem jest oczywiście postawienie jakiegoś lekkiego demona smtp, który będzie wykorzystywał zewnętrzne konto pocztowe. Takową hybrydą jest połączenie lekkiego Exima z GMailem.

Instalujemy demona poleceniem:
```bash
apt-get install exim4-daemon-light
```

Następnie przechodzimy do konfiguracji:
```bash
dpkg-reconfigure exim4-config
```

Pokrótce poszczególne opcje konfiguracyjne:

- poczta wysyłana przez pośrednika: _otrzymywana przez SMTP lub fetchmail_
- Nazwa pocztowa systemu: _domain.com_
- Adresy IP, na których nasłuchiwać nadchodzących połączeń: _zostawiamy bez zmian_
- Inne systemy docelowe dla których poczta jest przyjmowana:  _puste_
- Komputery dla których przekazywać pocztę: _puste_
- Adres IP lub nazwa pośrednika przyjmującego wychodzącą pocztę: _smtp.gmail.com::587_
- Ukrywać lokalną nazwę w wychodzącej poczcie? _Nie_
- Utrzymywać ilość zapytań DNS na minimalnym poziomie (dzwonienie na żądanie)? _Nie_
- Format dostarczania poczty lokalnie: _format mbox w /var/mail/_
- Podzielić konfigurację na małe pliki? _Nie_
- Odbiorca poczty dla kont root i postmaster: _puste_

Następnie edytujemy plik szablonu Exima: 
```bash
gedit /etc/exim4/exim4.conf.template
```
znajdujemy linię z tekstem
```bash
.ifdef DCconfig_smarthost DCconfig_satellite
```
a następnie dodajemy w tej sekcji:
```
send_via_gmail:
       driver = manualroute
       domains = ! +local_domains
       transport = gmail_smtp
       route_list = * smtp.gmail.com
```
znajdujemy linię z tekstem  
```
begin authenticators
```
a następnie dodajemy w tej sekcji:
```
gmail_login:
       driver = plaintext
       public_name = LOGIN
       client_send = : nazwaTwojegoKonta@gmail.com : TwojeGMailoweHasło
```
znajdujemy linię z tekstem 
```
transport/30_exim4-config_remote_smtp_smarthost
```
a następnie dodajemy w tej sekcji:
```
gmail_smtp:
       driver = smtp
       port = 587
       hosts_require_auth = $host_address
       hosts_require_tls = $host_address
```

Aby autentykacja przeszła prawidłowo, edytujemy plik
```bash
/etc/exim4/passwd.client
```
dodając tekst:
```
Gmail-smtp.l.google.com:nazwaTwojegoKonta@gmail.com:tw0j3H4sU0
*.google.com:nazwaTwojegoKonta@gmail.com:tw0j3H4sU0
smtp.gmail.com:nazwaTwojegoKonta@gmail.com:tw0j3H4sU0
```

Teraz aktualizujemy konfigurację Exima:
```bash
update-exim4.conf
```
oraz restarujemy sam demon:
```bash
/etc/init.d/exim4 restart
```
Czasami zdarza się sytuacja, że maila nie wychodzą, a logu Exima można znaleźć takie rekordy:
```
R=send_via_gmail T=gmail_smtp defer (-53): retry time not reached for any host
```
Aby rozwiązać ten problem, należy zalogować się na używane konto przez przeglądarkę, a następnie wydać polecenie
```bash
exim -qff
```

Wszystkie skolejkowane wiadomości powinny zostać wysłane.