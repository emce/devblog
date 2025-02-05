---
title: QuickTip - Skype nie działa - błąd audio?
date: 2013-05-13 09:12:54 +0100
categories: [linux]
tags: [linux,ubuntu,skype,audio]
image:
  path: /assets/img/skype-for-linux.jpg
  alt: skype nie działa
author: michal_cwiklinski
toc: false
---

# QuickTip - Skype nie działa - błąd audio?

Dla wersji 64-bitowej dowolnej dystrybucji Linuksa występują problemy z systemem audio w przypadku korzystania ze Skype'a. Oczywiście wszelkie problemy są generowane faktem, że Skype jest aplikacją 32-bitową, więc potrzebne są odpowiednie biblioteki dla tej architektury. I tak w przypadku problemów z audio (głośniki, mikrofon, itp) wystarczy doinstalować odpowiedni pakiet - w przypadku Ubuntu i pochodnych jest to:
```bash
apt-get install libpulse0:i386
```