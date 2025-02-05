---
title: Thunar jako domyślny Menedżer Plików w Gnome
date: 2008-02-02 09:12:54 +0100
categories: [linux]
tags: [linux,ubuntu,thunar,file manager]
image:
  path: /assets/img/ubuntu.jpg
  alt: menedzer plików
author: michal_cwiklinski
toc: false
---

# Thunar jako domyślny Menedżer Plików w Gnome

Wiem, że temat był wałkowany parę razy, ale trzeba udzielać wskazówek, jak znalazło się coś nowego. A ja wałkowałem, aż coś wyszło.

Pierwsza sprawa, to oczywiście instalacja Thunara. W konsoli należy wpisać:

```bash
apt-get install thunar*
```

Zainstalują się wszystkie pakiety dla Thunara, łącznie z menedżerem woluminów, podglądem miniaturek, itd. Po zainstalowaniu Thunara robimy backup pliku z ustawieniami Nautilusa:

```bash
mv /usr/bin/nautilus /usr/bin/nautilus.real
```

Następnie edytujemy nowy plik, w którym będą ustawienia dotyczące tego, które polecenia ma wykonywać Thunar, a przy których dalej wykorzystywany będzie Nautilus:

```bash
gedit /usr/bin/nautilus
```

Związane jest to z faktem, że Thunar nie obsługuje jeszcze wszystkich protokołów, np.: SSH(ssh:), Samba(smb:). Teraz wpisujemy kod skryptu, który będzie to dla nas załatwiał:

```bash
#!/bin/bash


#zenity --info --text="${1}/${@}" #DEBUG
if [ -n "${1}" ]; then
    if [ "${1:0:1}" == "/" ]; then
        thunar "${1}"
    elif [ "${1}" == "--no-desktop" ]; then
        if [ -n "${2}" ]; then
            if [ "${2:0:7}" == "file://" ] || [ "${2:0:1}" == "/" ]; then
                thunar "${2}"
            else
                nautilus.real "${2}"
            fi
        else
            thunar
        fi
    else
        nautilus.real "${@}"
    fi
else
    thunar
fi
```

Później wystarczy tylko ustawić prawa:

```bash
chmod 755 /usr/bin/nautilus
```

i gotowe!