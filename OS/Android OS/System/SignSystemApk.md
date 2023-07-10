# 给app打系统签名

1. 在AndroidManifest.xml中加入'android.uid.system'：

   ```xml
   <?xml version="1.0" encoding="utf-8"?>
   <manifest xmlns:android="http://schemas.android.com/apk/res/android"
     xmlns:tools="http://schemas.android.com/tools"
     package="com.khronos.hello_xr"
     android:installLocation="auto"
     android:versionCode="1"
     android:versionName="1.0"
     android:sharedUserId="android.uid.system">
   ```

2. 在build.gradle中加入以下：

   ```groovy
       signingConfigs {
           release {
               storeFile file("<folder path>/platform.jks.keystore")
               storePassword 'xxxx'
               keyAlias 'xxxx'
               keyPassword 'xxxx'
           }
           debug {
               storeFile file("<folder path>/platform.jks.keystore")
               storePassword 'xxxx'
               keyAlias 'xxxx'
               keyPassword 'xxxx'
           }
       }
   
       buildTypes {
           debug {
               minifyEnabled false
               signingConfig signingConfigs.debug
           }
           release {
               minifyEnabled false
               signingConfig signingConfigs.release
           }
       }
   ```

   