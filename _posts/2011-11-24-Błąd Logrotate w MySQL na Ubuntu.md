---
title: Błąd Logrotate w MySQL na Ubuntu
date: 2011-11-24 09:12:54 +0100
categories: [database]
tags: [database,mysql,ubuntu,debian,cron]
image:
  path: /assets/img/mysql-ubuntu.jpg
  alt: bład logrotate mysql
author: michal_cwiklinski
toc: false
---

# Błąd Logrotate w MySQL na Ubuntu

Od pewnego czasu dostaję codziennie w logach następujący błąd:
```bash
/etc/cron.daily/logrotate:

error: error running shared postrotate script for '/var/log/mysql.log /var/log/mysql/mysql.log /var/log/mysql/mysql-slow.log '

run-parts: /etc/cron.daily/logrotate exited with return code 1
```

Dzieje się tak dlatego, że po wszystkich procesach archiwizacji, jak wszystko zostanie już zrobione, Ubuntu próbuje użyć użytkownika `debian-sys-maint` do wyczyszczenia logów, a wywoływane jest to w `/etc/logrotate.d/mysql-server`. I tu jest pies pogrzebany - ten użytkownik nie jest używany w Ubuntu.
Rozwiązaniem jest wygrzebanie z pliku `/etc/mysql/debian.cnf` hasła wyżej wspomnianego użytkownika, i dodanie mu odpowiednich uprawnień w bazie. Plik `/etc/mysql/debian.cnf` powinien mieć mniej więcej taką strukturę:
```
# Automatically generated for Debian scripts. DO NOT TOUCH!

[client]
host     = localhost
user     = debian-sys-maint
password = xxxxxxxxxxxxxxxx
socket   = /var/run/mysqld/mysqld.sock
[mysql_upgrade]
user     = debian-sys-maint
password = xxxxxxxxxxxxxxxx
socket   = /var/run/mysqld/mysqld.sock
basedir  = /usr
```

Zapytanie dodające uprawnienia wygląda następująco:
```sql
GRANT RELOAD, SHUTDOWN, PROCESS, SHOW DATABASES, SUPER, LOCK TABLES ON *.* TO 'debian-sys-maint'@'localhost' IDENTIFIED BY PASSWORD 'xxxxxxxxxxxxxxxx'
```

gdzie `xxxxxxxxxxxxxxxx` to hasło z pliku `/etc/mysql/debian.cnf`.
Po dodaniu uprawnień w logach zniknął wspomniany błąd.