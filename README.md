# Avenue Analytics SDK Integration

To integrate the SDK into your Android app:

1. Put the distributed aar file in the libs directory of your app. 
2. In your project level build.gradle, add the following lines:
 
```groovy
allprojects {
  repositories {
    flatDir {
      dirs 'libs'
    }
  }
}
```

3.  In your app level build.gradle files include the following lines:
```groovy
compile(name: 'avenue-analytics-5.3.4', ext: 'aar')

compile 'com.google.code.gson:gson:2.8.0'
```



4. Initialize the SDK in your Application’s onCreate() method:
```java
import com.youtility.datausage.AvenueAnalytics;

public class YourApplication extends Application {

    @Override public void onCreate() {
        super.onCreate();
        AvenueAnalytics.init(this);
    }
}
```

5. To collect data the SDK (and therefore your app) needs Usage Access. To prompt the user to grant this permission launch the Usage Access info page with the following code:

```java
if (AvenueAnalytics.hasUsageAccessSetting(context)) {
    Intent intent = new Intent(Settings.ACTION_USAGE_ACCESS_SETTINGS);
    startActivity(intent);
}
```

6. If the Target SDK of the host app (your app) is higher than 23 the following permissions are required to get system info. To implement required run-time permission, please see https://developer.android.com/training/permissions/requesting.html. 
``` xml
<uses-permission android:name="android.permission.PACKAGE_USAGE_STATS" />
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
<uses-permission android:name="android.permission.READ_PHONE_STATE" />
<uses-permission android:name="android.permission.ACCESS_WIFI_STATE"/>
```

7. ProGuard may remove some portions of the SDK code during optimization which causes unpredictable issues in our data collection. To avoid that please add this line in the config file. 
```
-keep class com.youtility.datausage.** { *; }
```
