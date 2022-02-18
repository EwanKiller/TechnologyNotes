# 让自己习惯C++

- accustoming yourself to c++

## 1. 视C++为一个语言联邦

- view c++ as a federation of language；
- C++
  - procedural\面向过程
  - object-oriented\面向对象
  - functional\函数式
  - generic\泛型
  - metaprogramming\元编程

## 2. 以const\enum\inline替代#define

- prefer consts enums and inline to #define；
- 对于单纯常量，以const、enum替代#define；
- 对于形似函数的宏（macros），用inline函数替代#define；

```cpp
#define ASPECT_RATIO 1.653
const double AspectRadio = 1.653;

static const int Num = 5; // 某些编译器不能支持声明时，赋初值
enum{Num = 5}；

#define CALL_WITH_MAX(a,b)f((a)>(b)?(a):(b))
template<typename T>
inline void callWithMax(const T& a, const T& b){
    f(a>b?a:b);
}
```

## 3. 尽可能使用const

- use const whenever possible；
- const & pointer
  - const在pointer的左侧，表示指针指向的对象是常量对象；
  - const在pointer的右侧，表示该pointer是常量指针；
- 对于不更改的对象使用const修饰，const可以帮助编译器检测出错误用法；
- const修饰member function的作用是为了该函数可以作用const object；而在const member function内修改non-const object时，需要用mutable声明该non-const obejct；
```cpp
class TextBlock{
public:
    std::size_t length() const;
private:
    char* pText;
    mutable std::size_t textLength;  // 该variables可能总是会被修改，即使在const member function内
    mutable bool lengthIsVaild; // 该variables可能总是会被修改，即使在const member function内 
}；
std::size_t TextBlock::length() const {
    if(!lengthIsVaild){
       textLength = std::strlen(pText);  // 现在修改是允许的
       lengthIsVaild = true;  // 现在修改是允许的
    }
    return textLength;
}
```
- 当non-const member function与const member function有实质等价的实现时，用non-const版本调用const版本有助于减少代码重复；
```cpp
class TextBlock{
public:
    const char& operator[](std::size_t position) const{
        ...  // bounds checking
        ...  // log   
        return text[position];
    }
    char& operator[](std::size_t position) {
        return 
            const_cast<char&>(   // 用const_cast 移除返回值的const修饰
                static_cast<const TextBlock&>(*this)  // 为*this加上const修饰
                    [position]
         );
    }
private:
    std::string text;
}
```
- 思考：是否const member function和non-const member function可以互相调用?
  - const member function内可以调用non-const member function，但反之不行，原因是const member function承诺函数内不改变函数内的对象，但non-const member function没有承诺，所以承诺了的调用没承诺的是可以的，但是反之违背了该承诺；

## 4. 确定对象被使用前已先被初始化

- make sure that objects are initialized before they're used；
- 为内置型对象进行手工初始化；
```cpp
class buildInClass {
    int x = 1; // 手工初始化
}
```
- 构造函数使用成员初始化列表(member initialization list)而不是使用赋值(assignment)；
```cpp
class customClass{
    public:
    customClass():theName(),theOthers(),num(0){} // 无参数构造函数，内置类型需要手工初始化
    
    customClass(std::string& name,std::list<otherClass> others,int inNum):theName(name),theOthers(others),num(inNum){       
    }

    private:
    std::string theName;
    std::list<otherClass> theOthers;
    int num;
}
```
- 为避免‘跨编译单元的初始化顺序问题’，用local static对象替换non-local static对象；
  - 对于local function static object，c++保证在函数运行期间，首次遇上该object定义的时候进行初始化；
```cpp
class pikachu { 
    void pikaPika(){}    
};

pikach& ths(){
    static pikachu& pika; // 声明并初始化 reference
    return pika; // 返回reference
}

class xiaoZhi {
    void Attack(){
        ths().pikaPika();   // 通过reference来调用,而非直接操作object
    }
};
```

# 构造/析构/赋值运算

## 5. 了解C++默默编写并调用了哪些函数

