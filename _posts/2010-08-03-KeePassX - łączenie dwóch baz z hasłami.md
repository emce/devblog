---
title: KeePassX - łączenie dwóch baz z hasłami
date: 2010-08-03 09:12:54 +0100
categories: [linux]
tags: [linux,ubuntu,password manager,database]
image:
  path: /assets/img/keepassx.jpg
  alt: keepassx łączenie dwóch baz
author: michal_cwiklinski
toc: false
---

# KeePassX - łączenie dwóch baz z hasłami

KeePassX jest programem, bez którego nie wyobrażam sobie funkcjonowania w świecie haseł, loginów, pinów, itp. Wydarzyła się sytuacja, w której musiałem połączyć dwie bazy - domową i "pracową". KeePassX nie ma opcji łączenia baz, jest ona w tej chwili zgłoszona jako nowa i deweloperzy nad nią pracują, więc należy radzić sobie samemu.

Sprawa jest prosta, oczywiście przepis po długim szperaniu znalazłem na Google'u. Oto procedurka:

1. Eksportujemy obie bazy do plików XML
2. Ściągamy załączony plik (schemat konwersji) [Schemat konwersji XML](http://blog.cwiklinski.org/wp-content/uploads/2010/08/merge.zip)
3. Kopiujemy wszystkie 3 pliki do jednej lokalizacji i odpalamy polecenie
```bash
xsltproc --output merged.xml --stringparam with db1.xml merge.xsl db2.xml
```
gdzie `db1.xml` i `db2.xml` to nasze wyeksportowane bazy, a `merged.xml` to wynik połączenia.