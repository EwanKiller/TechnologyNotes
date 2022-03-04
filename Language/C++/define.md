# c/c++中的#define的用法

## 无参宏定义
```c++
#define Pi 3.1415926;
```

## 有参宏定义
```c++
#define Add(x,y) x+y; 
```

## 特殊操作符
```c++
// #
// x => "x"
#define FUNC(x) #x

// ##
// string => PRE_string
#define PREFIX(string) PRE_##string

// @#
// x => 'x'
#define FUNC(x) @#x

// Log("Test") => Ewan:Test
#define TAG "Ewan"
#define Log(...) std::cout << TAG << ":" << __VA_ARGS__ << std::endl
```