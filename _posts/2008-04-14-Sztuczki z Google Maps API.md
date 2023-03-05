---
title: Sztuczki z Google Maps API
date: 2008-04-13 09:12:54 +0100
categories: [javascript]
tags: [javascript,google,google maps,api]
image:
  path: /assets/img/start-blogowania.jpg
  alt: start blogowania
author: michal_cwiklinski
toc: false
---

# Sztuczki z Google Maps API

Pracując z tym Googlowym API wyszukałem parę sztuczek, które pomogą przy wykorzystywaniu tego interfejsu.

1. Pozycja środka mapy
Aby znaleźć pozycję środka mapy, którą aktualnie oglądamy, wystarczy w pasek adresu przeglądarki wkleić poniższy kod:

```javascript
javascript:void(prompt('',gApplication.getMap().getCenter()));
```

a na ekranie pojawi się popup z koordynatami.

2. Ukrycie nawigacji, gdy kursor jest poza mapą: wystarczy do skryptu dokleić kod:
 
```javascript
 function load() {  
     if (GBrowserIsCompatible()) {  
      var map = new GMap2(document.getElementById("map"));  
      map.setCenter(new GLatLng(lat, lng), zoom);  
      map.addControl(new GLargeMapControl ());    
      map.addControl(new GOverviewMapControl());    
      map.addControl(new GScaleControl());    
      map.addControl(new GMapTypeControl());    
      map.setCenter(new GLatLng(lat, lng), zoom);  
   
      map.hideControls();  
      GEvent.addListener(map, "mouseover", function(){map.showControls();});  
      GEvent.addListener(map, "mouseout", function(){map.hideControls();});  
       
       }  
     }  
```

3. Pokazanie bieżącej pozycji kursora na mapie: (również kawałek kodu)

```javascript
var lastPoint;

GEvent.addListener(map, "mousemove", function(point){

var latLngStrF = point.lat().toFixed(14) + ', ' + point.lng().toFixed(14) ;
var latLngStr8 = point.lat().toFixed(8) + ', ' + point.lng().toFixed(8);
var latLngStr6 = point.lat().toFixed(6) + ', ' + point.lng().toFixed(6);
var latLngStr5 = point.lat().toFixed(5) + ', ' + point.lng().toFixed(5);
var latLngStr4 = point.lat().toFixed(4) + ', ' + point.lng().toFixed(4);


document.getElementById("precision").options[0].text = latLngStrF;
document.getElementById("precision").options[1].text = latLngStr8;
document.getElementById("precision").options[2].text = latLngStr6;
document.getElementById("precision").options[3].text = latLngStr5; 
document.getElementById("precision").options[4].text = latLngStr4;

lastPoint = point;
});
```

Przykład [tutaj](http://koti.mbnet.fi/ojalesa/exam/mousemove.html)