- know what functions c++ silently writes and calls；
- default ctor, copy ctor, copy assignment operator, dtor；
```cpp
class Empty{};  // 当用户定义一个 empty class

// 以下是编译器中的内容
class Empty {
    Empty(){ ... } // default ctor
    Empty(const Empty& rhs){ ... } // copy ctor
    Empty& operator=(const Empty& rhs){ ... }  // copy assignment 
    ~ Empty(){ ... }  // dtor
};
```

## 6. 如果不想使用编译器生成的函数，就应该明确拒绝

- explicitly disallow the use of compiler-generated functions you do not want；
- 对于copy ctor 和 copy assignment ，没有显式声明，但当有调用时编译器就会为其创建；
```cpp
class Empty {
public:
    Empty() { ... }
private:  // 声明为private 外部则不可以调用，避免了自动生成，但无法阻止class内部和friend类调用
    Empty(const Empty&);
    Empty& operator=(const Empty& rhs);
}
// 声明一个base class ，然后只定义不实现copy ctor and copy assignment
class uncopyable{
protected:
    uncopyable() {}
    ~uncopyable() {}
private:
    uncopyable(const uncopyable&);
    uncopyable& operator=(const uncopyable&);
}
// derived class 内就会阻止调用 copy ctor and copy assignment
class Empty:private uncopyable {};
```

## 7.为多态性的基类声明virtual析构函数

- declare destructor virtual in polymorphic base class；

- 为 polymorphic base class  声明virtual 析构函数;

  ```c++
  public class baseClass {
      baseClass();
      virtual ~baseClass();
  };
  
  public class derivedClassA:baseClass {};
  public class derivedClassB:baseClass {};
  
  void main(){
      baseClass* ptr = getDerivedObj(); // 返回一个derived class的ptr
      
      delete ptr;  //这里会引发错误，只调用了derived class的dtor，而base class的dtor没有被调用，于是造成内存泄漏
  }
  ```

  给base class的析构函数声明为virtual就可以解决这个问题；

- 如果类中有virtual函数，那么需要声明virtual 析构函数;如果类中没有virtual函数，但为了防止别人当作基类，需要声明一个pure virtual dtor；

  ```c++
  class noVirtualInClass{
  public:
      ~noVirtualInClass() = 0;  // 即是抽象函数，又有virtual函数，不担心析构函数的问题，但是必须要有定义；
  };
  noVirtualInClass::~noVirtualInClass(){}
  ```
  
  
## 8.不要让异常跑出析构函数

  - prevent exceptions from leaving destructor；
  
  - 如果被析构函数调用的函数可能抛出异常，那么不要让异常跑出析构函数，应该吞下异常（不传播）或者终止程序；
  
  - 如果运行期间需要对抛出的异常做出反应，那么应该提供一个普通函数执行该原本准备在析构函数中执行的函数；
  
	```c++
    class pikachu {
    public:
    	superAttack(); // 百万伏特攻击，但是可能因为电量不足抛出异常
    }
    class xiaoZhi {
    private:
    	pikachu* pika;
    	bool attacked;
    public:
    	void pikachuAttack(){
    		pika->superAttack();
    		attacked = true;
    	}
    	
    	~xiaoZhi(){
    	if(!attacked){
    		try{
    		pika->superAttack();
    		}
    		catch(e){
    		...  // 捕获异常，记录下来，终止程序
    		}
    	}
    }
	```
## 9.不要在构造函数/析构函数运行期间调用virtual函数
- never call virtual function during constructor or destructor；
- 不要在构造，析构期间调用virtual函数，因为virtual函数不会从base class 下降至derived class；
	```c++
	class pikachu {
	public:
    pikachu();
    ~pikachu();
    
    virtual void attack() const = 0;
	}
	pikachu::pikachu(){    
        attack();
	};
	
	class littlePikachu:public pikachu {
	public:
    	virtual void attack() const;
	};
	
	void main(){
    	littlePikachu pika;  // 这里将不会在构造的时候执行littlePikachu的	attack(),而且执行pikachu的attack();
	}
	```

## 10.赋值操作符返回一个 reference to *this

```c++
	pikachu& pikachu::operator= (const pikachu& rhs){
	    ...
	    return *this; // 这里的*this实际上是 完整的传参列表里（const pikachu 	*this,const pikachu& rhs)
	}
```


