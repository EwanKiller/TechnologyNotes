# Unity for Android

## 打包环境配置

1. build gradle version : 4.0.1
2. gradle version : 6.8

## Native Plugin相关

### build.gradle

```java
apply plugin: 'com.android.library'
android {
    compileSdkVersion 30
    buildToolsVersion '30.0.2'
    ndkVersion '21.0.6113669'

    defaultConfig {
        minSdkVersion 29
        targetSdkVersion 30
        versionCode 1
        versionName "1.0"

        consumerProguardFiles "consumer-rules.pro"
        externalNativeBuild {
            cmake {
                cppFlags "-std=c++11"
                arguments "-DANDROID_TOOLCHAIN=clang"
            }
        }
    }

    buildTypes {
        release {
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android-optimize.txt'), 'proguard-rules.pro'
        }
    }
    compileOptions {
        sourceCompatibility JavaVersion.VERSION_1_8
        targetCompatibility JavaVersion.VERSION_1_8
    }
    externalNativeBuild {
        cmake {
            path file('src/main/cpp/CMakeLists.txt')
            version '3.10.2'
        }
    }
}

dependencies {
    implementation 'androidx.appcompat:appcompat:1.3.1'
    implementation 'com.google.android.material:material:1.4.0'
}
```

### 获取Unity Context

```c#
AndroidJavaObject context = new AndroidJavaClass("com.unity3d.player.UnityPlayer").GetStatic<AndroidJavaObject>("currentActivity");
```

### Unity与Android通信

1. C# call Java

```java
package com.ewan.renderplugin;

public class ImageReader
{
    public void Read(String name) {}
}
```

```c#
AndroidJavaObject javaObject = androidJavaClass.GetStatic<AndroidJavaObject>("com.ewan.renderplugin.ImageReader");
jo.Call("Read", "Name of Image");
```

### Android Java与NDK通信

1. Java call C/C++

```Java
package com.ewan.java;
public class JavaClass
{   
    public native void NativeFunction(int[] intArray);
}
```

```c
int* nativeArray = nullptr;

extern "C" JNIEXPORT void JNICALL Java_com_ewan_java_JavaClass_NativeFunction(JNIEnv *env, jobject obj, jintArray data)
{
    int length = env->GetArrayLength(data);
    jint* array = env->GetIntArrayElements(data, NULL);
    nativeArray = array;
    env->ReleaseIntArrayElements(data, array, NULL);
}
```

### Unity的InputFiled不显示原生输入框

- InputFiled组件下有个Hide Mobile Input即可以隐藏原生输入框，只调用原生键盘；
- 还有个Hide Soft Keyboard可以禁用原生键盘；
