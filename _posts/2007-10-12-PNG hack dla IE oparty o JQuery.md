---
title: PNG hack dla IE oparty o JQuery
date: 2007-10-12 09:12:54 +0100
categories: [javascript]
tags: [javascript,jquery,png,transparency]
image:
  path: /assets/img/code.jpg
  alt: code
author: michal_cwiklinski
toc: false
---

# PNG hack dla IE oparty o JQuery

Poszukując rozwiązania ciągle istniejącego problemu wyświetlania przeźroczystych PNG'ków w IE6, znalazłem bardzo fajny skrypcik. Jest to zastosowanie biblioteki JQuery, "zaprzęgające" do pracy odpowiednie filtry do wskazanych przez użytkownika elementów, bez zmiany tagów. Skrypt obsługuje zarówno elementy `<img>`, jak i style CSS. No i co najważniejsze - jak widać poniżej - jest bardzo prosty w użyciu!

Biblioteka [JQuery](http://jquery.khurshid.com/jquery-1.2.1.pack.js)

Biblioteka [ifixpng](http://jquery.khurshid.com/jquery.ifixpng.js)

```javascript
 // apply to all png images

$('img[@src$=.png]').ifixpng();// apply to all png images and to div#logo

$('img[@src$=.png], div#logo').ifixpng();

// apply to div#logo, undo fix, then apply the fix again

$('img[@src$=.png], div#logo').ifixpng().iunfixpng().ifixpng();

// apply to div#logo2, modify css property and add click event

$('div#logo2').ifixpng().css({cursor:'pointer'}).click(function(){ alert('ifixpng is cool!'); });
```

Strona projektu: [ifixpng](http://jquery.khurshid.com/ifixpng.php)