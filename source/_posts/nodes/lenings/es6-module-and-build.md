---
title: ES6 的模块化使用和开发环境的打包
date: 2020-10-15 16:42:12
tags:
  - 面试
  - 学习笔记
  - ES6
description: 
---

## 模块化基本语法

### 导出模块语法 `export`

`export`是一个将魔模块导出的命令，可导出函数、类和变量等数据。

```js
// 导出类
export class Zoom {
  ...
}

// 导出变量
export const x = 1

// 导出函数
export function fn1 () {
  alert('fn1')
}
```

个人将以上代码理解为多模块导出，就是在一个模块文件中，可以导出多个模块，供其他模块调用。上面的代码导出方式也可写为：

<!--more-->

```js

// 导出类
class Zoom {
  ...
}

// 导出变量
const x = 1

// 导出函数
function fn1 () {
  alert('fn1')
}

export { Zoom, x, fn1 }

```

可通过导出一个对象集合的方式，导出多个模块，虽然上面两种写法不同，但本质上是一样的。模块导出时，默认导出的模块名与为变量名，也可在导出时使用 `as` 自定义模块名，`as` 语法的左边表示要导出的变量，右边表示导出的重命名。

```js
function x1 () { ... }
function x2 () { ... }

export {
  x1 as y1,
  x2 as y2
}
```

上面代码中，导出变量`x1`和`x2`时，分别将两个变量重命名为`y1`和`y2`。

> 要注意的是`export`命令规定对外的接口，必须与模块内的变量建立一一对应关系。

### 导出默认模块 `export default`

一个模块文件中可导出一个默认模块，通过`export default`语法将值默认的模块导出。

```js
export default function () {
  ...
}
```

可理解为将数据导出时已重命名成`default`。要注意的是多模块导出与默认模块导出的引入方式不一样。

## 导入模块语法 `import`

使用`import`命令可引入已导出的模块代码，引入后即可直接使用。

```js
// export.js
export const x1 = 1
export const x2 = 2

// import.js
import { x1, x2 } from 'export.js'
console.log(x1, x2) // 1, 2
```

在引入时，可使用`as`命令，将引入的模块重命名，重命名后的变量只会影响当前引入的模块文件，不会对导出模块文件及其他引入文件有影响。

```js
// 接上面代码

import { x1 as y1, x2 as y2 } from 'export.js'
console.log(y1, y2) // 1, 2
```

引入默认导出的模块，不需要加大括号，直接输入自定义的变量名可将默认的模块重命名导入。

```js
// export.js
export default function () {
  alert('default module')
}

// import.js
import fn from 'export.js'
fn() // 'default module'
```

多模块引入和默认模块引入主要的区别是，默认模块不需要加大括号，且一个导出文件中只有一个默认导出模块。而多模块导出顾名思义一个模块文件可导出多个模块，引入时，需要将模块名写在大括号中`{}`。默认模块引入可与多模块引入同时进行。

```js
// export.js
export const x1 = 1
export function () {
  alert('default module')
}

// import.js
import fn, { x1 as y1 } from 'export.js'
console.log(y1) // 1
fn() // 'default module'
```

## 模块化标准

模块化的发展

1. 没有模块化，蛮荒时代，有什么用什么。
2. AMD成为标准（`require.js`、`CMD` 等），，通过语法定义（`default`、`require` 等函数），完成模块化的编写。
3. 前端打包工具，`nodejs`模块化可以被使用（[CommonJS](https://javascript.ruanyifeng.com/nodejs/module.html)标准）。
4. ES6 出现，想统一现在所有的模块化标准。
5. 现状：nodejs（服务端）积极支持，浏览器尚未统一（需要使用babel编译成浏览器可识别的代码，例如：ES5）。
