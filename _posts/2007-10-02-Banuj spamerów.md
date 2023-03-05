---
title: Banuj spamerów
date: 2007-09-17 09:12:54 +0100
categories: [php]
tags: [php,ban,ip,adblocker]
image:
  path: /assets/img/php.jpg
  alt: php
author: michal_cwiklinski
toc: false
---

# Banuj spamerów!

Bardzo prosta biblioteka, której używam do banowania IP'ików, z których dodawane są komentarze lub wpisy "pseudoreklamowe". Można z niej korzystać na dwa sposoby:

1. pierwszy - standardowa tablica array z adresami IP;
2. drugi - tabela w bazie danych.

```php
class Ban {

function Ban()
{
	$CI =& get_instance();

	$CI->load->helper('url');
	//  Prosta tablica
	$ip = $CI->input->ip_address();
	$banned = array('209.47.94.52','88.119.246.111');

	// Dane z bazy
	$CI->load->model('MBan');
	$banned = $CI->MBan->getAddresses();

	if(!strstr($CI->uri->uri_string(),'youarebanned'))
	{
		if(in_array($ip,$banned))
		{
			redirect('youarebanned');
		}
	}
}
```

[Biblioteka Ban](http://blog.cwiklinski.org/wp-content/uploads/2007/10/ban.zip)