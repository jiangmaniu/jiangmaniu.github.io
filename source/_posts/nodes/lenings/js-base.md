---
title: javascript 基础
date: 2019-12-10 20:42:12
tags:
  - JS
  - 面试
  - 学习笔记
description: js的面试知识记录
---

## 基础篇

### 1.1 变量类型

变量类型分为值类型和引用类型

#### 1.1.1 值类型

值类型：`number`（数值）、`string`（字符串）、`boolean`（布尔）、`null`（空指针）、`undefined`（未定义）

定义：值类型会在内存中开辟两个内存空间用来存储值

特点：内容赋值为值赋值；独享内存；

<!--more-->

``` javascript
  var valueA = 100
  var valueB = valueA
  console.log(`初始值：valueA -> ${ valueA }, valueB -> ${ valueB }`)
  valueA = 200
  console.log(`valueA改变: valueA -> ${ valueA }, valueB -> ${ valueB }`)

  // 初始值：valueA -> 100, valueB -> 100
  // valueA改变: valueA -> 200, valueB -> 100
```

#### 1.1.2 引用类型

引用类型：`object`（对象）、`function`（函数）、`array`（数组）

定义：引用类型会将对象存储在内存中，赋值时会将对象的内存地址以一个指针的方式赋值给变量

特点：无限制扩展属性（此特点会导致赋值给其他变量时，会多占用很大的内存空间）；共享内存（为了避免第一个特点造成的内存浪费）

``` javascript
  var quoteA = { age: 20 }
  var quoteB = quoteA
  console.log(`初始值：quoteA ->`, quoteA , `quoteB -> `, quoteB)

  quoteA.age = 30
  console.log(`改变quoteA.age：quoteA -> `, quoteA, `quoteA：quoteB ->`, quoteB)

  var quoteC = quoteB
  console.log(`定义quoteC：quoteC -> `, quoteC, `quoteB -> `, quoteB, `quoteA -> `, quoteA)

  quoteC.age = 40
  console.log(`改变quoteC.age：quoteC -> `, quoteC, `quoteB -> `, quoteB, `quoteA -> `, quoteA)

  quoteC.sex = '男'
  console.log(`新增quoteC.sex：quoteC -> `, quoteC, `quoteB -> `, quoteB, `quoteA -> `, quoteA)
```

![图片](/assets/blog-imgs/interview-basis-1.png '输出内容')

### 1.2 typeof 操作符

定义：typeof操作符一共可以检查五种类型（分别是：`undefined`，`string`，`number`，`boolean`，`object`，`function`）

注意：typeof操作符只能检测**值类型**的详细类型（`null`除外），引用类型只能区分`object`（`object`, `array`, `null`）和`function`。`null`是特殊情况，`null`虽然是**值类型**，但typeof操作符会将其检测成`object`

``` javascript
  console.log(`typeof undefined：`, typeof undefined) // typeof undefined： undefined
  console.log(`typeof 'abc'：`, typeof 'abc')  // typeof 'abc'： string
  console.log(`typeof 123：`, typeof 123)  // typeof 123： number
  console.log(`typeof true：`, typeof true) // typeof true： boolean
  console.log(`typeof {}：`, typeof {}) // typeof {}： object
  console.log(`typeof []：`, typeof [])  // typeof []： object
  console.log(`typeof null：`, typeof null) // typeof null： object
  console.log(`typeof console.log：`, typeof console.log) // typeof console.log： function

```

### 1.3 变量计算 - 强制类型转换

只有`正负0` `''` `NaN` `null` `false` `undefined`强制类型转`boolean`为`false`，其他任何值都为`true`

``` javascript
  Boolean(0) // false
  Boolean(-0) // false
  Boolean('') // false
  Boolean(NaN) // false
  Boolean(false) // false
  Boolean(undefined) // false
```

#### 1.3.1 字符串拼接

定义：数值与数值相加得到数学计算后的值，数值与字符串相加会将数值转成字符串然后进行拼接

``` javascript
  var number = 10 + 10
  console.log(`数值相加：10 + 10 = ${ number }`)
  var splice = number + '10'
  console.log(`数值与字符串相加：20 + '10' = ${ splice }`)

  // 数值相加：10 + 10 = 20
  // 数值与字符串相加：20 + '10' = 2010
```

#### 1.3.2 `==` 运算符

