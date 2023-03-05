---
title: Tylko liczby
date: 2007-09-17 09:12:54 +0100
categories: [javascript]
tags: [javascript]
image:
  path: /assets/img/code.jpg
  alt: code
author: michal_cwiklinski
toc: false
---

# Tylko liczby

Prosta funkcja walidacyjna, dzięki której można wymusić na użytkowniku wpisanie w pole tekstowe tylko i wyłącznie liczb.

```javascript
function isNumberKey(evt)
{
	var charCode = (evt.which) ? evt.which : event.keyCode
	if (charCode > 31 && (charCode < 48 || charCode > 57))
		return false;
	return true;
}
```