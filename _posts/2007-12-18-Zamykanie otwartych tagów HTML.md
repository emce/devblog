---
title: Zamykanie otwartych tagów HTML
date: 2007-12-18 09:12:54 +0100
categories: [php]
tags: [php,html,tags]
image:
  path: /assets/img/php.jpg
  alt: zamykanie otwartych tagów
author: michal_cwiklinski
toc: false
---

# Zamykanie otwartych tagów HTML

Malutka funkcja, która się przydaje, np.: przy skracaniu tekstu artykułu, newsa, itp., gdzie mogą wystąpić niezamknięte tagi.

```php
function close_tags($html){
  #umieszcza wszystkie otwarte tagi w tablicy
  preg_match_all("#<([a-z]+)( .*)?(?!/)>#iU",$html,$result);
  $openedtags=$result[1];

  #umieszcza wszystkie zamknięte tagi w tablicy
  preg_match_all("#</([a-z]+)>#iU",$html,$result);
  $closedtags=$result[1];
  $len_opened = count($openedtags);
  # wszystkie tagi są zamknięte
  if(count($closedtags) == $len_opened){
    return $html;
  }

  $openedtags = array_reverse($openedtags);
  # zamykanie tagów
  for($i=0;$i < $len_opened;$i++) {
    if (!in_array($openedtags[$i],$closedtags)){
      $html .= '</'.$openedtags[$i].'>';
    } else {
      unset($closedtags[array_search($openedtags[$i],$closedtags)]);
    }
  }
  return $html;
}
```