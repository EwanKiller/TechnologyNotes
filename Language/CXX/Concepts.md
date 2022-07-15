# Concepts

## 值类型

- 左值  (lvalue)
  - 指向内存位置的表达式;
  - 可寻址的变量，具有持久性，可以被修改;
  - A lvalue is an expression that refers to memory location and allow us to take the address of that memory via the & operator;

- 右值 (rvalue)
  - 内存中某些地址所表示的值;
  - 不可寻址，具有临时性，字面常量或表达式求值过程中产生的临时量，不能被修改;
  
- 分辨依据：取决于要使用的是它在内存中的 ***位置*** 还是它的 ***值*** ;
## 指针（pointer）与引用（reference）

- 指针（pointer）
  - 指向内存地址的变量;

- 引用（reference)
  - 某个已经存在的变量的别名;
  
- 指针与引用的不同点
  - 可以有空指针，但不能有空引用;
  - 指针可以在适合适合初始化，而引用必须在创建时初始化;
  - 指针可以改变指向的对象，而引用指向一个对象后，就不能再指向另一个对象;

- 指针与引用的辨析
  - 引用本身 **不是一个对象**,而是一个 **对象的别名** ,被声明的时候必须初始化,初始化的过程是将引用与初始值 **绑定** 在一起,一旦绑定即不能再修改绑定其他对象;
  - 指针本身就是一个 **对象** ,它指向的是 **一个对象的内存地址** ,可以在使用时再被初始化,指针指向的对象是可以修改的;

- 右值引用
  - 对右值取引用;
  - 解决了 **移动语义(move semantic)** 和 **完美转发(perfect forwarding)** 两个问题;
    - 移动语义
      - 简单讲就是使用std::move将左值引用转换为右值引用,这样可以调用移动构造函数进行对象的构造而不用调用构造函数;
        - 例如f = f(fo())时，fo()是左值，那么f(fo())就会调用拷贝构造函数进行对象的构造，而std:move()可以把fo()由左值引用转为右值引用，这样就可以调用移动构造函数进行对象的构造，不必多调一次用拷贝构造函数产生一次额外拷贝消耗;
        - 拷贝构造函数的执行过程:
         ```c++
         void String(const String &s){
           m_data = new char [strlen(s.m_data)+1];
           strcpy(m_data,s.m_data);
         }
         ```
         所以为了拷贝，需要申请一块新的内存把被拷贝的内容复制过来的内存拷贝操作;
        - 移动构造函数的执行过程：
          ```c++
          void String(String&& s){
            if(s.m_data!=nullptr){
              m_data = s.m_data;
              s.m_data = nullptr;
            }
          }
          ```
          移动拷贝构造函数对于内存的处理不是拷贝，而是转移内存的所有权;
    - 完美转发
      - 解决了下面这个问题
      ```c++
      templete<class Type,class ARG>
      Type* get_instance(ARG arg){
        Type* ret;
        ret = new Type(arg);
        return ret;
      }
      ```
      这一段实现了把ARG类型的参数，pass by value的方式初始化一个Type类型，然后返回指针，但不够灵活;
      ```c++
      templete<class Type,class ARG>
      Type* get_instance(const ARG& arg){
        Type* ret;
        ret = new Type(arg);
        return ret;
      }
      ```
      这里是把pass by value改成了pass by reference的方式传值，但是如果想要传入一个右值，就没办法实现了;
      ```c++
      templete<class Type,class ARG>
      Type* get_instance(ARG&& arg){
        Type* ret;
        ret = new Type(std::forward<ARG>(arg));
        return ret;
      }
      ```
      这里传入的参数 ARG&& 并不是一个右值引用，而是 ***forwarding reference*** ;
      
      std::forward()完美转发会自动推导出传入的变量的类型进行传入;

## 浅拷贝和深拷贝
- 浅拷贝
  - 是按bit进行拷贝;
  - 如果with pointer member的话，就会直接对指针地址进行拷贝，会造成两个指针指向同一块地址,其中任意一个指针对象销毁时，都会调用2次析构函数(dtor)造成内存泄露(memory leak);
- 深拷贝
  - 是对指针指向的内容进行拷贝;
  - 如果with pointer member的话，就会创建一个新的指针，指向一块新的与被拷贝对象大小一致的内存空间，把要拷贝的内容拷贝到新的地址中;
## static 与 heap 和 stack 的联系
- stack
  - 栈;
  - 是一块存在于作用域(scope)内的内存空间;
  - 当一个函数被调用时，函数本身就会形成一个stack，用来存放传入的变量，返回值，函数内的变量声明;
- heap
  - 堆;
  - 由操作系统提供的一块global的内存空间;
  - 由程序员动态分配（dynamic allocated)的若干的区块(blocks);
  - 由new申请的内存空间，在使用完后需要手动delete进行释放，否则会造成内存泄漏;
    - 生成时先申请内存空间(alloc memory)，然后调用构造函数(ctor);
    - 销毁时先调用析构函数(dtor)，然后释放内存空间(free memory);
- static
  - 修饰的对象既不存在heap中也不存在stack中;
  - 被分为local static object与non-local static object:
    - local static object
      - 修饰函数内成员变量，生命周期不会随着离开作用域而结束，与程序保持一致;
      - 在函数第一次被调用的时候初始化，在main函数结束的时候自动析构;
    - non-local static object
      - 修饰类成员变量或函数，即使类对象不存在，依旧可以使用ClassName::func()来进行调用;
      - 修饰全局变量，作用域被限制在定义它的类中;
