---
title: Google Play services divided into small parts - AT LAST (UPDATE - 7.0)
date: 2014-12-10 09:12:54 +0100
categories: [android]
tags: [blog]
image:
  path: /assets/img/android-gradle.png
  alt: google play services
author: michal_cwiklinski
toc: false
---

# Google Play services divided into small parts - AT LAST (UPDATE - 7.0)

So far, Google didn't allow developers to use a small parts of **Google Play Services** library - if you want to use just Google Signup - you had to pull whole big library, which end finally in 65K DEX method limit, or use some strange Gradle modification, which slow down building process. At last Google have listened to developers, and split Google Play Services into small libraries along with version 6.5.87 (current is 7.0.0).

For this version, you can use a number of smaller client libraries, so that only these chosen ones will get compiled into your .dex file, and therefore their methods won't count towards method limit. This is, what you can use now:

|Google Play services API|Description in build.gradle|
|Google+|`com.google.android.gms:play-services-plus:7.0.0`|
|Google Account Login|`com.google.android.gms:play-services-identity:7.0.0`|
|Google Actions, Base Client Library|`com.google.android.gms:play-services-base:7.0.0`|
|Google App Indexing|`com.google.android.gms:play-services-appindexing:7.0.0`|
|Google Analytics|`com.google.android.gms:play-services-analytics:7.0.0`|
|Google Cast|`com.google.android.gms:play-services-cast:7.0.0`|
|Google Cloud Messaging|`com.google.android.gms:play-services-gcm:7.0.0`|
|Google Drive|`com.google.android.gms:play-services-drive:7.0.0`|
|Google Fit|`com.google.android.gms:play-services-fitness:7.0.0`|
|Google Location, Activity Recognition, and Places|`com.google.android.gms:play-services-location:7.0.0`|
|Google Maps|`com.google.android.gms:play-services-maps:7.0.0`|
|Google Mobile Ads|`com.google.android.gms:play-services-ads:7.0.0`|
|Google Nearby|`com.google.android.gms:play-services-nearby:7.0.0`|
|Google Panorama Viewer|`com.google.android.gms:play-services-panorama:7.0.0`|
|Google Play Game services|`com.google.android.gms:play-services-games:7.0.0`|
|SafetyNet|`com.google.android.gms:play-services-safetynet:7.0.0`|
|Google Wallet|`com.google.android.gms:play-services-wallet:7.0.0`|
|Android Wear|`com.google.android.gms:play-services-wearable:7.0.0`|

[More info](http://android-developers.blogspot.com/2014/12/google-play-services-and-dex-method.html)