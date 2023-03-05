---
title: Vista i łączenie z linuksowym DHCP
date: 2008-09-03 09:12:54 +0100
categories: [linux]
tags: [linux,windows,ubuntu,dhcp]
image:
  path: /assets/img/start-blogowania.jpg
  alt: start blogowania
author: michal_cwiklinski
toc: false
---

# Vista i łączenie z linuksowym DHCP

Chcąc nie chcąc, Vista wchodzi do domów i do biur. Szczególnie tych, w których kupuje się laptopy. Niby wszystko ok, ale jak zwykle pojawiają sie problemy. Tym razem problemem było otrzymanie przez laptopy z Vistą adresu IP z serwera, który stał na Linuksie.

System łączył sie z Access Pointem (połączenie bezprzewodowe), i nawet z serwerem (broadcast), serwer otrzymywał zapytanie DHCP, ale Vista nie przyjmowała ustawień. Okazało się, że ten wspaniały system i jego klient DHCP nie uznaje "innych niż Microsoft'owe" serwerów DCHP, i trzeba wyłączyć w rejestrze odpowiednią flagę dla rozgłoszeń broadcastowych. Całą sytuacje opisuje jeden z artykułów na stronie Wsparcia technicznego Microsoftu.

Aby uniknąć w przyszłości tych problemów, należy:

1. Włączyć edytor rejestru (Start -> Uruchom -> regedit)
2. Znaleźć klucz `HKEY_LOCAL_MACHINESYSTEMCurrentControlSetServicesTcpipParametersInterfaces{GUID}`, gdzie `{GUID}` to id karty sieciowej (choć najlepiej dodać ten klucz we wszystkich)
3. Dodać nową wartość `DWORD 32-bit`
4. Jako nazwę klucza podać `DhcpConnEnableBcastFlagToggle`
5. Wartość ustawić na `1`
6. Wyłączyć edytor rejestru.

Od tej pory Vista powinna bez problemu otrzymywać adres IP z serwerów DHCP.