``` javascript
  console.log(`数值类转string：100 == '100' res ${ 100 == '100' }`)
  console.log(`数值0与空字符串转boolean：0 == '' res ${ 0 == '' }`)
  console.log(`null与undefined转boolean：null == undefined res ${ null == undefined} `)

  // 数值类转string：100 == '100' res true
  // 数值0与空字符串转boolean：0 == '' res true
  // null与undefined转boolean：null == undefined res true
```

  问：何时使用 `===` 何时使用 `==`？

  答：当判断变量扽与（不等于）多个类型时（如：x === null || x === undefined || x === 0 ），可简写成 `==`运算符（x == null），否则一律使用 `===` 运算符，Jquery框架源码中推荐的写法

#### 1.3.3 if 语句

注意：`if`语句会自动将数值和字符串转为`boolean`，数值(`+0` `-0`)和字符串(`''`)会转为`false`，其余所有值都转为`true`

``` javascript
  var isNumber = 100
  if (isNumber) console.log(`isNumber：${ isNumber }`)
  isNumber = 0
  if (isNumber) console.log(`isNumber：${ isNumber }`)

  var isString = '100'
  if (isString) console.log(`isString：${ isString }`)
  isString = ''
  if (isString) console.log(`isString：${ isString }`)
  
  // isNumber：100
  // isString：'100'
```

#### 1.3.4 逻辑运算符

##### 1.3.4.1 在 `if` 语句中

- &&(与运算)：所有的值转`boolean`都为`true`时，返回`true`

- ||(或运算)：其中一个值转`boolean`都为`true`时，返回`true`

- !(非运算)：判断的值转`boolean`都为`true`时，返回相反值`false`，反之亦然

``` javascript
  if(10 && 100) console.log(`与运算符 10 && 100 -> true`)
  if(10 && 0) console.log(`与运算符 10 && 0 -> true`)

  if(0 || 10) console.log(`或运算符 0 || 10 -> true`)
  if(null || 0) console.log(`或运算符 null && 0 -> true`)

  if(!0) console.log(`非(取返)运算符 !0 -> true`)
  if(!10) console.log(`非(取返)运算符 !10 -> true`)

  // 与运算符 10 && 100 -> true
  // 或运算符 0 && 10 -> true
  // 非(取返)运算符 !0 -> true
```

##### 1.3.4.2 在表达式中

- &&(与运算)：返回 值转`boolean`都为`false`的值，若都为`true`则返回最后一个值

- ||(或运算)：返回 值转`boolean`都为`true`的值，若都为`false`则返回最后一个值

- !(非运算)：判断的值转`boolean`都为`true`时，返回相反值`false`，反之亦然

``` javascript
  console.log(`与运算符 10 && 0 -> ${ 10 && 0 }`) // 0
  console.log(`与运算符 100 && 10 -> ${ 100 && 10 }`) // 100

  console.log(`或运算符 0 || 10 -> ${ 0 || 10 }`) // 10
  console.log(`或运算符 null || 0 -> ${ null || 0 }`) // 0

  console.log(`非(取返)运算符 !10 -> ${!10}`) // false
  console.log(`非(取返)运算符 !0 -> ${!0}`) // true
```

### 1.4 内置函数

JS内置函数：

- Object（对象）
- Number（数值）
- String（字符串）
- Boolean（布尔）
- Function（函数）
- Array（数组）
- Date（日期）
- RegExp（正则）
- Error（报错）

### 1.5 理解JSON

定义：JSON是一种数据格式，也是一个JS对象（JSON在JS中就是一个数据对象）

JSON有两个API：

- JSON.stringify（将对象转成字符串）
- JSON.parse（将字符串转成对象）

### 1.6 原型链

#### 1.6.1 构造函数

特性：1.命名首字母需大写，2.可以new多个实例

``` javascript
  function Foo (name, age) {
    this.name = name
    this.age = age
    // return this 默认有这一行
  }

  var foo = new Foo('Maniu', 20)
  console.log(foo) // Foo {name: "Maniu", age: 20}
```

#### 1.6.2 实例的过程

1. `new` 构造函数时（不管有没有传参数），构造函数哩的this时指向一个空对象
2. `this.xxx`赋值
3. 默认 `return this`
4. 将`return`的this赋值给变量

#### 1.6.3 语法糖

`var a = {}` 实际是`var a = new Object()`的语法糖（简写），也就是说对象`a`的构造函数时`Object`

`var a = []` 实际是`var a = new Array()`的语法糖（简写），也就是说数组`a`的构造函数时`Array`

`function a () { ... }` 实际是`var a = new Function()`的语法糖（简写），也就是说方法`a`的构造函数时`Function`

本人**推荐使用语法糖的写法创建变量**

### 1.7 原型规则

