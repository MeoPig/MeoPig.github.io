---
title: 前端面试-TypeScript篇
date: 2023-09-18 21:02:15
tags:
---
## 1. TypeScript是什么？与js的区别？

### 一、概念

TypeScript 是 JavaScript 的超集，扩展了 JavaScript 的语法，支持ES6语法，支持面向对象编程的概念，如类、接口、继承、泛型等。
其是一种静态类型检查的语言，提供了类型注解，在代码编译阶段就可以检查出数据类型的错误，ts在编译阶段需要转译成js来运行。

### 二、特性

* 类型批注和编译时的类型检查
* 类型推断：ts中没有批注类型的变量会自动推断变量类型。
* 接口：ts中接口定义了类的结构，接口可以定义对象类型。
* 枚举：ts新增的一种数据结构和类型，相当于一个容器，用来存放常量，使用时候就像对象一样。它既是一种类型，也是一个值。绝大多数 ts 语法都是类型语法，编译后会全部去除，但是 Enum 结构是一个值，编译后会变成 JavaScript 对象，留在代码中。
* 泛型

## 2. TypeScript中泛型是什么？

泛型（Generics）是一种编程语言特性，允许在定义函数、类、接口等时使用占位符来表示类型，而不是具体的类型。
在ts中，定义函数，接口或者类的时候，不预先定义好具体的类型，而在使用的时候在指定类型的一种特性。
使用示例：

1. 泛型函数

   ```javascript
   function identity<T>(arg: T): T {
       return arg;
   }

   // 使用泛型函数
   let result = identity<string>("Hello");
   console.log(result); // 输出: Hello

   let numberResult = identity<number>(42);
   console.log(numberResult); // 输出: 42
   ```
2. 泛型接口

   ```javascript
    // 基本语法
    interface Pair<T, U> {
        first: T;
        second: U;
    }

    // 使用泛型接口
    let pair: Pair<string, number> = { first: "hello", second: 42 };
    console.log(pair); // 输出: { first: 'hello', second: 42 }
   ```
3. 泛型类

   ```javascript
    // 基本语法
    class Box<T> {
        private value: T;

        constructor(value: T) {
            this.value = value;
        }

        getValue(): T {
            return this.value;
        }
    }

    // 使用泛型类
    let stringBox = new Box<string>("TypeScript");
    console.log(stringBox.getValue()); // 输出: TypeScript
   ```

## 3. Ts中什么是方法重载？

方法重载是一种允许函数在不同参数量或者参数类型下具有不同的返回类型或行为的特性/这可以以一种更灵活的方式定义函数。
例如：

```javascript
function hello(name:string): string;
function hello(age:number): string;
function hello(value: string | number): string {
    if (typeof value === 'string') {
        return `Hello, ${value}`;
    }else{
        return `Your age is ${value}`;
    }
}
console.log(hello('Tom'));
console.log(hello(18));
```

## 4. Ts中any和unknown的区别？

any和unknown都是顶级类型，但是unknown更加严格，它不像any那样不做类型检查，反而unknown因为未知性质，不允许访问属性，不允许赋值给其他有明确类型的变量。

## 5. ts中declare关键字是什么？

用于环境声明和你要定义可能在其他位置存在的变量（全局变量）的方法。
如：

```typescript
declare var jQuery: (selector: string) => any;
```

## 6. 说说你对 TypeScript 中枚举类型的理解？应用场景？

**概念**：
枚举是一个被命名的整型常数的集合，用于声明一组命名的常数,当一个变量有几种可能的取值时,可以将它定义为枚举类型。
**使用方式**：

```typescript
enum xxx { ... }
// 声明d为枚举类型Direction
let d: Direction;
```

**应用场景**：
后端返回的字段使用 0 - 6 标记对应的日期，这时候就可以使用枚举可提高代码可读性，如下：

```typescript
enum Days {Sun, Mon, Tue, Wed, Thu, Fri, Sat};

console.log(Days["Sun"] === 0); // true
console.log(Days["Mon"] === 1); // true
console.log(Days["Tue"] === 2); // true
console.log(Days["Sat"] === 6); // true
```

## 7. 说说你对 TypeScript 中接口的理解？应用场景？

**概念**：
接口是一系列抽象方法的声明，是一些方法特征的集合，这些方法都应该是抽象的，需要由具体的类去实现，然后第三方就可以通过这组抽象方法调用，让具体的类执行具体的方法。
**使用方式**：

```typescript
interface User {
    name: string
    age?: number
    readonly isMale: boolean
    say: (words: string) => string
    [key: string]: any //索引签名
}
const getUserName = (user: User) => user.name
```

**应用场景**：
索引签名：但是当我们无法确定对象中有哪些属性时，或者说对象中可以出现任意多个属性时，此时，我们就可以用索引签名类型了。索引签名，主要是用来规定索引和属性值的类型，索引类型只能是 string 类型 或者 number 类型 或者 symbol 类型。主要用于定义具有动态属性的对象，允许通过字符串或数字索引来访问属性。

如果多人开发的都需要用到这个函数的时候，如果没有注释，则可能出现各种运行时的错误，这时候就可以使用接口定义参数变量

```typescript
// 先定义一个接口
interface IUser {
  name: string;
  age: number;
  [key: string]: number;
}

const getUserInfo = (user: IUser): string => {
  return `name: ${user.name}, age: ${user.age}`;
};

// 正确的调用
getUserInfo({name: "koala", age: 18});
//由于加了索引签名，所以也可以
getUserInfo({name: "koala", age: 18, height: 180});
```

## 8. Ts中的数据类型有哪些？

* boolean
* number
* string
* array
* tuple (元组)
* enum （枚举）
* any 任意类型，不进行类型检查
* void 无返回值（常用于函数）
* null undefined
* never 表示不会有返回值
* object

## 9. Ts中never和void的区别？

* void 表示没有任何类型（可以赋值为null undefined）。
* never表示一个不包含值的类型，即表示永远不存在的值。
* 拥有void返回值类型的函数能正常运行，而never返回值类型的函数则无法返回，无法终止，或抛出异常。

## 10. Ts中interface和type的区别？

* interface 和 type 都能定义对象类型，区别在于：interface 支持声明合并和继承，适合定义对象和类契约；
* type 可定义联合、元组、条件类型等更复杂类型。一般优先用 interface，需要联合或复杂类型时用 type。而且type可以声明基本类型别名。

## 11. 说说Ts中的类？

类表示一组相关对象的共享行为和属性。
一个类包含以下内容：

* 构造器
* 属性
* 方法

特性：

* 继承
* 封装
* 多态
* 抽象

## 12. Ts中类型断言是什么？

类型断言（Type Assertion） 是 TypeScript 中一种手动指定值类型的方式，告诉编译器“我比你更清楚这个值的类型”。
**语法**：`let len2 = (value as string).length;`
**使用场景**：

* 从 any 类型中获取更具体的类型
* DOM 元素类型转换（如 document.getElementById() 默认返回 HTMLElement，但你知道它是 HTMLInputElement）

  ```javascript
  const input = document.getElementById('username') as HTMLInputElement;
  input.value = '张三'; // 现在可以访问 value 属性
  ```
