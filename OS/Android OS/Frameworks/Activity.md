# Activity

## 启动Activity

### 被启动Activity相关设置

1. Manifest中必须加入:

```
<activity
    android:name=".ChildActivity"
    android:exported="true" >
    <intent-filter>
        <category android:name="android.intent.category.DEFAULT" />
    </intent-filter>
</activity>
```

category必须加入，否则会找不到Activity；

### 通过Inten启动

1. intent需要加入flag：`Intent.FLAG_ACTIVITY_NEW_TASK`，否则某些情况会报错，因为startActivity会检查是否拥有该flag；

### Unable to start service Intent in Android 11

* 在Client的Manifest中加入需要引用的service的package name；

```HTML
<queries>
    <package android:name="com.example.ndkbinderservice" />
</queries>

<application
.......
```
