---
title: Generowanie kluczy - SSH bez hasła
date: 2008-08-16 09:12:54 +0100
categories: [linux]
tags: [linux,ubuntu,ssh,private key]
image:
  path: /assets/img/ubuntu.jpg
  alt: generowanie kluczy
author: michal_cwiklinski
toc: false
---

# Generowanie kluczy - SSH bez hasła

Wiem, wiem - prościzna, ale zazwyczaj o tym się nie pamięta. Więc trzeba sobie przypomnieć... Oto procedurka:

Na lokalnym komputerze (z którego będziemy się łączyć) generujemy dwa klucze: prywatny i publiczny:
```bash
ssh-keygen -t rsa
```
wygenerowane zostaną klucze id_rsa (prywatny) i id_rsa.pub (publiczny). Tego pierwszego radzę nikomu nie udostępniać.
 Kopiujemy klucz publiczny na serwer zdalny, z którym będziemy się łączyć:
```bash
scp /home/użytkownik/.ssh/id_rsa.pub użytkownik@zdalny_serwer:~/
```
Logujemy się na zdalny serwer i dodajemy klucz do autoryzowanych:
```bash
cat ~/id_rsa.pub >> ~/.ssh/authorized_keys
```
teraz wystarczy połączyć się ze zdalnych hostem poleceniem:
```bash
uzytkownik@zdalny_host
```
i już nie trzeba podawać hasła!
Oczywiście procedura jest jednostronna, i żeby tą samą operację wykonać ze zdalnego hosta, trzeba wszystko powtórzyć na tym hoście. 

P.S. Jak ktoś już ma klucz, i chce go użyć, może ustawić korzystanie z tego klucza, tworząc lub modyfikując plik `~/.ssh/config`, dodając treść:
```bash
IdentityFile ~/sciezka_do_klucza_prywatnego
```