#### 1.7.1 自由扩展属性

所有引用类型（数组，对象，函数）都具有对象特性，即可自由扩展属性（null除外）

``` javascript
  var obj = {}
  obj.a = 100
  console.log(`obj:`, obj ) // obj: {a: 100}

  var arr = []
  arr.a = 100
  console.log(`arr:`, arr) // arr: [a: 100]

  function fn () {}
  fn.a = 100
  console.log(`fn.a:`, fn.a) // 100
```

#### 1.7.2 `__proto__` 隐式原型

所有引用类型（数组，对象，函数）都具有一个`__proto__`（隐式原型）属性，属性值都是一个普通对象。

继续上面代码：

``` javascript
  console.log(`obj.__proto__：`, obj.__proto__)
  console.log(`arr.__proto__：`, arr.__proto__)
  console.log(`fn.__proto__：`, fn.__proto__)
```

![图片](/assets/blog-imgs/interview-basis-2.png '输出内容')

#### 1.7.3 `prototype`显示原型

所有函数都有一个prototype（显示原型）属性，属性值也是一个普通对象。

继续上面代码:

``` javascript
  console.log(`fn.prototype；`, fn.prototype)
```

![图片](/assets/blog-imgs/interview-basis-3.png '输出内容')

#### 1.7.4 查找原型链

当视图访问一个对象的某个属性时，如果这个对象本身没有这个属性，那么会去它的`__proto__`（即它的构造函数的prototype）中寻找，换句话说它会从原型链向上查找

``` javascript
  function Foo (name) {
    this.name = name
  }
  Foo.prototype.fuName = function () {
    // this指向调用的实例
    console.log("fuName", this.name)
  }

  var f = new Foo('jiangmaniu')
  f.ziName = function () {
    console.log('ziName：', this.name)
  }
  f.fuName() // fuName：jiangmaniu
  f.ziName() // ziName：jiangmaniu
```

原型链会一直向上查找到构造函数的原型，如果还是未找到则返回`undefined`

继续上面代码：

``` javascript
  console.log(f.toString) // toString() { [native code] }
  console.log(f.notAttr) // undefined
```

问：请写一个原型链继承的例子

答：通用的写法：

``` javascript
  function Animal () {
    this.eating = function () {
      console.log(`${ this.name } 正在吃东西`)
    }
  }

  function Dog (name) {
    this.name = name
    this.calling = function () {
      console.log(`${ this.name } 正在叫`)
    }
  }

  Dog.prototype = new Animal()

  var hashiqi = new Dog('hashiqi')
  hashiqi.eating() // hashiqi 正在吃东西
  hashiqi.calling() // hashiqi 正在叫
```

不过不建议在面试中这样写，因为不符合实际需求，所以可写一个距离实际需求比较接近的继承

``` javascript
  var div = document.createElement('div')
  div.id = 'box'
  div.innerHTML = '<h1>这是初始内容</h1>'
  document.body.appendChild(div)

  function Dom (id) {
    this.elem = document.getElementById(id)
  }
  Dom.prototype.html = function (val) {
    var $ = this.elem
    if(val) {
      $.innerHTML = val
      return this
    } else {
       // get
      return $.innerHTML
    }
  }
  Dom.prototype.text = function (val) {
    var $ = this.elem
    if(val) {
      // set
      $.innerText = val
      return this
    } else {
      // get
      return $.innerText
    }
  }
  Dom.prototype.on = function (type, fn) {
    var $ = this.elem
    $.addEventListener(type, fn)
    return this
  }
  
  var div = new Dom('box')
  div.html('<p>html methods changed</p>').on('click', function () {
    console.log(div.text())
  })
```

#### 1.7.5 `for in` 循环对象

使用`for in`循环对象属性会循环出原型链上的属性

继续上面代码：

``` javascript
  var allList = []
  for(item in f) {
    allList.push(item)
  }
  console.log(allList) // ["name", "ziName", "fuName"]
```

可使用`hasOwnProperty`排除原型链上的属性

- obj.hasOwnProperty(attr) 只对象自身是否有传入的属性，返回布尔值

继续上面代码：

``` javascript
  var allList = [], thisList = []
  for(item in f) {
    allList.push(item)

    if (f.hasOwnProperty(item)) {
      // 高技浏览器已经在 for in 中屏蔽了来自原型链的属性
      // 但是这里建议大家加上这个判断，博鳌镇程序的健壮性
      thisList.push(item)
    }
  }
  console.log(allList) // ["name", "ziName", "fuName"]
  console.log(thisList) // ["name", "ziName"]
```

