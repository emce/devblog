---
title: Nowe Google Analytics API v4 dla Androida
date: 2014-06-18 09:12:54 +0100
categories: [android]
tags: [android,gradle,google analytics]
image:
  path: /assets/img/google-analytics-android.jpg
  alt: nowe google analytics
author: michal_cwiklinski
toc: false
---

# Nowe Google Analytics API v4 dla Androida

Od wersji 4.0 API Google Analytics dla Androida zostało przeniesione do Google Play Services (hura??!!). Używana wcześniej klasa `EasyTracker` została usunięta, ale nie oznacza to, że w prosty sposób nie można zaimplementować śledzenia. Do rzeczy...

Oczywiście podstawowym krokiem implementacji nowego API jest posiadanie biblioteki Google Play Services. Dla korzystających z Gradle sprawa jest prosta: `compile 'com.google.android.gms:play-services:+'` Google Analytics API v4 posiada szereg klas pomocniczych i opcji konfiguracyjnych, ale całość wydaje się trochę niejasno opisana. Pierwszym krokiem jest utworzenie plików konfiguracyjnych. Pierwszy jest plik `global_tracker.xml`, który jest plikiem zawierającym ogólną konfigurację. Zapisujemy go w katalogu `res/xml`. Zawiera również listę ekranów, które będziemy monitorować.
```xml
<?xml version="1.0" encoding="utf-8"?>

<resources xmlns:tools="http://schemas.android.com/tools" tools:ignore="TypographyDashes">

<!-- poziom logowania -->
<string name="ga_logLevel">verbose</string>

<!-- częstotliwość dispatchera -->
<integer name="ga_dispatchPeriod">30</integer>

<!-- wyłączenie śledzenia np.: do testów -->
<bool name="ga_dryRun">false</bool>

<!-- definicje ekranów -->
<string name="mobi.cwiklinski.MainActivity">Main Activity</string>

</resources>
```

  W tym pliku nie podajemy jeszcze ID śledzenia, a dzięki opcji `ga_dryRun` można wyłączyć śledzenie. Kolejnym plikiem konfiguracyjnym jest `app_tracker.xml`, w którym definiujemy ID śledzenia, tutaj również określamy listę naszych ekranów.
```xml
<?xml version="1.0" encoding="utf-8"?>

<resources xmlns:tools="http://schemas.android.com/tools" tools:ignore="TypographyDashes">

<!-- Analytics Tracking Id -->
<string name="ga_trackingId">UX-XXXXXXXX-X</string>

<!-- procent zdarzeń uwzględnianych w raportowaniu -->
<string name="ga_sampleFrequency">100.0</string>

<!-- automatyczne mierzenie aktywności -->
<bool name="ga_autoActivityTracking">true</bool>

<!-- wyłapywanie i raportowanie niewyłapanych wyjątków w aplikacji -->
<bool name="ga_reportUncaughtExceptions">true</bool>

<!-- długość sesji -->
<integer name="ga_sessionTimeout">-1</integer>

<!-- alternatywna lista ekranów -->
<screenName name="com.mycompany.MyActivity">My Activity</screenName>

</resources>
```

Ostatnią modyfikacją związaną z konfiguracją usługi jest wpis do `AndroidManifest.xml`. W gałęzi `<application>` dodajemy:
```xml
<meta-data android:name="com.google.android.gms.analytics.globalConfigResource" android:resource="@xml/global_tracker" />
```

Następnie tworzymy bądź modyfikujemy naszą klasę aplikacji. Poniższy przykład jest najprostszym sposobem zdefiniowania tej klasy, aby można było korzystać ze śledzenia.
```java
package mobi.cwiklinski.analytics;

import android.app.Application;

import com.google.android.gms.analytics.GoogleAnalytics;
import com.google.android.gms.analytics.Tracker;

import java.util.HashMap;

public class App extends Application {

    // The following line should be changed to include the correct property id.
    private static final String PROPERTY_ID = "UX-XXXXXXXX-X";

    public enum TrackerName {
        APP_TRACKER,
        GLOBAL_TRACKER
    }

    HashMap<TrackerName, Tracker> mTrackers = new HashMap<TrackerName, Tracker>();

    public App() {
        super();
    }

    public synchronized Tracker getTracker(TrackerName trackerId) {
        if (!mTrackers.containsKey(trackerId)) {
            GoogleAnalytics analytics = GoogleAnalytics.getInstance(this);
            Tracker t = (trackerId == TrackerName.APP_TRACKER) ? analytics.newTracker(R.xml.app_tracker)
                : analytics.newTracker(PROPERTY_ID);
            mTrackers.put(trackerId, t);
        }
        return mTrackers.get(trackerId);
    }
}
```
Po stworzeniu/modyfikacji powyższej klasy nie pozostaje nic innego jak implementacja śledzenia w aktywnościach.
```java
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        ((App) getApplication()).getTracker(App.TrackerName.APP_TRACKER);
    }

    @Override
    protected void onStart() {
        super.onStart();
        GoogleAnalytics.getInstance(this).reportActivityStart(this);
    }

    @Override
    protected void onStop() {
        super.onStop();
        GoogleAnalytics.getInstance(this).reportActivityStop(this);
    }
```

Do pełnej obsługi zdarzeń należy zaimplementować odpowiednie funkcje w każdym z powyższych trzech callback'ów. 

Good luck!