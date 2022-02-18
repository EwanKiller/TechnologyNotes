# export .so

1. Preference/Build，Execution，Deployment/CMake/CMake Options里填入

```cmake
-DCMAKE_SYSTEM_NAME=Android
-DCMAKE_ANDROID_ARCH_ABI=armeabi-v7a // 这里需要声明对应的架构
-DCMAKE_ANDROID_NDK=/Users/ewan/Library/Android/sdk/ndk/21.4.7075529 // 指明NDK路径
-DCMAKE_SYSTEM_VERSION=30
-DCMAKE_C_FLAGS=""
-DCMAKE_CXX_FLAGS=""
-DCMAKE_ANDROID_NDK_TOOLCHAIN_VERSION=clang
```

2. CMakeList.txt

```
cmake_minimum_required(VERSION 3.20)
project(SeedXRController) // 指明项目名称

set(CMAKE_CXX_STANDARD 14)

file(GLOB sourceFiles log/*.h log/*.cpp log/*.c) // 拓展自己写的log函数，方便so能输出到logcat

add_library(SeedXRController SHARED SeedXRController.cpp) // 指定so的名字和脚本

find_library(
        log-lib 
        log) // log-lib必须加上才能在os中输出log

target_link_libraries(
        SeedXRController
        ${log-lib})

include_directories("Headers")  // 引入头文件
```

3.让so可以输出log到android studio的logcat

```
// log
#ifndef SEEDXRCONTROLLER_LOG_TOOL_H
#define SEEDXRCONTROLLER_LOG_TOOL_H

#include <android/log.h>

#define TAG "Ewan Native"

#define LOGD(...)__android_log_print(ANDROID_LOG_DEBUG, TAG, __VA_ARGS__)

#endif //SEEDXRCONTROLLER_LOG_TOOL_H
```

