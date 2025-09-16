---
title: 前端面试-js篇
date: 2025-08-30 16:04:20
tags:
---
## 1.js中的数据类型？

1. number、string、boolean、null、undefined、object、symbol
   （symbol 是 ES6 新增的数据类型，用于创建唯一标识符。）、bigint
2. 引用数据类型: 对象Object（包含普通对象-Object，数组对象-Array，正则对象-RegExp，日期对象-Date，数学函数-Math，函数对象-Function）

---

## 2. 0.1 + 0.2 = 0.3 的结果是多少？

0.1和0.2在转换成二进制后会无限循环，由于标准位数的限制后面多余的位数会被截掉，此时就已经出现了精度的损失，相加后因浮点数小数位的限制而截断的二进制数字在转换为十进制就会变成0.30000000000000004.

## 3.typeof能否判断正确类型？

typeof 1 返回 number
typeof '1' 返回 string
typeof true 返回 boolean
typeof undefined 返回 undefined
typeof symbol() 返回 symbol

但对于引用数据类型，typeof 无法判断正确的类型，除了函数之外，都会显示object
typeof {} 返回 object
typeof [] 返回 object
typeof console.log 返回 function

## 4. instanceof ？

instanceof 运算符用于检测构造函数的 prototype 属性是否出现在某个实例对象的原型链上。
instanceof的原理是基于原型链的查询，只要处于原型链中，判断永远为true。

## 5. typeof 和instanceof 区别？

typeof与instanceof都是判断数据类型的方法，区别如下：

* typeof会返回一个变量的基本类型，instanceof返回的是一个布尔值。
* instanceof 可以准确地判断复杂引用数据类型，但是不能正确判断基础数据类型。
* 而typeof 也存在弊端，它虽然可以判断基础数据类型（null 除外），但是引用数据类型中，除了function 类型以外，其他的也无法判断。

## 6.手动实现一个instanceof

核心：获取对象的原型，判断对象的原型是否等于构造函数的 prototype 对象。原型链的向上查找。

```javascript
function myInstanceof(left, right) {
    if (typeof left !== 'object' || left === null) return false;
    let proto = Object.getPrototypeOf(left);
    while (true) {
        if (proto === null) return false;
        if (proto === right.prototype) return true;
        proto = Object.getPrototypeOf(proto);
    }
}
console.log(myInstanceof('111', String)); //false
console.log(myInstanceof(new String('111'), String)); //true
```

---

## 7.object.is和===区别？

* NaN 和 NaN 相等
* 0 和 -0 不相等

---

## 8. js类型转换？

三种：

* 转换成数字
* 转换成布尔值
* 转换成字符串

![ ](8-1.png)

## 9. === 和 == 的区别？

`===` 严格相等，左右两边不仅值要相等，类型也要相等。
`==` 允许类型转换，只要两边的值相等，就返回true。

`==`不像 `===`那样严格，对于一般情况，只要值相等，就返回true，但==还涉及一些类型转换，它的转换规则如下：

* 两边的类型是否相同，相同的话就比较值的大小，例如1==2，返回false。
* 判断的是否是null和undefined，是的话就返回true。
* 判断的类型是否是String和Number，是的话，把String类型转换成Number，再进行比较。
* 判断其中一方是否是Boolean，是的话就把Boolean转换成Number，再进行比较。
* 如果其中一方为Object，且另一方为String、Number或者Symbol，会将Object转换成字符串，再进行比较。

## 10. js闭包？

概念：闭包是指有权访问另一个函数作用域中的变量的函数。闭包=函数+词法环境。闭包就是能够读取其他函数内部变量的函数。

```javascript
function m(){
    var a=0;
    function sub(){
    }
}
```

特性：

* 函数内再嵌套函数
* 内部函数可以引用外层的参数和变量
* 参数和变量不会被垃圾回收机制回收

表现形式：

1. 返回一个函数
2. 作为函数参数传递

   ```javascript
   var a=0;
   function m(){
       var a=1;
       function sub(){
           console.log(a);
       }
       bar(sub);
   }
   function bar(fn){
       fn();
   }
   m();//2
   ```