## 11.在operator=处处理好‘自我赋值’

- handle assignment to self in operator=；

- 确保当进行自我赋值的时候有良好的行为，包括比较‘源目标’ ‘对象目标’是否为同一个，语句的顺序安排，copy and swap；

```c++
class widget{
	void swap(widget& rhs);
};
widget& widget::operator=(const widget& rhs){
    widget temp(rhs);
    swap(temp);
    return *this;
}
```

```c++
widget& widget::operator=(widget rhs){  // rhs是以pass by value的形式传入，所以传入的是一份 copy 复制件
	swap(rhs); 
	return *this;
}
```



## 12.复制对像的时候勿忘每一个成分

- copy all parts of an object；
- copy对象的时候，不光要copy所有的成员变量，还有base class里的成分；

```c++
class pikachu {
public:
	void pikachu(const pikachu& rhs):theName(rhs.theName){};
    pikachu& operator=(const pikachu& rhs){
        theName = rhs.theName;
        return *this;
    }
private:
    string theName;
};		

class littlePikachu {
public:
    void littlePikachu(const littlePikachu& rhs):pikachu(rhs),theHp(rhs.theHP){};
    pikachu& operator=(const lttlePikachu& rhs){
        pikachu::pikachu=(rhs);
        theHP = rhs.theHp;
        return *this;
    }
private:
    int theHp;
};
```

# 资源管理

- resource management；

## 13.以资源对象管理资源

- use objects to manage resources ; 
- 尽量使用RAII(Resoureces acquisition is initialization)对象，如std::auto_ptr 或std:shared_ptr等，他们在使用的时候于构造函数初始化，然后于析构函数delete释放内存；

```c++
class pikachu_ptr {
    // RAII 在对象申请初始化的时候获取对象资源
    pikachu_ptr(){
        pika = new pikach();  // 在ctor中获取pikachu的指针
    }
    
    ~pikachu_ptr(){
        delete pika; // 在dtor中删除指针
    }
    // 返回pika指针
    pikachu* createPikachuPtr(){
        return pika;
    }
    
	pikachu* pika;
}
```

## 14.在资源管理类中小心copy行为

- think carefully copy behavior in resource-managing objects；
- 复制RAII对象的时候要一并复制它管理的资源；

## 15.在资源管理类中提供对原始资源的访问

- provide access to raw resources in resource-managing objects;
- APIs往往要求访问原始资源，所以RAII对象要提供一个其管理之资源的方法；

## 16.成对使用new和delete时采取相同的形式

- use the same form in corresponding uses of new and delete；
- new和delete , new[ ] 和 delete[ ]总是记得成对使用；

## 17.以独立语句将newed对象置入智能指针

-  stores newed objects in smart point with standalon statement；

# 设计与声明

- designs and declarations；

## 18.让接口容易被正确使用，不容易被误用

- make interfaces easy to use correctly and hard to use incorrectly；

## 19.设计class犹如设计type

- treat class design as type design；

## 20.以pass by reference to const 替代pass by value

- prefer pass-by-reference-to-const to pass-by-value；
- 尽量pass-by-reference-to-const替代pass-by-value，因为前者更加高效而去避免了切割问题；
- 对于内置类型，STL迭代器和函数对象来说，pass-by-value往往比较恰当；

## 21.必须返回对象时，别妄想返回reference

- Dont try to return a reference when u must return an object；

## 22.将成员变量声明为private

- declare data members private；

## 23.宁以non-member function 、non-friend function替代member function

- prefer non-member or non-friend function to member function；

- 在c++中，可以把member function 换成统一namespace下的non-member function或non-friend function；

## 24.若所有参数皆需类型转换，请为此采用non-member函数

- declare non-member function when type conversions should apply to all parameters；

## 25.提供一个不抛出异常的swap函数

- consider support for a no-throwing swap function；

# 实现

- implementations

## 26.尽可能延后变量定义式的出现时间

- postpone variable definitions as long as possible；
- 尽可能在使用前进行定义，避免中间因为异常或其他原因并没有使用到该变量；
- 对于for循环中，尽量在循环内部声明临时变量；

