# Cmake for Android

## 利用Clion的Cmake构建编译Android SO

```cmake
# CMakeLists.txt
set(CMAKE_C_COMPILER "${ANDROID_NDK}/toolchains/llvm/prebuilt/arch/bin/clang")
set(CMAKE_CXX_COMPILER "${ANDROID_NDK}/toolchains/llvm/prebuilt/arch/bin/clang++")

# terminal CMake cli

cd project

mkdir build && cd build

cmake
-DCMAKE_SYSTEM_NAME=Android
-DCMAKE_SYSTEM_VERSION=29
-DCMAKE_ANDROID_ARCH_ABI=arm64-v8a
-DCMAKE_ANDROID_NDK=/home/ewan/Android/Sdk/ndk/21.4.7075529
-DCMAKE_C_FLAGS=""
-DCMAKE_CXX_FLAGS=""
-DCMAKE_ANDROID_NDK_TOOLCHAIN_VERSION=clang ../.
```
