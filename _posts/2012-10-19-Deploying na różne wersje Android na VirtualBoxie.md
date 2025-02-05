---
title: Deploying na różne wersje Android na VirtualBoxie
date: 2012-10-19 09:12:54 +0100
categories: [android]
tags: [android,deploy,virtualbox]
image:
  path: /assets/img/virtualbox-android.jpg
  alt: android na virtualboxie
author: michal_cwiklinski
toc: false
---

# Deploying na różne wersje Android na VirtualBoxie

Android znajduje się w chwili obecnej na baaardzo dużej ilości urządzeń, oraz w dużej ilości różnych wersji tego systemu. Ale w związku z tym, programiści mają twardy orzech do zgryzienia - napisanie aplikacji kompatybilnej (pod względem wersji systemu, jak i wyglądu). Jak to zrobić?

Rozwiązań jest parę. Po pierwsze - masz parę różnych telefonów (sic!), a po drugie - i zapewne lepsze (tańsze) rozwiązanie - korzystasz z możliwości projektu Android x86 i VirtualBoxa. Ale - bo jest małe ale - rozdzielczość w projekcie Android x86 to rozdzielczość tabletowa - z telefonami? Otóż - można tak poustawiać sobie poszczególne maszyny w VirtualBoxie, aby pracowały z różnymi rozdzielczościami - czyli emulowały różne telefony. Pominę krok tworzenia maszyny wirtualnej z Androidem, przejdę od razu do ustawiania rozdzielczości. Najważniejsze są dwa kroki: ustawienie dodatkowej rozdzielczości w VirtualBoxie, i obsłużenie jej w Androidzie (nie do końca w Androidzie, ale o tym zaraz). 

Przyjmijmy, że chcemy ustawić rozdzielczość dla jakiegoś telefonu, i przyjmijmy, że będzie to 400x640x16 (bardzo ładnie prezentuje się na laptopie i łatwo na niej pracować).  Oto kolejne kroki:
1. Poniższym poleceniem dodajemy maszynie wirtualnej obsługę dodatkowej rozdzielczości:
```bash
VBoxManage setextradata "nazwa_maszyny" "CustomVideoMode1" "400x640x16"
```
2. Teraz trzeba poprosić naszego Androida, żeby tą rozdzielczość obsłużył. A dokładniej nie Androida, tylko bootloadera, którym w przypadku projektu Android x86 jest GRUB. Trzeba zmodyfikować plik konfiguracyjny GRUBa, aby wczytał nam kernel Androida z obsługą naszej rozdzielczości:
  - przełączamy maszynę w tryb debug,
  - w konsoli wpisujemy polecenie `vi /mnt/grub/menu.lst`
3. linię ładującą domyślny kernel modyfikujemy, dodając obsługę rozdzielczości, w moim przypadku był to Android 2.3, i zmiana wyglądała następująco: 
  przed:
```
kernel /android-2.3-RC1/kernel quiet root=/dev/ram0 androidboot_hardware=eeepc acpi_sleep=s3_bios,s3_mode DPI=160 SRC=/android-2.3-RC1 SDCARD=/data/sdcard.img
```
  po:
```
kernel /android-2.3-RC1/kernel quiet root=/dev/ram0 androidboot_hardware=eeepc acpi_sleep=s3_bios,s3_mode DPI=160 UVESA_MODE=400x640x16 SRC=/android-2.3-RC1 SDCARD=/data/sdcard.img vga=864
```
4. restart, i voila!