#### 1.7.6 `instanceof`

`instanceof`可以判断右边是否是左边变量的构造函数，会从原型链一层一层查找

继续上面代码：

``` javascript
  console.log( f instanceof Foo) // true
  console.log( f instanceof Object) // true
  console.log( f instanceof Function) // false
  console.log( Foo instanceof Function) // true
```

问：如何准确判断一个变量是数组类型？

答：`typeof`无法只能检查出`Object`，因该使用`instanceof`检查。如下代码:

``` javascript
  var arr = []
  console.log(typeof arr) // Object
  console.log(arr instanceof Array) // true
```

### 1.8 捕获与冒泡

`addEventListener(type, fn, false)`可以给元素添加事件

- 第一个参数为事件类型
- 第二个参数为事件回调
- 第三个参数指定事件**捕获**时运（`true`）行还是**冒泡**时运行（`false`），默认值：`false`

## 2 函数作用域

### 2.1 匿名函数

`function () {}` 就是一个匿名函数，匿名函数不会自动调用，也没有存储变量。需要以赋值给变量的形式调用函数，或者以`function xx () {}`（函数声明）的方式调用

### 2.2 前置执行

特性：会把变量的声明和函数的声明不管在哪里声明会前置执行，也可以说在函数第一行代码未执行前，变量声明和函数声明会前置执行（最开始执行）

``` javascript
  var number = 1;
  function fn () {
    // ...
  }

  // 相当于

  // 最开始执行
  var number, fn
  fn = function () {
    // ...
  }

  // 执行到当前行执行
  number = 1
```

函数声明会被自动前置声明变量（包括匿名函数的赋值），所以如果在函数声明之前调用函数还是可以正常执行，不会报错

``` javascript
  fn() // 函数声明前调用
  function fn () {
    // ...
  }

  // 相当于

  // 最开始执行
  var fn
  fn = function () {
    // ...
  }

  // 执行到当前行执行
  fn()
```

但普通变量只是会自动前置声明变量，并不会在前置声明的同时赋值，所以默认值为`undefined`，等到程序执行到赋值行的时候才会给变量赋值

``` javascript
  console.log(a) // undefined
  var a = 1
  console.log(a) // 1

  // 相当于

  // 最开始执行
  var a

  // 执行到当前行执行
  console.log(a) // undefined
  a = 1
  console.log(a) // 1
```

如果在变量声明之前给变量赋值，则变量声明会自动前置声明（未赋值，默认值：`undefined`），执行到赋值行时在给变量赋值

``` javascript
  name = 'jiangmaniu'
  var name // 会前置声明

  // 相当于

  // 最开始执行
  var name

  // 执行到当前行执行
  name = 'jiangmaniu'
```

### 2.3 函数声明和函数表达式

函数声明与函数表达式最大的不同就是函数声明会前置声明变量并且赋值匿名函数，函数表达式只是前置声明变量并未赋值匿名函数

- function fn1 () { ... } 函数声明
- var fn1 = function () { ... } 函数表达式

这也导致无法在函数表达式之前是调用函数

``` javascript
  fn() // error: fn is not a function
  var fn = function () {
    // ...
  }

  // 相当于

  // 前置执行
  var fn

  // 执行到当前行执行
  fn()
  fn = function () {
    // ...
  }
```

### 2.4 函数的特殊值

每个函数内都有两个特殊值（`this`和`arguments`），并且这两个在函数执行前就已经有值

- `this` 只当前（函数）作用于
- `arguments` 函数传入的参数集合体（数组）

### 2.5 自执行函数

匿名函数除了以赋值给变量和函数声明的形式调用，还可以用`(...)()`包围一个匿名函数就成了自执行函数，自执行函数会自动执行`(...)`内的匿名函数，不需要赋值变量的形式调用

``` javascript
  (function () {
    console.log('I am a automated method')
  })()
```

### 2.6 作用域

函数的父级作用域拿不到子函数作用域的内容，反之子函数作用域可以拿到父函数作用域的内容

``` javascript
  (function () {
    var x = 1
  })()

  console.log(x) // error: x is not defined

  var y = 1

  function fn () {
    console.log(y)
  }
  fn()
```

#### 2.6.1 var、let和const

##### `var` 函数作用域

注意：**在ES6之前，由于只有`var`声明语句，所以JS本身没有块级作用域，只有函数作用域和全局作用域**

特性：JS会自动把`var`声明的变量前置声明，并未赋值（默认：`undefined`），等程序执行到赋值的行在给变量赋值。

