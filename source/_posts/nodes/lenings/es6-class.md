---
title: ES6 Class 概念
date: 2020-04-09 22:32:42
tags:
  - 学习笔记
  - ES6

descriptions: 
---

## Class 和 普通构造函数有何区别

### JS构造函数

通过`function`命令生成基本的构造函数，然后设置构造函数的`prototype`属性，在原型链中写如方法，供实例调用。

```js
// 声明基本构造函数
function MathHandle(x, y) {
  this.x = x;
  this.y = y
}

// 在构造函数原型中添加 add 方法
MathHandle.prototype.add = function () {
  return this.x + this.y
}

// 声明实例
var m = new MathHandle(1, 2);
console.log(m.add()) // 3
```

### Class 基本语法

构造函数的代码通过`Class`语法写为：

<!--more-->

```js
Class MathHandle {
  constructor (x, y) {
    this.x = x;
    this.y = y
  }
  add () {
    return this.x + this.y
  }
}
const m = new MathHandle(1, 2)
console.log(m.add()) //3
```

### 语法糖

定义：语法糖与原始写法本质是一样的（效果或结果），但语法糖会比原始的写法会更加简单，易懂，清晰。可以理解为语法糖的代码会被解析成原始代码。

JS中没有真真意义上的`Class`，只是为了让类的写法与`java` `C#`等语言的类看起来一样，本质上还是通过 `prototype` 原型链的形式运行的。

```js
Class MathHandle {
  // ...
}

var m = new MathHandle(1, 2)

typeof MathHandle // "function"
MathHandle === MathHandle.prototype.constructor // true
m.__proto__ === MathHandle.prototype // true
```

上面代码可以看出，通过`Class`语法定义的类，其实就是一个构造函数，与通过`function`定义的构造函数是一样的。

### 继承

继承就是从抽象到具象的一个关系（过程），由高级到低级。例子：一个`狗`的类继承了`动物`的类，`动物` 会比 `狗` 更抽象，所以 `狗` 会继承 `动物`类。通过实例化`狗`这个类，产生比`狗`这个类更具体的`哈士奇`，`哈士奇`继承`狗`的所有特点，由于`狗`继承了`动物`，所以哈士奇还将有`动物`的特点。

通过`function`实现类的继承效果：

```js
// 动物类
function animal () {
  this.eat = function () {
    console.log('Animal eat')
  }
}

// 狗类
function Dog () {
  this.berk = function () {
    console.log('Dog bark')
  }
}

// 绑定原型，实现继承
Dog.prototype = new Animal()

// 声明实例
var hashiqi = new Dog()
hashiqi.bark() // "Animal eat"
hashiqi.eat() // "Dog bark"
```

同样效果的代码使用`Class`语法为：

```js
// 动物类
Class Animal {
  constructor (name) {
    this.name = name
  }

  eat () {
    console.log(this.eat + 'eat')
  }
}

// 狗类
Class Dog extends Animal {
  constructor (name) {
    // 调用父类的构造方法
    super(name)
    bark () {
      console.log(this.name + 'bark')
    }
  }
}

// 声明哈士奇
var hashiqi = new Dog('哈士奇')
console.log(hashiqi.eat) // "哈士奇eat"
console.log(hashiqi.bark) // "哈士奇bark"
```
