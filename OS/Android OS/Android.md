# Android Project Settings

## could not find tools.jar

- 打开安卓工程中的 gradle.properties
- 加入一行 org.gradle.java.home=/Library/Java/JavaVirtualMachines/jdk1.8.0_291.jdk/Contents/Home

## Android Bluetooth permission in Manifest
```html
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.example.myapplication">
    /* 允许网络Socket */
    <uses-permission android:name="android.permission.INTERNET"/>
    /* 允许程序连接到已配对的蓝牙设备 */
    <uses-permission android:name="android.permission.BLUETOOTH"/>
    /* 允许程序发现和配对蓝牙设备 */
    <uses-permission android:name="android.permission.BLUETOOTH_ADMIN"/>
    /* 允许一个程序访问CellID或WiFi热点来获取粗略的位置 */
    <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>
    /* 允许一个程序访问精良位置(如GPS) */
    <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION"/>
    <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/Theme.MyApplication">
        <activity
            android:name=".MainActivity"
            android:label="@string/app_name"
            android:theme="@style/Theme.MyApplication.NoActionBar">
            <intent-filter>
                // 决定程序最先启动的Activity是“.MainActivity”
                <action android:name="android.intent.action.MAIN" />
				// 决定应用程序是否显示在程序列表里
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    </application>

</manifest>
```



