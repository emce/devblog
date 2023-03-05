---
title: Zend Framework w wersji 1.11.0
date: 2010-11-03 09:12:54 +0100
categories: [php]
tags: [php,zend framework]
image:
  path: /assets/img/php.jpg
  alt: zend framework
author: michal_cwiklinski
toc: false
---

# Zend Framework w wersji 1.11.0

Zend wydał kolejną stabilną wersję swojego frameworka PHP. Jest kilka nowości w tej wersji, choć nie jest ona wielkim "milestone'm".

### Nowości:
1. **Wsparcie dla urządzeń mobilnych**
Nowa klasa Zend_Http_UserAgent umożliwia wykrywanie rodzaju urządzeń, z których łączymy sie ze stroną. Pozwoli to (w przyszłości w pełni) do stosowania alternatywnych szablonów (rozwiązań) dla urządzeń mobilnych.

2. **Zend_Cloud: SimpleCloud API**
Interfejs SimpleCloud zapewnienie wsparcie dla przechowywania dokumentów, usług kolejce i przechowywania plików opartych po pracę "w chmurze" (wsparcie dla Amazona i Windows Azure).

3. **Bezpieczeństwo**
Naprawiono wiele miejsc z możliwymi dziurami do wykorzystania (np. za pomocą Remote Timing Attack).

4. **Zmiany w Dojo**
Dojo Toolkit został zaktualizowany do wersji 1.5.0, która zawiera nową klasę dojox.mobile, czyli wsparcie dla aplikacji mobilnych.

5. **Wsparcie dla SimpleDB**
W tej wersji dodano wsparcie dla SimpleDB. Wsparcie jest dostępne dla wszystkich operacji SimpleDB za pomocą Zend_Service_Amazon_SimpleDb .

6. **Kompatybilność z MariaDB**
Adaptery Zend_Db MySQL i PDO_MYSQL są w pełni zgodne z MariaDB, a dokumentacja została zaktualizowana w celu uwzględnienia opcji konfiguracyjnych dla tego forka MySQL .

7. **Nowe formaty plików konfiguracyjnych**
W tej wersji zawarto obsługę dodatkowych formatów konfiguracyjnych: Yaml i JSON.

8. i wiele niepomniejszych...