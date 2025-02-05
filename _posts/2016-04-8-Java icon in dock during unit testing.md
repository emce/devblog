---
title: Java icon in dock during unit testing
date: 2016-04-08 09:12:54 +0100
categories: [android]
tags: [android,java,jetbrains,ide,intellij,android studio]
image:
  path: /assets/img/intellij-idea-welcome-screen.png
  alt: java icon
author: michal_cwiklinski
toc: false
---

# Java icon in dock during unit testing

Almost every dev using unit tests on his/her Mac spotted popping java icon on dock during running test. It can be totally annoying, especially when you have Android Studio/IntelliJ IDEA open in full screen. There are two simple ways to handle this.

![IntelliJ IDEA](/assets/img/java-icon.png)

First: To your `.bash_profile` file add line:
```bash
export JAVA_TOOL_OPTIONS="-Dapple.awt.UIElement=true"
```

Add above argument to VM execution command on Android Studio/IntelliJ IDEA like on picture above on Configuration screen.

Simple...