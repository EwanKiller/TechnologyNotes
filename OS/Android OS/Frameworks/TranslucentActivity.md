# 实现半透明Activity效果

1. 在res/values/themes中的themes.xml中自定义Style：

```xml
<style name="MyTransparent" parent="Theme.AppCompat.NoActionBar">
        <item name="android:windowBackground">#00FFFFFF</item> <!-- 背景色透明度 -->
        <item name="android:windowNoTitle">true</item><!-- 无标题 -->
        <item name="android:windowIsTranslucent">true</item><!-- 半透明,设置为false无透明效果 -->
        <item name="android:backgroundDimEnabled">true</item><!-- 模糊 -->
        <item name="android:windowAnimationStyle">@android:style/Animation.Translucent</item> <!-- 窗口样式Dialog -->
        <item name="android:windowFullscreen">true</item>
    </style>
```

2. AndroidManifest.xml中修改Activity的theme：

```xml
<activity
	android:name=".MainActivity"
	android:exported="true"
        android:label="@string/app_name"
        android:theme="@style/MyTransparent"
        android:screenOrientation="userLandscape"
        >
        <intent-filter>
        	<action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
        </intent-filter>
</activity>
```