- 参考链接：
  https://blog.csdn.net/weixin_41042404/article/details/81416130

## class和struct的区别 in C++，以及分别何时使用？
  - 不同点：
    - class的成员默认作用域和继承权限是private;
    - struct的成员默认作用域和继承权限是public;
  - 相同点：
    - 都具有成员变量，构造函数，析构函数;
  - 使用策略：如果不是为了向下兼容C，不推荐使用struct;

## 构造函数(ctor)与析构函数(dtor)
- 构造函数
  - 基类与派生类的调用顺序：
    
    先调用基类的构造函数，再调用派生类的构造函数;
    - 派生类继承了基类的属性，派生类继承的这些属性初始化要看基类是怎么初始化的;
    - 如果基类没有空参的构造函数，派生类必须显式调用基类的构造函数，即Super::func(type n,...);
    - 如果基类有空参的构造函数，则派生类是否显式调用均可;
- 析构函数
  - 基类与派生类的调用顺序:

    先调用派生类的析构函数，再调用基类的析构函数;
  - 基类的析构函数声明为virtual时，用基类的指针去声明一个派生类的对象，当销毁这个指针时会调用派生类的析构函数;

## inheritance with virtual functions
- non-virtual function
  - 你不希望子类(derived class)去重新定义(override)这个函数
- virtual function
  - 你允许子类(derived class)去重新定义(override)这个函数，并且这个虚函数已经有默认的定义了
- pure virtual
  - 你要求子类（derived class)必须去重新定义(override)这个函数，这个函数默认没有定义

## 面向对象编程 类的三种关系
- 复合 composition
  - has-a 
- 继承 inheritance
  - is a
- 委托 delegation
  - 成员变量里含有其他类的指针

## 四种强制转换类型
- static_cast : 良性转换
  - 原有类型转换如int转short，non-const转const；
  - void指针转换为其他原有类型指针；
  - 有类型转换函数，构造转换函数：
  ```c++
  class Complex{
  public:
      Complex(double real = 0.0, double imag = 0.0): m_real(real), m_imag(imag){ }
    	Complex(double real): m_real(read),m_imag(0){ } // 转换构造函数
  public:
      operator double() const { return m_real; }  //类型转换函数
  private:
      double m_real;
      double m_imag;
  };
  
  int main(){
      //下面是正确的用法
      int m = 100;
      Complex c(12.5, 23.8);
      long n = static_cast<long>(m);  //宽转换，没有信息丢失
      char ch = static_cast<char>(m);  //窄转换，可能会丢失信息
      int *p1 = static_cast<int*>( malloc(10 * sizeof(int)) );  //将void指针转换为具体类型指针
      void *p2 = static_cast<void*>(p1);  //将具体类型指针，转换为void指针
      double real= static_cast<double>(c);  //调用类型转换函数
     
      //下面的用法是错误的
      float *p3 = static_cast<float*>(p1);  //不能在两个具体类型的指针之间进行转换
      p3 = static_cast<float*>(0X2DF9);  //不能将整数转换为指针类型
      return 0;
  }
  ```
- reinterpret_cast ： 转换无关类型，完全复制二进制比特位到目标对象，转后的值与原始对象无关，但比特位完全一致，前后无精度损失；即重新解释二进制比特位，比如可以把一个double转换为一个char，并且还可以无损失的转换回来；
```c++
double d = 1.21;
char* p = reinterpret_cast<char*>(&d);
double* dp = reinterpret_cast<double*>(p);
std::cout << *dp;  // 1.21
```
- const_cast：将const/volatile类型转换为 non-const/non-volatile类型；

  ```c++
  const int n = 100;
  int *p = const_cast<int*>(&n); // &n为地址，是一个const int*类型，所以需要转换后才能赋值给non-const int*类型；
  ```

- dynamic_cast 

## 修饰符
- const
  - 含义
    - const修饰的变量或者对象的值不能被修改；
  - 作用
    - const常量具有类型，可以被编译器检查

## 多态性
- 静态绑定
  - 在编译期就把调用函数名与具体函数内容绑定
    - 函数重载
    - 运算符重载
- 动态绑定
  - 在程序运行期才能把调用函数名与具体函数内容绑定
    - 继承
    - 虚函数

## conversion function 转换函数
```cpp
class Fraction{
public:
    Fraction(int num, int len = 1): m_numerator(num), m_denominator(len){}
    operator double() const{
      return (double)(m_numerator/m_denominator);
    }
private:
    int m_numerator;  // 分子
    int m_denominator;  // 分母
};

-----------------------------------------------------
Fraction f(3,5);
double d = 4 + f;  // 将会调用 operator double()将f转换为0.6
```
## non-explicit-one-argument dtor
```cpp
class Fraction{
public:
    Fraction(int num, int len=1):m_numerator(num), m_denominator(len){}
    Fraction operator+(const Fraction& frac){
      return Fraction(this->m_numerator+frac->m_numerator,
      this->m_denominator+frac->m_denominator);
    }
private:
    int m_numerator; // 分子
    int m_denominator; // 分母
};

-----------------------------------------------------
Fraction f(3,5);
double d2 = f + 4; // 调用non-explicit-ctor把4转换成Fraction(4,1)，然后调用operator+
```
## =defualt and =delete

```c++
class Ewan
{
    Ewan() = default;  // 允许默认构造函数
    Ewan(const Ewan &) = delete;  // 删除默认拷贝构造函数，不允许默认拷贝构造函数
};
```