3. 在定时器、事件监听、Ajax请求、跨窗口通信、Web Workers或者任何异步中，只要使用了回调函数，实际上就是在使用闭包

   ```javascript
   // 定时器
   setTimeout(function timeHandler(){
   console.log('111');
   }，100)
   // 事件监听
   $('#app').click(function(){
   console.log('DOM Listener');
   })
   ```
4. IIFE(立即执行函数表达式)创建闭包, 保存了 全局作用域window 和 当前函数的作用域 ，因此可以全局的变量

   ```javascript
   var a = 2;
   (function IIFE(){
   // 输出2
   console.log(a);
   })();
   ```

理解：

* 使用闭包主要是为了设计私有的⽅法和变量。闭包的优点是可以避免全局变量的污染，缺点是闭包会常驻内存，会增⼤内存使用量，使用不当很容易造成内存泄露。在js中，函数即闭包，只有函数才会产⽣作用域的概念。
* 闭包 的最⼤用处有两个，⼀个是可以读取函数内部的变量，另⼀个就是让这些变量始终保持在内存中。
* 闭包的另⼀个用处，是封装对象的私有属性和私有⽅法
* 好处：能够实现封装和缓存等；
* 坏处：就是消耗内存、不正当使用会造成内存溢出的问题

注意点：

* 由于闭包会使得函数中的变量都被保存在内存中，内存消耗很⼤，所以不能滥用闭包，否则会造成网⻚的性能问题，在IE中可能导致内存泄露。
* 解决⽅法是，在退出函数之前，将不使用的局部变量全部删除

---

## 11.原型对象和构造函数的关系？

每当定义一个函数数据类型（普通函数、类）时候，会天生自带一个prototype属性，这个属性指向函数的原型对象。
当函数经过new调用时，这个函数就成了构造函数，会返回一个全新的实例对象，这个实例对象有proto属性，指向构造函数的原型对象。

---

## 12. 原型链

![ ](12-1.png)

---

## 13. this对象？

* this总是指向函数的直接调用者（而非间接调用者）
* 如果有 new 关键字， this 指向 new 出来的那个对象
* 在事件中， this 指向触发这个事件的对象，特殊的是， IE 中的 attachEvent 中的this 总是指向全局对象 Window

---

## 14. js继承

