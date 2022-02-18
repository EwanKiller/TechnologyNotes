# Typescript 遇到的问题

## TypeScript编程规范化

- 参数声明，返回值类型不使用隐式声明

## Typescript中区分值类型和引用类型
- 区分值类型和引用类型赋值时的区别
```typescript
    Vec3 A = new Vec3(0,1,3);
    Vec3 B = new Vec3(1,3,4);
    A = B;  // 引用类型赋值，赋的是引用的地址
    A = new Vec3(1,1,1);
    console.log("B equals " + B);
    -----------------------------
    [log] B equals (1,1,1)
```

## 命名规范
- 类型名： PascalCase
- 接口名前不要加I
- 枚举值： PascalCase
- 函数名： camelCase
- 属性/局部变量： camelCase
- 私有属性名不要使用 _ 前缀
- 命名时尽可能使用全称

## switch 作用域问题
- 在typescript或ES6中，switch的 **块级作用域** 是整个switch语句，而不是每个case;

## var和let的区别
- var是函数作用域，在外层循环和内层循环中定义的i是同一个;
- let是块级作用域，在外层循环和内层循环中定义的i不是同一个;

## Typescript接口设计参考

- 看Tencent MGOBE API设计

## const 和readonly的用法

- const 

  - 声明必须初始化；

  - 块级作用域；

  - 深不可变性，但允许子属性改变；

    - ```typescript
      const foo = { bar: 123 };
      foo = { bar: 456 };  // error
      foo.bar = 456;   // right
      ```

- readonly
  - 只能刚刚创建的时候修改其值，之后不能再被改变；
- const 和 readonly的使用区分
  - 当修饰变量的时候用const，修饰属性的时候用readonly；

## 函数命名空间+类

- 方式一：

```typescript
// A.ts
export class A {
    draw() {
        console.log("A");
    }
}
```

```typescript
// B.ts
export class B { 
    draw(){
        console.log("B");
    }
}
```

```typescript
// index.ts
export * from './A';
export * from './B';
```

```typescript
// Test.ts
import * as Ewan from './Index';
export class Test {
    private a: Ewan.A | undefined;
    private b: Ewan.B | undefined;
    
    func() {
      a.draw();
      b.draw();
    }
}
```

-  方式二：
```typescript
namespace Ewan {
		export class A {
    }
}
```

```typescript
namespace Ewan {
  	export class B {
    }
}
```

```typescript
import { Ewan } from './A'
import { Ewan } from './B'  // error !
import * as ewan from './B'
class test {
  	private a: Ewan.A;
  	private b: Ewan.B; // error !
  	private b: ewan.Ewan.B;
}
```

## callback回调函数作用域问题

```typescript
// system.ts
private callback: Function | undefined;

public bindToSys(usrCallback: Function){
    callback = usrCallback;
}
private dispatchSysEvent(eventIndex:number, data:any)
{
    callback.call(this,eventIndex,data);
}
// user.ts
private userCallback(eventIndex:number, data:any){
    // 根据index转换data类型
}
system.bindToSys(userCallback.bind(this)); // 这里bind(this)是为了自定义的callback作用域内，this还是指向user.ts，否则this将会指向system.ts
```

## number?undefined  和 number ｜ undefined

- 区别主要在参数个数上不一样

## 装饰器语法

1. 分类
   - 类装饰器 class decorator
   - 属性装饰器 property decorator
   - 方法装饰器 function decoratior
   - 参数装饰器 parameter decorator

