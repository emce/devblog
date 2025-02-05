---
title: Media scanning
date: 2015-01-15 09:12:54 +0100
categories: [android]
tags: [intent,camera,media scanning,android,sd card]
image:
  path: /assets/img/camera-application.jpg
  alt: media scanning
author: michal_cwiklinski
toc: false
---

# Media scanning

So far, after recording on downloading new media file in order to make system see this file developer had to fire broadcast:
```java
sendBroadcast(new Intent(Intent.ACTION_MEDIA_MOUNTED, Uri.parse("file://" + context.getExternalFilesDir(null))));
```

But now, instead of scan proccess, one can get
```java
java.lang.SecurityException: Permission Denial: not allowed to send broadcast android.intent.action.MEDIA_MOUNTED
```
The simplest solution to get rig of this exception is to use MediaScanner:
```java
    public static void scanFile(Context context, String path) {
        MediaScannerConnection.scanFile(
            context.getApplicationContext(),
            new String[]{ path },
            null,
            new MediaScannerConnection.OnScanCompletedListener() {
                @Override
                public void onScanCompleted(String path, Uri uri) {
                    Log.e(TAG, "file " + path + " was scanned successfully: " + uri);
                }
            });
    }
```