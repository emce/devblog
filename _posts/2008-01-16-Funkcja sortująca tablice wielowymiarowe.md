---
title: Funkcja sortująca tablice wielowymiarowe
date: 2008-01-16 09:12:54 +0100
categories: [php]
tags: [php,sort,multiarray]
image:
  path: /assets/img/php.jpg
  alt: funkcja sortująca
author: michal_cwiklinski
toc: false
---

# Funkcja sortująca tablice wielowymiarowe

Typowe funkcje sortujące wbudowane w PHP działają, ale nie zawsze tak, jakbyśmy chcieli. Stworzyłem tablicę, która zawiera różne dane, między innymi datę. I jak tu posortować taką tablicę wg "wieku" rekordów??

I znowu z pomocą przyszedł wujek Google. Znalazłem mała, ale fajnie sprytną funkcję, dzięki której można sortować tablice wielowymiarowe:

```php
function sort_multi_array($mult_array , $field , $sort_type="ASC_NUM")
{
	$code = '';
	switch($sort_type) {
		case 'ASC_NUM':
			$code .= 'return strcmp($a["'.$field.'"], $b["'.$field.'"]);';
		break;
		case 'DESC_NUM':
			$code .= 'return (-1*strcmp($a["'.$field.'"], $b["'.$field.'"]));';
		break;
	}

	$compare = create_function('$a, $b', $code);
	usort($mult_array, $compare);
	return $mult_array;
}
```