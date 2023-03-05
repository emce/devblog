---
title: Walidacja NIP i PESEL
date: 2010-02-26 09:12:54 +0100
categories: [javascript]
tags: [javascript,validate,pesel,nip]
image:
  path: /assets/img/code.jpg
  alt: walidacja pesel nip
author: michal_cwiklinski
toc: false
---

# Walidacja NIP i PESEL

Temat wałkowany paręset razy, aczkolwiek zawsze wraca...
```javascript
function validatePesel(p) {
    p += '';
    var arr = p.split("");
    var ratio = parseInt(arr[0]) * 1;
    ratio += parseInt(arr[1]) * 3;
    ratio += parseInt(arr[2]) * 7;
    ratio += parseInt(arr[3]) * 9;
    ratio += parseInt(arr[4]) * 1;
    ratio += parseInt(arr[5]) * 3;
    ratio += parseInt(arr[6]) * 7;
    ratio += parseInt(arr[7]) * 9;
    ratio += parseInt(arr[8]) * 1;
    ratio += parseInt(arr[9]) * 3;
    var controlInt = ratio % 10;
    var controlSum = parseInt(p.substr(10, 1));
    return (controlInt == controlSum);
}

function validateNip(nip) {
    nip += '';
    nip = nip.replace(/[^0-9]+/g, '');
    if (nip.length < 10) return false;
    if (nip.length > 10) return false;
    var controlSum = 0;
    controlSum += parseInt(nip.charAt(0)) * 6;
    controlSum += parseInt(nip.charAt(1)) * 5;
    controlSum += parseInt(nip.charAt(2)) * 7;
    controlSum += parseInt(nip.charAt(3)) * 2;
    controlSum += parseInt(nip.charAt(4)) * 3;
    controlSum += parseInt(nip.charAt(5)) * 4;
    controlSum += parseInt(nip.charAt(6)) * 5;
    controlSum += parseInt(nip.charAt(7)) * 6;
    controlSum += parseInt(nip.charAt(8)) * 7;
    if ((controlSum % 11) == parseInt(nip.charAt(9))) {
        return true;
    } else {
        return false;
    }
}
```