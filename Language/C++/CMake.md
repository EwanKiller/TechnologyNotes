# cmake 

## cmake command introduction

```cpp
// 表示项目的名称是hello_cmake
project(hello_cmake)

// 限定cmake的版本
cmake_minimum_required(version 2.6)

// 将dir目录的源文件名称复制给变量variable
aux_source_directory(<dir> <variable>)
// 将当前目录中的源文件名称赋值给变量dir_srcs
eg: aux_source_directory( . dir_srcs)

// 指明本项目包含一个子目录src
add_subdirectory(src)

// 创建一个变量，名字是sources，包含了这些cpp
set(sources src/heelo.cpp src/main.cpp)

// 指示变量sources中的源文件需要编译成一个名称为main的可执行文件
// 第一个参数：可执行文件的名称
// 第二个参数：要编译的源文件列表
// 下面等价于：add_executable(main src/hello.cpp src/main.cpp)，因为在上面定义了变量sources
add_executable(main ${sources})

// 从源文件中创建一个静态库，默认生成在构建文件夹；
// 即调用源文件src/main.cpp来生成一个名为libhello_library.a的静态库
add_library(hello_library static src/hello.cpp)

// 指明可执行文件main需要链接到一个名为test的链接库
target_link_libraries(main test)

// 添加一个目录，该目录为库所包含的头文件的目录，并且设置库属性为public
target_include_directories(hello_library public ${project_source_dir}/include)

// 将参数的内容输出到终端
message(status "output this message")
// (无) = 重要消息
// status = 非重要消息
// warning = cmake警告，会继续执行
// author_warning = cmake警告(dev)，会继续执行
// send_error = cmake错误，继续执行，但是会跳过生成的步骤
// fatal_error = cmake错误，终止所有处理过程

// 指明头文件查找路径并保存在变量中
// 在${project_source_dir}/include中找hello.h并设置给变量hello_path
find_path(hello_path hello.h ${project_source_dir}/include) 

// 查找链接库并将结果保存在变量中
// 在/usr/local/lib和/usr/lib中寻找tiff和tiff2的库，如libtiff.so,libtiff2.so
find_library(tiff_library
                names tiff tiff2
                paths /usr/local/lib /usr/lib)

// 提供选项让用户选择是on或者off，默认为off
option(<option_variable> "help string describling option", [initial value])
eg: option(use_mylib "use my library for this project", on)

// 复制input file的内容到output file里，同时替换input file中用@var@或${var}的变量值
// 通常情况下，输入文件以xxx.h.in为后缀，输出文件以xxx.h为后缀
configure_file(<input file> <output file> [options])
options:
// 只拷贝文件，不进行变量替换；指定了newline_style选项时无效；
copyonly 
// 躲避转义，任何的反斜杠(c风格)转义
escape_quotes
// 限制变量替换，让其只替换被@var@引用的变量而不替换${var}
@only
// 指定输出文件中的新行格式；unix和lf的新行是\n,dos和win32和crlf的新行是\r\n；
// 选项在指定了copyonly选项时无效
newline_style
// xxx.h.in中使用#cmakedefine use_configuration就会在xxx.h中变为#define use_configuration
```

## cmake variables

```cpp
// 根源代码目录，工程顶层目录
cmake_source_dir  
// 当前处理的cmakelists.txt所在的路径
cmake_current_source_dir
// 工程顶层目录
project_source_dir
// 运行cmake的目录，外部构建时就是build目录
cmake_binary_dir
// 当前所在的build目录
cmake_cureent_binary_dir
// 项目的build目录
project_binary_dir
```

# cmake method
```cmake
target_include_directories(targetLib PUBLIC|INTERAFCE|PRIVATE otherLib)
```
- PUBLIC:targetLib生成时，头文件targetLib.h包含otherLib.h，且在targetLib.cpp中也包含otherLib.h；

- INTERFACE:targetLib生成时，头文件targetLib.h中包含otherLib.h，但在targetLib.cpp中不包含otherLib.h；

- PRIVATE:targetLib生成时，头文件targetLib.h中不包含otherLib.h,仅在targetLib.cpp中包含otherLib.h；