## 27.尽量少做转型

- minimize casting；
- 尽量避免转型；
- 如果转型是必要的，使用函数进行封装；
- 使用新型转型而非旧型转型；
  - const_cast : 转除变量的常量性；
  - dynamic_cast : 向下安全转型；
  - static_cast : 强迫隐式转换；

## 28.避免返回handles指向对象内部成分

- avoid returning 'handles' to object internals
- 避免返回'handles'（如reference，pointer，iterator，etc)指向对象内部，使得得到该handles的人可以更改指向的对象内部成分；
- 帮助const成员函数的行为像const；

## 29.为“异常安全”而努力是值得的

- strive for exception-safe code
- 异常安全 1.不泄漏任何资源 2.不允许数据破坏
- 有三种保证：基本型，强烈型，不抛异常型；
- 强烈型可以通过copy and swap来实现，但是并非所有的都值得实现；

## 30.透彻了解inlining里里外外

- 能够没有调用函数的开销，但是由于是替换每一个调用该函数的地方，所以代码会膨胀；
- 在class内声明的时候进行定义，是一种隐喻的方式；
- 过于复杂（带有循环、递归等）或者被声明virtual的函数无法使用；
- 将大多数inlining限制在小型、频繁被调用的函数上；

## 31.将文件间的编译依存关系降至最低

- 相依于声明式而不是定义式；
- 程序库的头文件应该“完全且仅有声明式”；

# 继承与面向对象设计

- inheritance and object-oriented  design

## 32.确定你的public继承塑模出 is-a 的关系

- make sure public inheritance models "is a"
- public inheritance 意味着 “is a”， 如果class B:public class A，那么A可以的，B一定可以，但反之不是；

## 33.避免遮掩继承而来的名称

- avoid hiding inherited names；
- derived class内的名称会遮掩掉base class内的名称；
- 被遮掩的名称可以使用using或forwarding functions来使得其在derived class的作用域内可见；

## 34.区分接口继承和实现继承

- defferentiate between inheritance of interface and inheritance of implementations;
- pure virtual - 只接口继承，实现由derived class；
- impure virtual - 接口继承和一份缺省的实现继承，derived class可以选择性重写；
- non-virtual - 接口继承和强制性的实现继承，derived class不能够修改实现；

## 35.考虑virtual函数以外的其他选择

- consider alternatives to virtual function；
- NVI手法（non-virtual interface）：通过public non-virtual function调用private virtual function；
- 将virtual function改为函数指针成员；

## 36.绝对不重新定义继承而来的non-virtual function

- never redefine an inherited non-virtual function；

## 37. 绝对不重新定义继承而来的缺省参数

- never redefine a function's inheritanted default parameter value

```c++
class shape {
public:
    virtual void draw(ShapeColor color = RED) const = 0;
}
class rectangle:public shape {
public:
    virtual void draw(ShapeColor color = GREEN) const = 0; // 错误，改变了缺省参数值
}
```

缺省参数值是静态绑定，而virtual函数是动态绑定；

## 38.通过组合实现' has-a '或' is-implemented-in-term-of ' 

- model ' has - a ' or ' is - implemented - in - term - of ' through composition；
- 复合与public继承完全不同，复合表示‘has - a’，而public表示的是‘is - a’；

## 39.明智而慎审的使用private继承
- use private inheritance judiciously
- private继承以为是 is-implemented-in-terms-of，它的级别比组合要低，当为了访问protected base class的成员，或者需要重新定义继承而来的virtual函数时；

## 40.明智而谨慎的使用多重继承

- use multiple inheritance judiciously

# 模版和泛型编程
- templete and generic programming

## 41.了解隐式接口和编译器多态

- understand implicit interface and compile-time polymorphism；
- class和templete都支持接口interface和多态polymorphism；
- 对于class来说interface是显式的，以函数签名为中心，多态则是通过virtual函数发生在运行期；
- 对于template来说interface是隐式的，奠基于有效表达式，多态则是通过template具现化和函数重载解析；

## 42.了解typename的双重意义

- understand the two meanings of typename



# 书籍链接

http://aleda.cn/books/Effective_C++.pdf
