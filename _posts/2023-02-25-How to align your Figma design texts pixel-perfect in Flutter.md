---
title: How to align your Figma design texts pixel-perfect in Flutter
date: 2023-02-28 12:06:04 +0100
categories: [Mobile]
tags: [flutter,textfield,border]
image:
  path: /assets/img/figma_pixel_perfect.png
  alt: textfield border
author: julia_iskierka
toc: false
---

# How to align your Figma design texts pixel-perfect in Flutter

You're implementing a component designed in Figma, trying to align everything perfectly but even though every margin, padding, and alignment seems to match those in Figma something is still off. This can be caused by differences in how Figma and Flutter distribute text line-height property.

## Text height problem

Designers working on typography have a number of parameters to emphasize the application character. One is line-height - the vertical distance between two lines of text. When the line height is bigger than the font size, empty space is added but how it's distributed can differ between Figma and Flutter.

![Figma](/assets/img/figma_pixel_perfect_2.png){: w="250" h="100" }
_Figma_
![Flutter](/assets/img/figma_pixel_perfect_3.png){: w="250" h="100" }
_Flutter_

## How to fix it

The Flutter `Text` widget has a property called `textHeightBehavior`. It accepts a `TextHeightBehavior` class with a `leadingDistribution` parameter that defaults to `TextLeadingBehaviour.proportional`. Changing it to `TextLeadingDistribution.even` may be the solution for you.
![TextLeadingDistribution.even](/assets/img/figma_pixel_perfect_4.png)
```dart
const Text(
  'Example text',
  textHeightBehavior: TextHeightBehavior(
    leadingDistribution: TextLeadingDistribution.even,
  ),
)
```