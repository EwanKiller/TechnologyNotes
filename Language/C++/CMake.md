# CMake 

## CMake Command Introduction

```cpp
// 表示项目的名称是hello_cmake
PROJECT(hello_cmake)

// 限定CMake的版本
CMAKE_MINIMUM_REQUIRED(VERSION 2.6)

// 将dir目录的源文件名称复制给变量variable
AUX_SOURCE_DIRECTORY(<dir> <variable>)
// 将当前目录中的源文件名称赋值给变量DIR_SRCS
eg: AUX_SOURCE_DIRECTORY( . DIR_SRCS)

// 指明本项目包含一个子目录src
ADD_SUBDIRECTORY(src)

// 创建一个变量，名字是SOURCES，包含了这些cpp
SET(SOURCES src/Heelo.cpp src/main.cpp)

// 指示变量SOURCES中的源文件需要编译成一个名称为main的可执行文件
// 第一个参数：可执行文件的名称
// 第二个参数：要编译的源文件列表
// 下面等价于：ADD_EXECUTABLE(main src/Hello.cpp src/main.cpp)，因为在上面定义了变量SOURCES
ADD_EXECUTABLE(main ${SOURCES})

// 从源文件中创建一个静态库，默认生成在构建文件夹；
// 即调用源文件src/main.cpp来生成一个名为libhello_library.a的静态库
ADD_LIBRARY(hello_library STATIC src/Hello.cpp)

// 指明可执行文件main需要链接到一个名为Test的链接库
TARGET_LINK_LIBRARIES(main Test)

// 添加一个目录，该目录为库所包含的头文件的目录，并且设置库属性为PUBLIC
TARGET_INCLUDE_DIRECTORIES(hello_library PUBLIC ${PROJECT_SOURCE_DIR}/include)

// 将参数的内容输出到终端
MESSAGE(STATUS "Output this message")
// (无) = 重要消息
// STATUS = 非重要消息
// WARNING = CMake警告，会继续执行
// AUTHOR_WARNING = CMake警告(dev)，会继续执行
// SEND_ERROR = CMake错误，继续执行，但是会跳过生成的步骤
// FATAL_ERROR = CMake错误，终止所有处理过程

// 指明头文件查找路径并保存在变量中
// 在${PROJECT_SOURCE_DIR}/Include中找hello.h并设置给变量HELLO_PATH
FIND_PATH(HELLO_PATH hello.h ${PROJECT_SOURCE_DIR}/Include) 

// 查找链接库并将结果保存在变量中
// 在/usr/local/lib和/usr/lib中寻找tiff和tiff2的库，如libtiff.so,libtiff2.so
FIND_LIBRARY(TIFF_LIBRARY
                NAMES tiff tiff2
                PATHS /usr/local/lib /usr/lib)

// 提供选项让用户选择是ON或者OFF，默认为OFF
OPTION(<option_variable> "help string describling option", [initial value])
eg: option(USE_MYLIB "Use my library for this project", ON)

// 复制input file的内容到output file里，同时替换input file中用@VAR@或${VAR}的变量值
// 通常情况下，输入文件以xxx.h.in为后缀，输出文件以xxx.h为后缀
configure_file(<input file> <output file> [options])
options:
// 只拷贝文件，不进行变量替换；指定了NEWLINE_STYLE选项时无效；
COPYONLY 
// 躲避转义，任何的反斜杠(C风格)转义
ESCAPE_QUOTES
// 限制变量替换，让其只替换被@VAR@引用的变量而不替换${VAR}
@ONLY
// 指定输出文件中的新行格式；UNIX和LF的新行是\n,DOS和WIN32和CRLF的新行是\r\n；
// 选项在指定了COPYONLY选项时无效
NEWLINE_STYLE
// xxx.h.in中使用#cmakedefine USE_CONFIGURATION就会在xxx.h中变为#define USE_CONFIGURATION
```

## CMake Variables

```cpp
// 根源代码目录，工程顶层目录
CMAKE_SOURCE_DIR  
// 当前处理的CMakeLists.txt所在的路径
CMAKE_CURRENT_SOURCE_DIR
// 工程顶层目录
PROJECT_SOURCE_DIR
// 运行cmake的目录，外部构建时就是build目录
CMAKE_BINARY_DIR
// 当前所在的build目录
CMAKE_CUREENT_BINARY_DIR
// 项目的build目录
PROJECT_BINARY_DIR
```