---
title: Skrypt do zmiany rozdzielczości grafiki
date: 2008-06-04 09:12:54 +0100
categories: [linux]
tags: [linux,ubuntu,display,resolution]
image:
  path: /assets/img/ubuntu.jpg
  alt: zmiana rozdzielczości
author: michal_cwiklinski
toc: false
---

# Skrypt do zmiany rozdzielczości grafiki

Jak to w życiu bywa, czasami trzeba się pobawić bardzo prostymi rzeczami, które w małej liczbie są przyjemne, zaś w dużej - stają się nudne. Tak jest z grafiką, którą w liczbie większej od 100 musimy zoptymalizować poprzez zmniejszenie rozdzielczości. Ale od czego jest Linux i shell...

Prosty skrypt, wykorzystujący bibliotekę `ImageMagick` (znaleziony na G), który wystarczy skonfigurować, podając katalog z grafiką, oraz maksymalną wysokość i szerokość grafiki, a następnie tylko czekać na efekty:

```bash
#!/bin/bash

if [ $# -le 0 ]; then
    echo "Użycie: resize_images.sh <ścieżka_do_katalogu>"
    exit 1
fi

final_width=800
final_height=800
dir_name="$1"

echo "Modyfikuję pliki z katalogu $dir_name"

for file in "$dir_name"/*.[jJ][pP][gG]; do
    width=`identify -format %w "$file"`
    height=`identify -format %h "$file"`
    if [ $width -gt $height ]; then
        if [ $width -le $final_width ]; then
            echo "$file: size = $width"x"$height, rozmiary nie przekraczają maksimum! Zmiany nie dokonano"
            continue;
        fi
        new_height=`echo "$height*$final_width/$width" | bc`
        file_basename=`basename "$file"`
        file_dirname=`dirname "$file"`
        new_file_name=""$file_dirname"/scaled_$file_basename"
     
        # resize the image
        echo "$file: rozmiar = $width"x"$height, zmieniam na " "$final_width"x"$new_height"
        convert -resize "$finale_width"x"$new_height" "$file" "$new_file_name"

        # replace the original file with the scaled file
        rm -f "$file"
        if [ $? -eq "0" ]; then
            mv "$new_file_name" "$file"
        fi
    else
        if [ $height -le $final_height ]; then
            echo "$file: rozmiar = $width"x"$height, rozmiary nie przekraczają maksimum! Zmiany nie dokonano"
            continue;
        fi
        new_width=`echo "$width*$final_height/$height" | bc`
        file_basename=`basename "$file"`
        file_dirname=`dirname "$file"`
        new_file_name=""$file_dirname"/scaled_$file_basename"
    
        # resize the image
        echo "$file: rozmiar = $width"x"$height, zmieniam na $new_width"x"$final_height"    
        convert -resize "$new_width"x"$final_height" "$file" "$new_file_name"

        # replace the original file with the scaled file
        rm -f "$file"
        if [ $? -eq "0" ]; then
            mv "$new_file_name" "$file"
        fi
    fi
done

echo "Zrobione"
```