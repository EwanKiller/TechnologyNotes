# coding规范

- https://zh-google-styleguide.readthedocs.io/en/latest/google-cpp-styleguide/naming/#id3

## 防卫式声明

- 作用是防止头文件被多次引用时，被重复编译;
    ```c++
    #ifndef __CLASS__
    #define __CLASS__
    ...
    ...
    #endif
    ```
## 参数传递规范
- 尽量能pass by reference就不要pass by value;
- 对于传入的参数，不会被修改的一定要加上const;

## 初始化列表（initialization list）
- 能使用初始化列表就不要在构造函数内初始化
  - 在构造函数内初始化，会多调用一次拷贝，而初始化列表不需要，性能更优;
  - 对于const修饰的变量只能初始化列表初始化，而不能通过赋值初始化;

## 析构函数相关
- 父类或者未来可能成为父类的类，析构函数必须声明为virtual，否则会报错

## 前向引用声明
- 在完整定义之前
  - 只能声明类的对象的指针
  - 不能声明类的对象
```c++
class Forward;

void main(){
  Forward* f; // 未完整定义前，可以声明类的对象指针
  Forward f; // error ,未完整定义前，不能声明类的对象
}

class Forward{
  void Forward();
private:
  int x;
}
```

## 类型赋值

- float 类型赋值时，使用0.0f的形式 