这个特性，让`var`可以重复声明相同的变量，后声明的变量会覆盖前面声明的变量

``` javascript
  var a = 1
  var a = 2
  console.log(a) // 2

  // 等同于

  // 最开始执行
  var a

  // 执行到当前行执行
  a = 1
  a = 2
  console.log(a) // 2
```

并且这个特性让`{ }`（局部作用域）内用`var`声明的变量，`{ }`外面可以访问

``` javascript
  {
    var a = 1
  }
  console.log(a) // 1

  // 等同于

  // 最开始执行
  var a

  // 执行到当前行执行
  {
    a = 1
  }
  console.log(a)
```

##### `let` 块级作用域

注意：**`let`是ES6的新语法，它让JS有了`{ }`（块级作用域）**

`let`（块级作用域）：JS不会自动把`let`声明的变量前置声明，所以`let`不能重复声明相同的变量

``` javascript
  let a = 1
  let a = 2 // error: Identifier 'a' has already been declared
```

并且这个特性让`{ }`（局部作用域）内用`let`声明的变量，`{ }`外面**不**可以访问

``` javascript
  {
    let a = 1
  }
  console.log(a) // error: a is not defined
```

##### `const` 静态块级作用域

注意: **`const` 与 `let`的特性一致（不再演示）**

`const`声明的变量不可完全更改（可更改属性）

``` javascript
  const a = { name: 'jiangmaniu' }
  a.name = 'heihei' // heihei
  a = { name: 'nihao' } // error: Assignment to constant variable.

  const b = 10
  b = 20 // error: Assignment to constant variable.
```

##### 未使用定义关键字给变量赋值

若在函数作用域**给未定义的变量赋值时**，JS会从作用域链向上查找变量，若在全局作用域都未找到该变量名，则JS会自动在全局作用域**声明这个未定义的变量名并赋值**

例子：

``` javascript
  // 作用与A
  var a = 1
  function fn () {
    if (a === 1) {
      // 作用域B
      var b = 2
      if (b === 2) {
        // 作用域C
        (function () {
          c = 3 // 给未定义的变量赋值
        })()
        console.log(`作用域C：${c}`)
      }
      console.log(`作用域B：${c}`)
    }
  }

  fn()
  console.log(`作用域A：${c}`)

  // 作用域C：3
  // 作用域B：3
  // 作用域A：3
```

#### 2.6.2 `this` 作用域

定义: `this`要在实行时才能确定指向的值，定义时是无法确认的

##### `this`指向变换的场景

作为构造函数执行，`this`指向构造函数

``` javascript
  function Foo (name) {
    // this 指向Foo构造函数
    this.name = name
  }

  var fn = new Foo('jiangmaniu')
```

作为对象属性执行，`this` 指向对象

``` javascript
  var obj = {
    name: 'jiangmaniu',
    printName: function () {
      console.log(this.name)
    }
  }

  obj.printName() // this 指向 obj
```

作为普通函数执行，`this` 指向 `windows` 或 `undefined`

``` javascript
  function fn () {
    console.log(this) // windows of undefined
  }

  fn()
```

使用`call`、 `apply`和`bind`方法执行，`this`指向传进来的参数对象

`call`：第一个参数为`this`指向的内容，后面的参数全是传入函数的参数

``` javascript
  function callFn (name, age) {
    console.log(`callFn：${ name }, ${ age }`) // jiangmaniu, 20
    console.log(`callFn.this：`, this) // { x: 100 }
  }

  callFn.call({ x: 100 }, 'jiangmaniu', 20)
```

`apply`：第一个参数为`this`指向的内容，后面所有传入函数的参数都写在第二个参数的`[ ]`中

``` javascript
  function applyFn (name, age) {
    console.log(`applyFn：${ name }, ${ age }`) // jiangmaniu, 20
    console.log(`applyFn.this：`, this) // { x: 100 }
  }

   // 传入的函数的参数写成数组
  applyFn.apply({ x: 100 }, ['jiangmaniu', 20])
```

`bind`：需要用函数表达式声明函数，且在匿名函数后添加`.bind`方法，里面传入`this`要指向的对象，之后直接调用函数并写上要传入函数的参数即可

``` javascript
  var bindFn = function (name, age) {
    console.log(`bindFn：${ name }, ${ age }`) // jiangmaniu, 20
    console.log(`bindFn.this：`, this) // { x: 100 }
  }.bind({ x: 100 })

   // 直接传入参数即可
  bindFn('jiangmaniu', 20)
```

#### 2.6.3 函数作用域链

