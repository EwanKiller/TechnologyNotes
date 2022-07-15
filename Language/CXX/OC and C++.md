# Objective - C 与C++混编

## 文件后缀命名

- C++的文件为 .h和.cpp ；
- Objective-C的文件为.h和.m ；
- 混编的文件为.mm，对于头文件要保持要么全是c++要么全是objective-c ，混写会导致Xcode无法识别；

## 如何在OC中调用C++

```c++
/** CppObject.h */
#include <iostream>
#include <string>
class CppObject
{
public:
    void cppFunc(const std::string& str);
}

/** CppObject.cpp */
#include <CppObject.h>
void CppObject::cppFunc(const std::string& str)
{
    std::cout<< str << std::endl;
}
```

```objective-c
/** OcObject.h */
#import <Foundation/Foundation.h>
#import "CppObject.h"  // error ! Xcode cannot recognize cpp.h.
@interface OcObject:NSObject
{
}   
-(void)ocFunc:(NSString*)str;
@end
```

上面的写法不行，那么就把上面的写的搬到.mm文件里去！

```objective-c
/** OcObject.mm */
#import "OcObject.h"
#import "CppObject.h"
@interface OcObject()
{
    CppObject* cppObj;
}
@end
    
@implement OcObject
    
-(void)OcFunc:(NSString*)str
{
    std::string cpp_str([str UTF8String],[str lengthOfBytesUsingEncoding:NSUTF8StringEncoding]);
    cppObject->cppFunc(cpp_str);
}
```

## 如何在C++中调用OC

```c++
/** CppObject.h */
#include <iostream>
class CppObject
{
    id OcObj; // 在头文件中利用 id 关键字标示
public:
    CppObject();
    ~CppObject();
    void cppFunc();
}

/** OcObject.h*/
#import <Foundation/Foundation.h>
@interface OcObject:NSObject
{   
}   
-(void)ocFunc:(NSString*)str;
@end
    
/** CppObject.mm */
#include "CppObject.h"
#import "OcObject.h"
CppObject::CppObject()
{
    ocObj = [[OcObject alloc]init];
}
CppObject::~CppObject()
{
    if(ocObj)
    {
        [ocObj release]; // 如果是arc，则忽略该句，mrc则需要手动release
    }
}
void CppObject::cppFunc()
{
    [this->ocObj ocFunc];
}
```

## OC与C++的内存管理

- OC
  - alloc和release 

- C++
  - new和delete

```c++
/** OC.mm */
-(id)init
{
    self = [self init];
    if(!cppObj)
    {
        cppObj = new cppObject;
    }
    return self;
}

-(void)dealloc
{
    delete cppObj;
}

/** CPP.mm */
CppObject:CppObject()
{
    ocObj = [[OcObject alloc]init];
}
CppObject::~CppObject()
{
    if(ocObj)
    {
        [ocObj release];  // 如果是arc，则不需要手动release，如果是mrc，则需要手动release
    }
}
```

