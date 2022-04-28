# Build For Android

## 通过Cmake交叉编译Android平台libopenxr_loader.so

```cmake
cmake -G Ninja -DCMAKE_SYSTEM_NAME=Android -DCMAKE_ANDROID_ARCH_ABI=arm64-v8a -DCMAKE_ANDROID_NDK=/Users/ewan/Library/Android/sdk/ndk/21.4.7075529 -DCMAKE_SYSTEM_VERSION=30 -DCMAKE_ANDROID_NDK_TOOLCHAIN_VERSION=clang -DCMAKE_BUILD_TYPE=RelWithDebInfo -DANDROID_NATIVE_APP_GLUE=/Users/ewan/Library/Android/sdk/ndk/21.4.7075529/sources/android/native_app_glue
```

