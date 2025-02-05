---
title: Screenshot + Conky
date: 2008-02-05 09:12:54 +0100
categories: [linux]
tags: [linux,ubuntu,conky,desktop]
image:
  path: /assets/img/conky.png
  alt: conky
author: michal_cwiklinski
toc: false
---

# Screenshot + Conky

Jak większość zwolenników Linuksa, ja również bardzo często eksperymentuje z różnymi programami (czytaj: nowinkami). Ostatnio postanowiłem wypróbować Conky. Poniżej konfiguracja mojego Conky'ego.

Plik .conkyrc:
```bash
# Create own window instead of using desktop (required in nautilus)
own_window yes
own_window_transparent yes
own_window_type override
own_window_hints undecorated,below,sticky,skip_taskbar,skip_pager

# Use double buffering (reduces flicker, may not work for everyone)
double_buffer yes

# fiddle with window
use_spacer yes
use_xft yes

xftfont tahoma-7
#xftfont aqui

# Text alpha when using Xft
xftalpha 0.6

# Update interval in seconds
update_interval 3.0

# Draw shades?
draw_shades no

# Text stuff
draw_outline no # amplifies text if yes
draw_borders no
font sans
border_margin 9

# border width
border_width 6

# Default colors and also border colors, grey90 == #e5e5e5
default_color grey

own_window_colour brown
own_window_transparent yes

# Text alignment, other possible values are commented
alignment top_right
alignment bottom_right

# Gap between borders of screen and text
gap_x 10
gap_y 10

# stuff after 'TEXT' will be formatted on screen

TEXT
$color
${color orange}${alignc}$sysname $kernel

${color red}PROCESOR ${hr 2}
${freq}MHz   Load: ${loadavg}   Temp: ${acpitemp}
$cpubar
${cpugraph 000000 ffffff}
Procesy          PID       CPU%      MEM%
${top name 1} ${top pid 1}   ${top cpu 1}    ${top mem 1}
${top name 2} ${top pid 2}   ${top cpu 2}    ${top mem 2}
${top name 3} ${top pid 3}   ${top cpu 3}    ${top mem 3}
${top name 4} ${top pid 4}   ${top cpu 4}    ${top mem 4}

${color orange}PAMIĘĆ / DYSKI ${hr 2}${color orange}
RAM:   $memperc%   ${membar 6}
Swap:  $swapperc%   ${swapbar 6}${color yellow}
/: ${fs_free_perc /}%/${fs_free /}   ${fs_bar 6 /}${color green}
C: ${fs_free_perc /mnt/system}%/${fs_free /mnt/system}   ${fs_bar 6 /mnt/system}
D: ${fs_free_perc /mnt/dane}%/${fs_free /mnt/dane}   ${fs_bar 6 /mnt/dane}
E: ${fs_free_perc /mnt/reszta}%/${fs_free /mnt/reszta}   ${fs_bar 6 /mnt/reszta}

${color violet}SIEĆ${hr 2}${color violet}
IP: ${addr eth0}
Signal: ${wireless_link_qual_perc eth0}
AP name: ${wireless_essid eth0}
Down: ${downspeed eth0} Kb/s ${alignr}Up: ${upspeed eth0} Kb/s
${downspeedgraph eth0 15,50 fe0000 fe0000} ${alignr}${upspeedgraph eth0 15,50 00FF00 00FF00}

${color white}${execi 60 /mnt/reszta/Linux/gmail.sh}
```