# Android Studio 设置

## Application ID 修改

- File - Project Structure - Modules - Default Config - Application ID

## 快速生成JNI

1. 创建Native函数；

```java
package com.ewan.autogenjni;

public class JniLib {
    public native String sendStringReturnString(String s);
    public native void sendFloatArrayReturnVoid(float array[]);
}
```

2. 在Project/app/src/main/cpp目录下打开terminal：

```
javah -jni com.ewan.autogenjni.JniLib
```

3. 会自动生成com_ewan_autogenjni_JniLib.h文件；

## Unity打包的工程打开直接failed

https://segmentfault.com/a/1190000039224031