[继承](https://blog.csdn.net/wtswts1232/article/details/130683362)：子类继承父类的属性和方法，并扩展新的属性和方法
继承的实现方式：原型链、对象组合、类继承、组合继承、寄生组合继承

1. 原型链继承

   ```javascript
   // 定义父类
   function Parent() {
       this.name = "parent";
   }
   // 在父类的原型上定义方法
   Parent.prototype.getName = function() {
       return this.name;
   }
   // 定义子类
   function Child() {
       this.name = "child";
   }
   // 子类继承父类，这里是关键，实现原型链继承
   Child.prototype = new Parent();
   // 实例化子类
   var child1 = new Child();
   console.log(child1.getName()); // 输出 "child"
   ```

   缺点：

   * 父类引用属性被所有子类实例共享。如果一个实例改变了该属性，那么其他实例的该属性也会被改变。
2. 构造函数继承：
   通过使用call或者apply方法,可以在子类中执行父类的构造函数，从而实现继承。

   ```javascript
        // 父类
    function Parent() {
    this.sayHello = function () {
        console.log("Hello");
    };
    }

    Parent.prototype.a = "我是父类prototype上的属性";
    // 子类
    function Child() {
    Parent.call(this);
    }

    // 创建两个 Child 实例
    var child1 = new Child();
    var child2 = new Child();

    console.log(child1.sayHello === child2.sayHello); // 输出 false
   ```

   优点：不会出现原型链继承出现的问题。
   缺点：它不能继承父类 prototype 上的属性
3. 组合继承

   原型链继承 + 构造函数继承

   ```javascript
    // 父类
    function Parent() {
    this.sayHello = function () {
        console.log("Hello");
    };
    }

    Parent.prototype.a = "我是父类prototype上的属性"; 

    // 子类
    function Child() {
    Parent.call(this);
    }

    Child.prototype = new Parent();

    var child1 = new Child();
   ```

   优点：解决了构造继承的问题
   缺点：调用了 2 次 Parent()。 它在 child 的 prototype 上添加了父类的属性和方法。
4. 寄生组合继承

   ```javascript
         // 父类
      function Parent() {
        this.sayHello = function () {
          console.log("Hello");
        };
      }

      Parent.prototype.a = "我是父类prototype上的属性";
      // 子类
      function Child() {
        Parent.call(this);
      }


      // 创建一个没有实例方法的父类实例作为子类的原型
      Child.prototype = Object.create(Parent.prototype);
      // 修复构造函数的指向
      Child.prototype.constructor = Child;

      var child1 = new Child();
   ```

   object.create(Parent.prototype)  创造了一个空对象，这个空对象的__proto__ 是 Parent.prototype 。所以它继承了 Parent 原型链上的属性和方法。由于我们删除了Child.prototype = new Parent(); 我们不再调用 Parent() 构造函数，因此 Child.prototype 不再包含 Parent 的属性和方法。所以第三小节部分提到的问题就解决了，Child.prototype 上不再有 sayHello 方法。
   优点：

   * 原型属性不会被共享
   * 可以继承父类的属性和方法
   * 只调用1次Parent(),因此它不会再child的prototype上添加Parent的属性和方法
     缺点：
     child.prototype的原始属性和方法会丢失

---

## 15. js浅拷贝和深拷贝

JS 分为原始类型和引用类型，对于原始类型的拷贝，并没有深浅拷贝的区别，我们讨论的深浅拷贝都只针对引用类型。

* 浅拷贝和深拷贝都复制了值和地址，都是为了解决引用类型赋值后互相影响的问题。
* 但是浅拷贝只进行一层复制，深层次的引用类型还是共享内存地址，原对象和拷贝对象还是会互相影响。
* 深拷贝就是无限层级拷贝，深拷贝后的原对象不会和拷贝对象互相影响。

### 浅拷贝手段

1. 手动实现

   ```javascript
   const shallowClone = (target) => {
    if (typeof target === 'object' && target !== null) {
        const cloneTarget = Array.isArray(target) ? []: {};
        for (let prop in target) {
            if (target.hasOwnProperty(prop)) {
                cloneTarget[prop] = target[prop];
            }
        }
        return cloneTarget;
    } else {
        return target;
    }
    }
   ```
2. object.assign()
   Object.assign() 拷贝的是对象的属性的引用，而不是对象本身。
   Object.assign 只会拷⻉所有的属性值到新的对象中，如果属性值是对象的话，拷⻉的是地址，所以并不是深拷⻉

   ```javascript
    let obj = { name: 'sy', age: 18 };
    const obj2 = Object.assign({}, obj, {name: 'sss'});
    console.log(obj2);//{ name: 'sss', age: 18 }
   ```
3. concat

   ```javascript
    let arr = [1, 2, 3];
    let newArr = arr.concat();
    newArr[1] = 100;
    console.log(arr);//[ 1, 2, 3 ]
   ```
4. slice

   ```javascript
    let arr = [1, 2, 3];
    let newArr = arr.concat();
    newArr[1] = 100;
    console.log(arr);//[ 1, 2, 3 ]
   ```
5. ...展开运算符

   ```javascript
   let arr = [1, 2, 3];
   let newArr = [...arr];//跟arr.slice()是一样的效果
   ```

### 深拷贝手段

1. JSON.parse(JSON.stringify())
   这种方法虽然可以实现数组或对象深拷贝，但不能处理函数对象或undefined（会显示为null）；结构复杂的数据function、undefined、Symbol会被过滤掉。
2. lodash库 cloneDeep()

---

## 16. 事件代理？

事件代理，也叫事件委托，是事件处理机制的一种，事件代理的原理是把事件绑定在父元素上，然后通过事件冒泡，把事件传递给子元素。
事件的发生经历三个阶段：捕获阶段（ capturing ）、目标阶段
（ targetin ）、冒泡阶段（ bubbling ）
适合事件委托的事件有：click，mousedown，mouseup，keydown，keyup，keypress。
从上面应用场景中，我们就可以看到使用事件委托存在两大优点：

* 减少整个页面所需的内存，提升整体性能
* 动态绑定，减少重复工作

但是使用事件委托也是存在局限性：

* focus、blur这些事件没有事件冒泡机制，所以无法进行委托绑定事件
* mousemove、mouseout这样的事件，虽然有事件冒泡，但是只能不断通过位置去计算定位，对性能消耗高，因此也是不适合于事件委托的

## 17. new操作符都干了什么？

* 创建⼀个空对象，并且 this 变量引用该对象，同时还继承了该函数的原型
* 属性和⽅法被加⼊到 this 引用的对象中
* 新创建的对象由 this 所引用，并且最后隐式的返回 this

## 18. 谈谈你对ES6的理解

* 新增模板字符串（为 JavaScript 提供了简单的字符串插值功能）
* 箭头函数
* for-of （用来遍历数据—例如数组中的值。）
* arguments 对象可被不定参数和默认参数完美代替。
* ES6 将promise 对象纳⼊规范，提供了原⽣的 Promise 对象。
* 增加了 let 和 const 命令，用来声明变量。
* 增加了块级作用域。
* let 命令实际上就增加了块级作用域。
* 还有就是引⼊ module 模块的概念。

---

## 19. let var const区别？

1. 作用域区别
   let和const具有块级作用域，var不存在块级作用域,可以跨块访问, 不能跨函数访问

   ```javascript
    if(true){
        var a = 0
        let b = 0
        const c = 0
    }
    console.log(a);
    console.log(b);
    console.log(c);
    //0，这里只有var声明的变量才能打印出来，因为var声明的事全局变量
   ```

   var出来的变量是全局的，但是不能跨函数访问。

   ```javascript
    function test() {
        var message = "zimo";   // 局部变量
    }
    test();
    console.log(message);  // 报错
   ```
2. 变量提升
   变量能在声明之前使用，就是变量提升。
   var存在变量提升，let和const不存在变量提升
3. 全局属性
   浏览器的全局对象是window。var声明的变量为全局变量，并且会将该变量添加为全局对象的属性，但是let和const不会。

   ```javascript
    var a = 1
    let b = 2
    const c = 3

    console.log(window.a);
    console.log(window.b);
    console.log(window.c);
    //1,undefined,undefined
   ```
4. 重复声明
   var声明变量时，可以重复声明变量，后声明的同名变量会覆盖之前声明的遍历。const和let不允许重复声明变量。
5. 暂时性死区
   在使用let、const命令声明变量之前，该变量都是不可用的。这在语法上，称为暂时性死区。使用var声明的变量不存在暂时性死区。但是访问时会undefined
6. 初始值设置
   在变量声明时，var 和 let 可以不用设置初始值。而const声明变量必须设置初始值。
7. 指针指向
   指针指向：let和const都是ES6新增的用于创建变量的语法。 let创建的变量是可以更改指针指向（可以重新赋值）。但const声明的变量是不允许改变指针的指向。
8. 应用场景
   声明对象类型使用 const，非对象类型声明选择 let

## 20. Dom 操作

1. 创建节点

   ```javascript
    createDocumentFragment() //创建⼀个DOM⽚段
    createElement() //创建⼀个具体的元素
    createTextNode() //创建⼀个文本节点
   ```
2. 添加、移除、替换、插入

   ```javascript
    appendChild() //添加
    removeChild() //移除
    replaceChild() //替换
    insertBefore() //插⼊
   ```
3. 查找

   ```javascript
    getElementsByTagName() //通过标签名称
    getElementsByName() //通过元素的Name属性的值
    getElementById() //通过元素Id，唯⼀性
   ```

## 21. 防抖与节流

### 防抖

为了防止快速且频繁的触发事件而导致多次执行事件函数，我们这多次触发的事件只执行一次事件函数。
手动实现：

```javascript
const debounce = (func, wait) => {
    let timer
    return () => {
      clearTimeout(timer)
      timer = setTimeout(func, wait);
    }
}
```

函数防抖在执行目标方法时，会等待一段时间。当又执行相同方法时，若前一个定时任务未执行完，则 清除掉定时任务，重新定时。
对于按钮防点击来说的实现

* 开始⼀个定时器，只要我定时器还在，不管你怎么点击都不会执行回调函数。⼀旦定时器结束并设置为 null，就可以再次点击了
* 对于延时执行函数来说的实现：每次调用防抖动函数都会判断本次调用和之前的时间间隔，如果⼩于需要的时间间隔，就会重新创建⼀个定时器，并且定时器的延时为设定时间减去之前的时间间隔。⼀旦时间到了，就会执行相应的回调函数

### 节流

防抖动和节流本质是不⼀样的。防抖动是将多次执行变为最后⼀次执行，节流
是将多次执行变成每隔⼀段时间执行

```javascript
const throttle = (func, wait) => {
    let timer;
  
    return () => {
        if (timer) {
            return
        }
        timer = setTimeout(() => {
            func();
            timer = null
        }, wait)
    }
}
```

函数节流的目的，是为了限制函数一段时间内只能执行一次。因此，通过使用定时任务，延时方法执行。在延时的时间内，方法若被触发，则直接退出方法。从而实现一段时间内只执行一次。

### 异同比较

相同点：

* 都可以通过使用 setTimeout 实现
* 目的都是，降低回调函数的执行频率，节省计算资源
  不同点：
* 函数防抖，是在一段连续操作结束之后，处理回调，利用 clearTimout 和 setTimeout 实现。
* 函数节流，是在一段连续操作中，每一段时间只执行一次，在频率较高的事件中使用来提高性能。
* 函数防抖关注一段时间内连续触发，只在最后一次执行；而函数节流侧重于在一段时间内只执行一次。

---

## 22. 事件流

事件流分为两种，捕获事件流和冒泡事件流

* 捕获事件流从根节点开始执行，⼀直往⼦节点查找执行，直到查找执行到目标节点
* 冒泡事件流从目标节点开始执行，⼀直往父节点冒泡查找执行，直到查到到根节点
* 事件流分为三个阶段，⼀个是捕获节点，⼀个是处于目标节点阶段，⼀个是冒泡阶段

---

## 23. 描述一下this

this ，函数执行的上下文，可以通过 apply ， call ， bind 改变 this
的指向。对于匿名函数或者直接调用的函数来说，this指向全局上下文（浏览器为window，NodeJS为 global ），剩下的函数调用，那就是谁调用它，this 就指向谁。当然还有es6的箭头函数，箭头函数的指向取决于该箭头函数声明的位置，在哪里声明， this 就指向哪里。

---

## 24. ajax、axios、fetch区别？

1. jQuery ajax

   ```javascript
   $.ajax({
      type: 'POST',
      url: url,
      data: data,
      dataType: dataType,
      success: function () {},
      error: function () {}
   });
   ```

   优缺点：

   * 本身是针对 MVC 的编程,不符合现在前端 MVVM 的浪潮
   * 基于原⽣的 XHR 开发， XHR 本身的架构不清晰，已经有了 fetch 的替代⽅案
2. axios

   ```javascript
   axios({
      method: 'post',
      url: '/user/12345',
      data: {
      firstName: 'Fred',
      lastName: 'Flintstone'
      }
   })
   .then(function (response) {
   console.log(response);
   })
   .catch(function (error) {
   console.log(error);
   });
   ```

   优缺点：

   * 从浏览器中创建 XMLHttpRequest
   * 从 node.js 发出 http 请求
   * 支持 Promise API
   * 拦截请求和响应
   * 转换请求和响应数据
   * 取消请求
   * ⾃动转换 JSON 数据
   * 客户端支持防⽌ CSRF/XSR
3. fetch

   ```javascript
   try {
      let response = await fetch(url);
      let data = response.json();
      console.log(data);
   } catch(e) {
      console.log("Oops, error", e);
   }
   ```

   优缺点：

   * fetch 只对网络请求报错，对 400,500 都当做成功的请求，需要封装去处理
   * fetch 默认不会带 cookie ，需要添加配置项
   * fetch 不支持 abort ，不支持超时控制，使用 setTimeout 及 Promise.reject 的实现的超时控制并不能阻⽌请求过程继续在后台运行，造成了量的浪费
   * fetch 没有办法原⽣监测请求的进度，⽽XHR可以

---

## 25. js作用域和作用域链？

### 作用域

Javascript中的作用域说的是变量的可访问性和可见性。也就是说整个程序中哪些部分可以访问这个变量，或者说这个变量都在哪些地方可见。有三种作用域：全局作用域；函数作用域；块级作用域

* 全局作用域
  任何不在函数中或是大括号中声明的变量，都是在全局作用域下，全局作用域下声明的变量可以在程序的任意位置访问。
* 函数作用域
  函数作用域也叫局部作用域，如果一个变量是在函数内部声明的它就在一个函数作用域下面。这些变量只能在函数内部访问，不能在函数以外去访问。
* 块级作用域
  ES6引入了let和const关键字,和var关键字不同，在大括号中使用let和const声明的变量存在于块级作用域中。在大括号之外不能访问这些变量。

```javascript
{
  // 块级作用域中的变量
  let greeting = 'Hello World!';
  var lang = 'English';
  console.log(greeting); // Prints 'Hello World!'
}
// 变量 'English'
console.log(lang);
// 报错：Uncaught ReferenceError: greeting is not defined
console.log(greeting);
```

### 作用域链

作用域链是指js引擎从变量或函数中查找变量或函数时，会从当前作用域开始，然后逐级向上查找，直到找到为止。

---

## 26. e.target和e.currentTarget区别？

区别是，event.currentTarget( ) 会返回当前触发事件的元素；而event.target( ) 会返回触发事件触发的源头元素。

用法：可以用来监听触发事件的元素是否事件发生的源头元素。这个源头元素指的是，当我点击子元素，虽然父元素的点击事件也会被触发（冒泡机制），但子元素才是事件的源头元素。

## 27. 改变this指向的方法？

1. bind()
   bind方法会创建一个新的函数，并将其内部的this绑定到指定对象

   ```javascript
   function sayhello() {
      console.log(this.name);
   }
   const person = {name:"john"};
   const boundFunction = sayhello.bind(person);
   boundFunction();//John
   ```
2. 箭头函数
   箭头函数没有this，他会继承外部作用域的this。因此在箭头函数中使用this时候，他会指向定义时所在的上下文。

   ```javascript
   const person = {
      name:"john",
      sayhello:function() {
         setTimeout(() => {console.log(this.name);},1000);
      }
   }
   person.sayhello();//John
   ```
3. call()或apply()

   call()和apply()方法可以立即调用函数，并显式指定函数内部的this值，他们之间的区别在于参数的传递方式。

   ```javascript
   function sayhello() {
      console.log(this.name);
   }
   const person = {name:"john"};
   sayhello.call(person);//John
   sayhello.apply(person);//John
   ```

---

## 28. 为什么要区分宏任务和微任务？他们的执行优先级是什么？

宏任务和微任务是为了解决 JavaScript 引擎中不同任务之间的执行优先级问题。
宏任务通常包含以下几种：

* setTimeout()和setInterval()
* DOM 事件处理程序
* AJAX 请求
* script标签的加载和执行

对于宏任务，JS引擎会将其添加到任务队列中，在当前任务执行完毕后按顺序依次执行。
微任务通常包含以下几种：

* Promise 的then和catch方法回调函数
* async/await
* MutationObserver 监听器

对于微任务通常比宏任务优先级高，JS引擎会优先执行微任务，然后再执行宏任务。微任务的执行在当前宏任务执行后立即进行。

## 29. const声明了数组，还能push元素吗？

可以。在 JavaScript 中，const 声明的变量不能被重新赋值，但它并不保证变量的值是不可变的。对于数组和对象这类引用类型，const 只是保证了变量指向的内存地址不能改变，但该地址所指向的数据内容是可以修改的。

## 30. 如何获取到一个实例对象的原型对象？

从构造函数获得原型对象：`构造函数.prototype`
从对象实例获得父级原型对象：`对象实例.__proto__`

## 31. 如何实现浏览器内多个标签页的通信？

* BroadcastChannel，专为同源页面间通信设计的轻量级 API

  ```javascript
  // 创建一个频道（所有标签页使用相同名称）
  const channel = new BroadcastChannel('my-channel');

  // 发送消息
  channel.postMessage('Hello from tab A!');

  // 接收消息
  channel.onmessage = function(event) {
  console.log('收到消息:', event.data);
  };

  // 可选：监听错误
  channel.onerror = function(event) {
  console.error('广播错误:', event);
  };
  ```
* localStorage 事件,当 localStorage 被修改时，同一源的其他标签页会触发 storage 事件（但不会通知当前修改的标签页）。

  ```javascript
  // 在所有监听的标签页中：
  window.addEventListener('storage', function(event) {
  if (event.key === 'tabMessage') {
     console.log('来自其他标签页的消息:', event.newValue);
  }
  });

  // 发送消息（在另一个标签页中执行）
  localStorage.setItem('tabMessage', 'Hello from tab!');
  // 可选：清除消息
  localStorage.removeItem('tabMessage');
  ```
* SharedWorke,创建一个共享的 Worker 线程，多个页面可以连接到它进行通信。
* 服务器中转（WebSocket / Server-Sent Events）
  如果标签页都连接了同一个后端服务，可以通过服务器中转消息。

## 32. document.write()和innerHTML的区别？

### document.write()

* 定义：是 document 对象的一个方法，用于向HTML 输出流中写入文本或 HTML。
* 如果在页面加载过程中调用：内容会插入到当前位置，相对安全。
* 如果在页面加载完成后调用（如点击事件、setTimeout）：会自动调用 document.open()，清空当前文档，重新写入内容，导致整个页面被覆盖！

### innerHTML

* 定义：是任意 DOM 元素的一个属性，表示该元素内部的 HTML 内容（作为字符串）
* 他将呢日哦那个写入某个DOM节点，不会导致页面重绘
* 在很多情况下都优于document.write(),其原因在于其允许更精确的控制要刷新页面的那一部分。

## 33. for...in和 for...of 有什么区别？

for...of 是ES6新增的，允许便利一个含有iterator接口的数据结构并返回各项的值，和for in的区别如下：

* for...of 遍历获取的是对象的键值，for...in获取的是对象的键名；
* for...in 会遍历对象的整个原型链，不推荐使用，而for...of只遍历当前对象不会遍历原型链。
* 对于数组的遍历，for...in会返回数组中所有可枚举的属性，而for...of只返回数组下标对应的属性值；
  总结：for...in循环主要为了遍历对象而生，不适用于遍历数组；for...of可以用来遍历数组、类数组对象，字符串、set、map一级Generator对象。

## 34. Object.defineProperty 和 ES6 的 Proxy 有什么区别？

### Object.defineProperty

Object.defineProperty 是一个用于定义 或 修改 JavaScript对象属性的方法。它可以修改对象的属性描述符，包括属性值、可枚举性、可配置性和可写性。
语法：`Object.defineProperty(obj, prop, descriptor)`，其中，obj 是要定义或修改属性的对象，prop 是要定义或修改的属性名称，descriptor 是一个包含属性描述符的对象。 属性描述符对象 descriptor 中包含以下属性：

* value：属性值，可以是任意类型的值，包括基本数据类型、对象、函数等。如果不指定该属性，则默认为 undefined。
* writable：属性是否可写，如果为 true，则属性的值可以被修改；如果为 false，则属性的值不能被修改。默认为 false。
* enumerable：属性是否可枚举，如果为 true，则属性可以被 for…in 循环遍历到；如果为 false，则属性不能被遍历到。默认为 false。
* configurable：是否可配置属性描述符，如果为 true，则属性描述符可以被修改；如果为 false，则属性描述符不能被修改。如果该属性为 false，则 writable 和 enumerable 的值也不能被修改。默认为 false。

  ```javascript
  var obj = {};

  Object.defineProperty(obj, 'name', {
  value: 'Tom',
  writable: false,
  enumerable: true,
  configurable: false
  });

  console.log(obj.name); // 输出 Tom
  obj.name = 'Jerry'; // 不会修改属性值
  console.log(obj.name); // 输出 Tom
  ```

### Proxy

Proxy 是 ES6 中新增的一个对象，它可以用来代理（拦截） JavaScript 中的对象操作，例如访问对象属性、调用函数、构造函数等。
语法：`new Proxy(target, handler)`，其中，target 是要代理的目标对象，handler 是一个对象，它包含了要代理的操作的处理函数（也称为拦截器）。
handler 对象中可以定义以下拦截器：

* get：拦截对象属性的读取操作，例如 obj.prop。
* set：拦截对象属性的赋值操作，例如 obj.prop = value。
* apply：拦截函数的调用操作，例如 fn(args)。
* construct：拦截类的构造函数的调用操作，例如 new MyClass(args)。

### 区别

二者之间的区别
  Object.defineProperty 和 ES6 的 Proxy 都是用于处理对象属性的工具，但它们的实现方式和用途略有不同。具体来说：

* Object.defineProperty 主要用于修改或扩展对象的属性描述符，它不能拦截对象的其他操作。
* Proxy 可以拦截对象的各种操作，包括读取、赋值、删除、枚举等，它可以用于实现各种高级功能。
* Object.defineProperty 是 ES5 中引入的，而 Proxy 是 ES6 中新增的。
* Object.defineProperty 支持的操作比 Proxy 少，只能修改或扩展属性描述符，而 Proxy 支持的操作更加广泛。
* 在Vue2.x中，双向绑定是基于Object.defineProperty 实现的。而在Vue3.x中用的是proxy

## 35. ES6 的Reflect对象和 Proxy对象有什么区别？

Proxy 与 Reflect 是 ES6 为了操作对象引入的 API 。

Proxy 可以对目标对象的读取、函数调用等操作进行拦截，然后进行操作处理。它不直接操作对象，而是像代理模式，通过对象的代理对象进行操作，在进行这些操作时，可以添加一些需要的额外操作。

Reflect 可以用于获取目标对象的行为，它与 Object 类似，但是更易读，为操作对象提供了一种更优雅的方式。它的方法与 Proxy 是对应的。

## 36. async/await 和 Promise 有什么关系？

async/await 是 ES7 引入的异步处理方案，它基于 Promise 实现。他们就是generator + promise 的语法糖,用同步的语句编写异步的代码，消灭异步回调。

* async 字面意思异步 async用于声明一个异步函数 `async function fn(){...}`
  在没有 await 的情况下执行 async 函数，它会立即执行，返回一个 Promise 对象，并且，绝不会阻塞后面的语句。这和普通返回 Promise 对象的函数并无二致。
* await 用于等待一个异步函数执行完成，并且语法规定，await只能在async函数里面被使用，如果需要用await等待一个async异步函数完成 ，需要在外面在包裹一层async。

async/await相辅相成，

* async函数返回的是promise实例对象
* await后面可以跟promise函数
* await相当于promise.then方法，并且只是成功的方法resolve,只是await时直接得到一个值，then需要传值进行回调
* async await与Promise一样，是非阻塞的。
* async await是基于Promise实现的，可以说是改良版的Promise，它不能用于普通的回调函数。

## 37. CSR和SSR 分别是什么？

CSR 和 SSR 是前端开发中两种主要的页面渲染方式，它们决定了网页内容是在客户端（用户浏览器）还是在服务器端生成并发送给用户的。

### CSR（Client-Side Rendering，客户端渲染）

**核心思想**：
服务器只返回一个空的 HTML 骨架（通常只有一个 `<div id="app"></div>`）和一个 JavaScript 文件。
所有页面内容、数据获取、DOM 操作都由浏览器下载 JS 后，在客户端动态生成。
**工作流程**：

* 用户访问页面。
* 服务器返回一个极简的 HTML 文件（几乎为空）。
* 浏览器下载并执行 JavaScript 文件。
* JS 代码向后端 API 请求数据。
* JS 根据数据动态构建 DOM，最终显示完整页面。

### SSR (Server-Side Rendering) - 服务端渲染

**核心思想**：

* 服务器在接收到请求时，提前将页面内容渲染成完整的 HTML 字符串，然后直接发送给浏览器。
* 用户打开页面时，看到的是一个已经包含内容的完整页面。

**工作流程**：

* 用户访问页面。
* 服务器接收到请求，调用后端逻辑或前端框架（在服务端运行）生成完整的 HTML。
* 服务器将包含数据的完整 HTML 发送给浏览器。
* 浏览器直接渲染出可视页面（首屏快）。
* 同时，JavaScript 也会被下载并“激活”（称为 Hydration），使页面具备交互能力。