# Build For Android

## 通过Cmake交叉编译Android平台libopenxr_loader.so

1. 修改src/CMakeLists.txt中：`find_path(ANDROID_NATIVE_APP_GLUE NAMES android_native_app_glue.h PATHS ${ANDROID_NDK}/sources/android/native_app_glue)`为`set(ANDROID_NATIVE_APP_GLUE ${ANDROID_NDK}/sources/android/native_app_glue)`


2. 命令行执行（先需要安装Ninja）：


```cmake
cmake -G Ninja -DCMAKE_SYSTEM_NAME=Android -DCMAKE_ANDROID_ARCH_ABI=arm64-v8a -DANDROID_NDK=/Users/ewan/Library/Android/sdk/ndk/21.4.7075529 -DCMAKE_SYSTEM_VERSION=30 -DCMAKE_ANDROID_NDK_TOOLCHAIN_VERSION=clang -DCMAKE_BUILD_TYPE=RelWithDebInfo  ../..
```

