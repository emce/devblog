---
title: AndroidAnnotations & IntelliJ IDEA [UPDATE]
date: 2012-06-27 09:12:54 +0100
categories: [android]
tags: [android,annatations,intellij]
image:
  path: /assets/img/android-studio-ubuntu.jpg
  alt: intellij
author: michal_cwiklinski
toc: false
---

# AndroidAnnotations & IntelliJ IDEA [UPDATE]

Setup IntelliJ IDEA to work with Android Annotations:

1. Download libraries: archive with JAR's is [here](https://github.com/excilys/androidannotations/wiki/Download) - download it and - CAUTION - file `androidannotations-X.X.X-api.jar` must be put in libs directory of project, and for `androidannotations-X.X.X.jar` file a new directory must be created (e.g. `ext-libs` - what is thea idea of this - description [here](https://github.com/excilys/androidannotations/wiki/Eclipse-Project-Configuration)).
2. Add `androidannotations-X.X.X-api.jar` to `libraries` of project and default module.
3. Set annotations compilation.
4. During the first compilation there could be a compilation error - fix it by adding suffix to default activity in `AndroidManifest.xml`(e.g. `MainActivity_` instead of `MainActivity`). AA generates additional classes in gen directory, and those all classes have _ suffix.
5. If the project still gets compilation error, switch default activity for other with no AA implementation or create temporary one, rebuild, and switch back to main one with suffix.

For more suggestions please check [this site](https://github.com/excilys/androidannotations/wiki/Cookbook).
 
Happy injection!