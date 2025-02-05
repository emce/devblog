---
title: Testy - Robolectric i ostrzeżenia o błędach klas cieni (Shadow)
date: 2012-10-02 09:12:54 +0100
categories: [android]
tags: [android,tests,unit testing,robolectric,shadows]
image:
  path: /assets/img/robolectric.png
  alt: robolectric
author: michal_cwiklinski
toc: false
---

# Testy - Robolectric i ostrzeżenia o błędach klas cieni (Shadow)

Pisząc testy dla aplikacji Androidowej wykorzystując framework Robolectric, a następnie uruchamiając je nawet na różne sposoby, można otrzymać następujące ostrzeżenia (warning):
```bash
Warning: an error occurred while binding shadow class: ShadowGeoPoint
Warning: an error occurred while binding shadow class: ShadowItemizedOverlay
Warning: an error occurred while binding shadow class: ShadowMapController
Warning: an error occurred while binding shadow class: ShadowMapActivity
Warning: an error occurred while binding shadow class: ShadowMapView
Warning: an error occurred while binding shadow class: ShadowOverlayItem
```

Dzieje się tak ze względu na fakt, że niektóre klasy Androidowe (z SDK) mają zależności znajdujące się w Google API. Rozwiązaniem jest podpięcie SDK z Google API, bądź dopięcie Google API w osobnym jar'ze.