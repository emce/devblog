---
title: Funkcja pokazująca czas generowania strony
date: 2007-12-13 09:12:54 +0100
categories: [php]
tags: [php,page generation time,performance]
image:
  path: /assets/img/php.jpg
  alt: czas generowania strony
author: michal_cwiklinski
toc: false
---

# Funkcja pokazująca czas generowania strony

Do sprawdzenia, jaki czas jest potrzebny, aby nasza strona się wygenerowała, wydłubałem małą funkcję. Podajemy jej parametr, jeden na początku kodu strony, drugi na samym końcu.

```php
function getmicrotime(){
    list($usec, $sec) = explode(" ",microtime());
    return ((float)$usec + (float)$sec);
    }
function pagetime($type)
{
	static $orig_time;
	if($type=="init")
	{
		$orig_time=getmicrotime();
	}
	if($type=="print")
	{
		printf("Page generated in %2.4f Seconds",getmicrotime()-$orig_time);
	}
}

pagetime('init');

// ----------------- tutaj kod strony  ------------------------------

pagetime('print');
```