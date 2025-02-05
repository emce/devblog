---
title: Klasa do dynamicznych stron
date: 2007-12-06 09:12:54 +0100
categories: [php]
tags: [php,code igniter,routing,cms]
image:
  path: /assets/img/php.jpg
  alt: klasa do dynamicznych stron
author: michal_cwiklinski
toc: false
---

# Klasa do dynamicznych stron

Przy okazji tworzenia CMSa do bardzo prostej (prawie statycznej, jeżeli chodzi o treść) strony musiałem wyszukać sposób na przepisanie klasy routowania adresów URL tak, aby jeden kontroler obsługiwał wszystkie dynamiczne adresy. Oczywiście pomógł wujek Google, dzieki któremu znalazłem klasę, która po lekkich przeróbkach pozwoliła mi uzyskać, to co chciałem.


```php
class MY_Router extends CI_Router
{
    function _validate_segments($segments)
    {
        $excluded = array('administracja','login');
    	if (count($segments) <= 1 && !in_array($segments[0],$excluded))
        {
            if(count($segments) == 1)
            {
        		array_unshift($segments, 'main', 'page');
            }
            elseif(count($segments) == 0)
            {
            	$segments = array('main', 'page', 'mainpage');
            }
        }
        elseif($segments[0]=='administracja')
        {
        	for($i=0;$i
        	{
        		if($i==0)
        		{
        			$new_segments[] = 'admin';
        		}
        		else
        		{
        			$new_segments[] = $segments[$i];
        		}
        		$segments = $new_segments;
        	}
        }
        return parent::_validate_segments($segments);
    }
}
```