函数作用域内定义的相同变量会覆盖全局作用域的相同变量

``` javascript
  var a = 100

  function fn () {
    var a = 200
    console.log(`fn：${a}`) // 200
  }

  fn()
  console.log(`global：${a}`) // 100
```

访问当前函数作用域没有定义的变量，则这个变量称为`自由变量`，`自由变量`会向**定义函数时**（不是调用时）的父级作用域一层一层的查找变量，直到全局作用域。如果全局作用域也查找到，则返回`undefined`

注意: **这里说的父级作用域指的是函数定义时的父级作用域，而不是函数执行时的父级作用域**

``` javascript
  // 全局作用域
  var a = 10

  function  fn1 () {
    // 函数作用域
    var b = 200

    // 为自由变量，会向，会向父级作用域查找变量
    console.log(a)

    function fn2 () {
      // 函数作用域

      // 自由变量
      console.log(a)
      console.log(b)
    }

    fn2()
  }
  
  fn1 ()

  // 10
  // 10
  // 200
```

#### 2.6.4 闭包

特性：1. 返回要给匿名函数；2. 封闭变量，收敛权限（只能在函数内部访问访问，防止外部篡改）

``` javascript
  function Foo () {
    var a = 100

    return function () {
      console.log(a) // 自由变量，会向父级作用域查找变量
    }
  }
  
  var fn = new Foo ()
  var a = 200
  fn()

  // 100
```

问：闭包的实际的场景

答：下面代码是避免重复添加相同的数据例子

``` javascript
  function FirstAddList () {
    // 以下划线（_）开头的变量被称为私有变量
    var _list = []
    var that = this

    // 返回闭包
    return function (num) {
      // 为防止篡改，只有在闭包内才能访问私有变量

      if (_list.indexOf(num) >= 0) {
        // _list数组已有要添加的值
        console.log('Not the first time')
      } else {
        _list.push(num)
        console.log('the first time')
      }
    }
  }

  var fn = new FirstAddList()
  fn(10) // the first time
  fn(10) // not the first time
  fn(100) // the first time
  console.log(_list) // error；_list is not defined
```

## 3 异步和单线程

### 3.1 异步

定义：为了避免程序在运行中一些类似于`alert`等需要等待的操作（操作完成才能继续执行）会阻塞整个程序的运行，需要将这些要等待的操作以异步（单独执行，避免阻塞整个程序的运行）的形式处理

#### 使用异步的场景

定时任务：`setTimeout`，`setInterval`

代码示例：

``` javascript
  console.log(1)
  setTimeout(function () {
    console.log(2)
  }, 1000)
  console.log(3)

  // 1
  // 3
  // 2（一秒后显示）
```

网络请求：`ajax`请求，动态`<img>`加载

- ajax请求代码示例：

``` javascript
  console.log('start')
  $.get('./data.json', function (data) {
    console.log(data)
  })
  console.log('end')

  // start
  // end
  // data（请求数据成功后显示）
```

- `<img>`加载示例

``` javascript
  console.log('start')
  var img = document.createElement('img')
  img.onload = function () {
    console.log('load end')
  }
  console.log('end')

  // start
  // end
  // load end（图片加载完成后显示）
```

事件绑定: `oo`，`addEventListener`

代码实例：

``` javascript
  console.log('start')
  document.getElementById('btn').addEventListener('click', function () {
    console.log('click')
  })
  console.log('end')

  // start
  // end
  // click（点击显示）
```

#### 单线程

>注意：JavaScript语言时单线程语言，即不会同时做两件事

JS会将异步操作与主程序抽离出来（主程序会继续执行），从而不影响主程序的运行。若未给异步代码设置一个延时执行时间（设置`0`毫秒执行与未设置时间效果一致），则异步操作会等主程序执行完成后，再来执行。若设置了延时执行时间，则异步操作会等到延时时间结束后，再来执行

代码示例：

``` javascript
  console.log(1)
  setTimeout(function () {
    console.log(2)
  }, 1000)
  console.log(3)
  setTimeout(function () {
    console.log(4)
  }, 0)
  console.log(5)
  setTimeout(function () {
    console.log(6)
  })
  console.log(7)
  
  // 1
  // 3
  // 5
  // 7
  // 4（主程序执行完成后显示）
  // 6（主程序执行完成后显示）
  // 2（一秒后显示）
```

### 4 API

#### 日期函数

`new Date()` 可以获取到当前的日期对象，可以通过对象的API获取去要的内容

