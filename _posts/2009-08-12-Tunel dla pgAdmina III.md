---
title: Tunel dla pgAdmina III
date: 2009-08-12 09:12:54 +0100
categories: [database]
tags: [database,postgresql,pgadmin,vpn]
image:
  path: /assets/img/postgresql.png
  alt: tunel dla pgAdmina
author: michal_cwiklinski
toc: false
---

# Tunel dla pgAdmina III

W przypadku, gdy nie mamy bezpośredniego dostępu do postgresowej bazy danych, a mamy dostęp do shella, najprostszym rozwiązaniem jest utworzenie tunelu SSH.

Wystarczy jedno proste polecenie:
```bash
ssh -L 5555:localhost:5432 -l uzytkownik adres.do.serwera
```
następnie podać hasło - i można definiować połączenie w pgAdminie lub innym GUI dla postgresa.