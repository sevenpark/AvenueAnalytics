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
compile(name: 'avenue-analytics', ext: 'aar')

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

5. In order to properly collect data, the SDK (and therefore your app) needs Usage Access. You will need to prompt the user to grant this permission then launch the Usage Access info page with the following code:

```java
if (AvenueAnalytics.hasUsageAccessSetting(context)) {
    Intent intent = new Intent(Settings.ACTION_USAGE_ACCESS_SETTINGS);
    startActivity(intent);
}
```

6. Permissions, there these permissions the apps must have in order to get system info, Target SDK is upper than 23 have to implement require permission run-time , see https://developer.android.com/training/permissions/requesting.html. 
``` xml
<uses-permission android:name="android.permission.PACKAGE_USAGE_STATS" />
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
<uses-permission android:name="android.permission.READ_PHONE_STATE" />
<uses-permission android:name="android.permission.ACCESS_WIFI_STATE"/>
```

7. For ProGuard, adding this line in the config file to avoid optimization. This is important because ProGuard might remove some code which causes unpredictable issues.
```
-keep class com.youtility.datausage.** { *; }
```