- `getTime` 获取当前时间的时间戳（单位：毫秒），从`1970.1.1`年开始
- `getFullYear` 获取完整的年份（4位，1970-????）
- `getYear` 获取当前年份（2位数）
- `getMonth` 获取当前月份（0-11，0代表1月）
- `getDate` 获取当前日（1-31）
- `getDay` 获取当前星期X（0-6，0代表星期日）
- `getHours` 获取当前小时（0-23）
- `getMinutes` 获取当前分钟（0-59）
- `getSeconds`获取当前秒（0-59）
- `getMilliseconds`获取当前毫秒（0-999）
- `toLocaleDateString`获取当前日期（yyyy/mm/dd）
- `toLocalString` 获取当前日期时间

问：获取2017-06-08的日期格式

答：

``` javascript
  function formatDate (dt) {
    if(!dt) dt = new Date()
    let year = dt.getFullYear()
    let month = dt.getMonth() + 1
    let day = dt.getDate()

    if (month < 10) month = '0' + month
    if (day < 10) day = '0' + day

    return `${year}-${month}-${day}`
  }
  formatDate()
```

#### 数学对象

`Math` 是JS内置的对象，用于数学计算

对象属性（只显示部分常用属性。[详情点击](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Math#Properties)）：

- `PI`：返回圆周率（约等于3.14159）

对象方法（只显示部分常用方法。[详情点击](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Math#Methods)）：

- `abs(x)` 返回数的绝对值
- `ceil(x)` 对数进行上舍入
- `floor(x)` 对数进行下舍入
- `round(x)` 把数四舍五入为最接近的整数
- `max(x)` 返回 x 和 y 中的最高值
- `min(x)` 返回 x 和 y 中的最低值

问：获取随机数，要求小数点长度是设置的长度，并且转成字符串

答：

``` javascript
  function getRandom (length) {
    if (!length) length = 9
    var num = Math.random(9).toString().slice(0, length + 2)
    return num
  }
  getRandom()
```

#### 数组（Array）对象

对象属性（只显示部分常用属性。[详情点击](https://www.w3school.com.cn/jsref/jsref_obj_array.asp)）：

- `length`：设置或返回数组中元素的数目
- `prototype`：使您有能力向对象添加属性和方法

对象方法（只显示部分常用方法。[详情点击](https://www.w3school.com.cn/jsref/jsref_obj_array.asp)）：

- `concat()` 连接两个或更多的数组，并返回结果
- `join()` 把数组的所有元素放入一个字符串。元素通过指定的分隔符进行分隔
- `pop()` 删除并返回数组的最后一个元素
- `push()` 向数组的末尾添加一个或更多元素，并返回新的长度
- `reverse()` 颠倒数组中元素的顺序
- `shift()` 删除并返回数组的第一个元素
- `unshift()` 向数组的开头添加一个或更多元素，并返回新的长度
- `slice()` 删除元素，并向数组添加新元素
- `splice()` 删除元素，并向数组添加新元素
- `toString()` 把数组转换为字符串，并返回结果

高级API

`forEach`遍历所有元素

``` javascript
  var arr = [1, 2, 3]

  var arrForEach = arr.forEach(function (item, index) {
    // 第一个参数为元素值，第二个参数为元素下标
    console.log(`forEach：`, index, item)
  })

  // forEach： 0 1
  // forEach： 1 2
  // forEach： 2 3
```

问：写一个能便利对象和数组的通用forEach函数

答：

``` javascript
  function forEach(obj, callback) {
    if (obj instanceof Array) {
      obj.forEach(function (item, index) {
        callback(item, index)
      })
    } else if ( obj instanceof Object) {
      for (key in obj) {
        callback(obj[key], key)
      }
    } else {
      console.log('请传入数组或对象')
    }
  }

  var arr = [1, 2, 3, 4]

  forEach(arr, function (item, index) {
    console.log('arr：', index, item)
  })

  //arr： 0 1
  //arr： 1 2
  //arr： 2 3
  //arr： 3 4

  var obj = { name: 'jiangmaniu', age: 18, sex: '男'}

  forEach(obj, function (item, key) {
    console.log('obj：', key, item)
  })

  // obj： name jiangmaniu
  // obj： age 18
  // obj： sex 男

  forEach() // 请传入数组或对象
```

`every` 判断所有元素都满足条件返回`true`，否则返回`false`（值默认返回`false`）

``` javascript
 var arr = [1, 2, 3]
 var result = arr.every(function (item, index) {
   // 第一个参数为元素值，第二个参数为元素下标
   if (item < 4) {
     return true
   }
 })
 console.log(result) // true
```

`some` 判断所有至少一个满足条件返回`true`，否则返回`false`（返回值默认返回`false`）

``` javascript
 var arr = [1, 2, 3]
 var result = arr.every(function (item, index) {
   // 第一个参数为元素值，第二个参数为元素下标
   if (item < 2) {
     return true
   }
 })
 console.log(result) // true
```

`sort` 对数组的元素进行排序

``` javascript
  var arr = [1, 4, 3, 2, 5]
  var arrSort = arr.sort(function (a, b) {
    // 从小到大
    return a - b

    // 从大到小
    // return b - a
  })
  
  console.log(`从小到大：`, arrSort) // [1, 2, 3, 4, 5]
```

`map` 对元素重新组装，生成新数组

``` javascript
  var arr = [1, 2, 3, 4]
  var arrMap = arr.map(function (item, index) {
    // 组装每一项
    return item + 10
  })
  console.log(arrMap) // [11, 12, 13, 14]
```

`filter` 过滤不符合条件的元素，返回`true`表示保留该元素，否则返回`false`（默认返回：`false`）

``` javascript
  var arr = [1, 2, 3, 4]
  var arrFilter = arr.filter(function (item, index) {
    if (item % 2 == 0) return true
  })
  console.log(arrFilter) // [2, 4]
```

## 5 DOM 文档对象模型

`DOM`的本质就是`html`的文档结构代码。可以理解为浏览器请求域名地址后拿到页面数据，并将页面数据解析成浏览器能识别并且JS可以构建的一个模型

问：DOM是那种基本的数据结构

答：树

### DOM节点常用API

#### 获取元素

| 方法 | 说明 |
| :------ | :---- |
| getElementById() | 通过ID获取单个节点 |
| getElementByTagName() | 通过标签名获取多个节点（数组）|
| getElementByTagClassName() | 通过类名获取多个节点（数组）|

``` html
  <div id="box" class='red'>this is div</div>
```

``` javascript
  var byId = document.getElementById('box')
  var byTagName = document.getElementByTagName('div') // 数组
  var byTagClass = document.getElementByTagClass('red') // 数组
```

#### 元素属性

| 方法 | 说明 |
| :------ | :---- |
| nodeName | 返回一个字符串，其内容是节点的名字 |
| nodeType | 返回一个整数，这个数值代表给定节点的类型，[详情点击](https://developer.mozilla.org/zh-CN/docs/Web/API/Node/nodeType)|
| nodeValue | 返回给定节点的当前值|

``` html
  <div id="box" class='red'>this is div</div>
```

``` javascript
  var div = document.getElementById('box')
  console.log(div.nodeName) // DIV
  console.log(div.nodeType) // 1
  // 获取div的首个子节点
  console.log(div.childNodes[0].nodeValue) // this is div
```

#### DOM操作

| 方法 | 说明 |
| :------ | :---- |
|creatElement(element)|创建一个新的元素节点|
|creatTextNode()|创建一个包含给定文本的新文本节点|
|appendChild()|指定节点的最后一个节点列表后添加一个新的子节|
|insertBefore()|将一个给定节点插入到一个给定元素节点的给定子节点的前面|
|removeChild()|从一个给定元素中删除子节点|
|replaceChild()|把一个给定父元素里的一个子节点替换为另外一个节点 |

问：DOM操作的常用API有哪些？

答：获取节点元素，获取兄弟节点，获取父子节点，获取节点内容

### property和attribute

property 是更改`DOM`元素的标准JS对象属性

``` javascript
  <div id="div" class="red"></div>
```

``` javascript
  var div = document.getElementById('div')

  console.log(div.className)
  div.className = div.className + 'green'
  console.log(div.className)

  // red
  // red green
```

attribute 是更改`DOM`元素的标签属性

- `getAttribute` 获取DOM元素的标签属性
- `setAttribute` 更改DOM元素的标签属性

``` html
  <div id="div" class="red" data-name="div">this is div</div>
```

``` javascript
  var div = document.getElementById('div')

  console.log(div.getAttribute('data-name'))
  div.setAttribute('data-name', 'DIV')
  console.log(div.getAttribute('data-name'))
  
  console.log(div.getAttribute('class'))
  div.setAttribute('class', 'green')
  console.log(div.getAttribute('class'))

  // div
  // DIV
  // red
  // green
```

问：DOM节点的property与attribute有何区别

答：他们两个都是更改dom元素的属性，但不同的是property是更改Dom元素的标准js对象属性，而attribute是更改Dom元素的标签属性
