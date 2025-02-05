---
title: Przewijany DIV
date: 2007-09-26 09:12:54 +0100
categories: [css]
tags: [css,div,scroll]
image:
  path: /assets/img/scrollable-div.jpg
  alt: przewijany div
author: michal_cwiklinski
toc: false
---

# It has begun…

Bardzo przydatna część kodu, która przydaje się na stronach zawierających dużo treści. Można ją stosować w elementach blokowych, oczywiście najczęściej wykorzystywanym jest DIV. Oczywiście jest to implementacja definicji CSS overflow:

- `overflow: auto` - powoduje wyświetlenie jedynie tego paska przewijania, który jest w danej sytuacji potrzebny. W przypadku akapitu będzie to jedynie pasek pionowy.
- `overflow: scroll` - powoduje wyświetlenie pionowego i poziomego paska przewijania obejmujących zdefiniowany obszar występowania.
- `overflow: visible` - powoduje wyświetlenie całej zawartości elementu, bez względu na zdefiniowany obszar występowania. W Internet Explorerze 6 wylana zawartość nie nakłada się na inne elementy, w IE 7, Firefoksie i Operze - nakłada się.
- `overflow: hidden` - powoduje przycięcie widocznej zawartości akapitu do obszaru zdefiniowanego pudełka. Wylana część akapitu nie będzie widoczna.


```css
div.scroll {
	height: 200px;
	width: 300px;
	overflow: auto;
	border: 1px solid #666;
	background-color: #ccc;
	padding: 8px;
}
```