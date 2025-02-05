---
title: IntelliJ IDEA i wiele maszyn wirtualnych na VirtualBox'ie
date: 2012-10-19 09:12:54 +0100
categories: [android]
tags: [android,intellij,virtualbox]
image:
  path: /assets/img/intellij-idea-welcome-screen.png
  alt: intellij idea
author: michal_cwiklinski
toc: false
---

# IntelliJ IDEA i wiele maszyn wirtualnych na VirtualBox'ie

W ostatnim poście napisałem, jak utworzyć wiele maszyn wirtualnych. Spoko, maszyn wirtualnych mamy kilka - ale jak skomunikować to z IDE, na którym pracujemy. Na pewno jedna będzie chodzić, a  co z resztą? Za każdym razem wyłączać i włączać nową? Zacznijmy może od tego, że wszystkie maszyny muszą być odpowiednio skonfigurowane. Skonfigurować je można na dwa sposoby. Chodzi o konfigurację karty sieciowej na wirtualnej maszynie. 

Pierwszy sposób to ustawienie karty sieciowej w tryb bridged.

Adres IP zostanie pobrany z tego samego serwera DHCP, z którego dostajemy IP dla naszego komputera. Aby się połączyć z maszyną, musimy wydać polecenie:
```bash
adb connect adres.ip.naszej.maszyny
```
Adres maszyny można sprawdzić poprzez Alt+F1 i wpisanie polecenia `netcfg`. 

Drugim sposobem (polecanym przeze mnie) jest skorzystanie z emulowania karty sieciowej w trybie NAT. Jest on o tyle lepszy, że nie trzeba się łączyć z maszyną przez ADB, oraz maszyna jest widziana jako emulator. Ale wymaga to dodatkowego przekierowania portów.
 
I o to właśnie przekierowanie portów chodzi, jeżeli chcemy w oknie dialogowym uruchamiania aplikacji na urządzeniu bądź emulatorze:
![Intellij IDEA - porty](/assets/img/intellij-idea-run.jpg){: w="200" }
 
Chodzi o to, że ADB wymaga dwóch portów do komunikacji z emulatorem. W przypadku jednej maszyny, jest to port 5555 (widoczny na konfiguracji połączenia NAT) i poprzedzający go 5554 (stąd widoczna nazwa maszyny emulator-5554). Więc aby ADB widział wszystkie maszyny wirtualne, trzeba każdej następnej maszynie dodawać wartość dwa do portu przekierowującego port w naszym komputerze na port 5555 wirtualnej maszyny, czyli komputer: 5557 - vm1: 5555, komputer 5559 - vm2: 5555, itd.