---
title: Instalacja SDK, ADB i Fastboot w Ubuntu
date: 2011-04-18 09:12:54 +0100
categories: [android]
tags: [android,linux,adb,fastboot]
image:
  path: /assets/img/android-sdk.jpg
  alt: android sdk
author: michal_cwiklinski
toc: false
---

# Instalacja SDK, ADB i Fastboot w Ubuntu

SDK Androida zawiera kilka fajnych narzędzi, dzięki którym można ustawić, a także w pełni wykorzystać swój telefon z tym system na pokładzie. Instalacja tego zestawu programów w Windowsie jest prosta, w Linuksie sprawa jest trochę bardzie skomplikowana.

1. Instalacja Sun Java JDK
ADB do działania wymaga środowiska Java, najbardziej zgodne jest to wydane przez Sun, choć z OpenJDK też nie powinno być problemów. W Ubuntu Natty (11.04) pakiety dostępne są w repozytorium, w niższych wersjach trzeba dodać repozytorium:
```bash
sudo add-apt-repository ppa:sun-java-community-team/sun-java6
sudo apt-get update
sudo apt-get install sun-java6-jre sun-java6-bin sun-java6-jdk
```

2. Pobranie i instalacja SDK Androida
Ze strony [developer.android.com](http://developer.android.com/sdk/index.html) pobieramy paczkę dla Linuksa (_android-sdk_r10-linux_x86.tgz_). Paczkę należy rozpakować, najlepiej do katalogu użytkownika, zalecane jest również nie zmieniać nazwy katalogu. Po rozpakowaniu wydajemy w konsoli polecenia:
```bash
cd ~/android-sdk-linux_x86/tools
./android update sdk
```
Otworzy sie okno _Android SDK and AVD Manager_. Jeżeli ktoś jest zainteresowany programowaniem dla Androida, wybiera odpowiedni paczki, dla normalnego użytkownika wystarczy zaznaczyć pierwszą opcję.

Po ściągnięciu wszystkich pakietów i zainstalowaniu ich, a także restarcie serwera ADB, w celu usprawnienia późniejszych prac można ustawić ścieżki do SDK i poszczególnych programów, aby nie wpisywać pełnych:
```bash
export PATH=${PATH}:~/android-sdk-linux_x86/tools
export PATH=${PATH}:~/android-sdk-linux_x86/platform-tools
```

3. Ustawienie urządzenia (uprawnienia)
Pierwszą rzeczą dotyczącą naszego urządzenia (telefonu) jest sprawdzenie, czy mamy odpowiednie uprawnienia. Opalamy polecenie w konsoli:
```bash
adb devices
```
Jeżeli otrzymamy następujący kod:
```bash
List of devices attached
???????????? no permissions
```
przechodzimy do punktu 4, jeżeli podobny do poniższego:
```bash
List of devices attached
0123456789ABCDEF device
```
krok 4 można pominąć.

4. Dodawanie uprawnień dla urządzenia
Aby dodać uprawnienia dla urządzenia, należy stworzyć regułę udev. Dla mojego urządzenia (LG GT540) odbyło się to w następujący sposób:
```bash
sudo touch /etc/udev/rules.d/51-android.rules
sudo echo 'SUBSYSTEM=="usb", ATTRS{idVendor}=="1004", SYSFS{idProduct}=="61b4", MODE="0666"' >> /etc/udev/rules.d/51-android.rules sudo chmod 644 /etc/udev/rules.d/51-android.rules
sudo restart udev
sudo ./adb kill-server
sudo ./adb start-server
```
Oczywiście dla różnych modeli telefonów są różne kody (idVendor, idProduct). Listę tych kodów można znaleźć tutaj.
Po ponownym wykonaniu polecenia
```bash
adb devices
```
powinniśmy uzyskać prawidłową odpowiedź.
I możemy cieszyć się połączeniem z naszym telefonem...

5. Fastboot
Pobieramy binarkę z tego adresu, a następnie kopiujemy ją do katalog tools w SDK. Ustawiamy odpowiednie uprawnienia:
```bash
chmod 777 fastboot
```
Aby korzystać w pełni z tego, co fastboot oferuje, należy przelogować się na konto roota. 
A teraz można wgrywać nowe ROMy na swoje telefony...
```bash
fastboot -w
fastboot erase boot
fastboot erase system
fastboot flash system system.img
fastboot flash boot boot.img
fastboot reboot
```