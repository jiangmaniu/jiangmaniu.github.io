---
title: zepto 和 jquery 的原型理解
date: 2020-03-17 22:32:42
tags:
  - zepto
  - jquery
  - 学习笔记

descriptions: 
---

## zepto 的原型的简单实现

在库外面使用`$()`语法获取一个或多个元素时，`zepto` 会将传入的`selector`字符串找到相应的元素，并将元素的一些信息记录在`this`中。然后`return`出`zepto`的实例，该实例为`zepto`中的`Z`构造方法实实例的。`Z`的构造方法的原型会指向`$.fn`对象，该对象内实现了`zepto`的造作方法等。

相关代码：

```html
<p>123</p>
<script>
var $p = $('p')
console.log($P.css()) // css
console.log($p.html()) // html
console.log($p.fn) // zepto prototype
</script>
```

```js
(function (window) {

  var zepto = {}

  function Z (dom, selector) {
     var i, len = dom ? dom.length : 0

    // 将元素信息保存在实例中
    for (i = 0; i < len; i++) {
      this[i] = dom[i]
    }
    this.length = len
    this.selector = selector || ''
    return this
  }

  zepto.Z = function (dom, selector) {
    return new Z(dom, selector)
  }

  zepto.init = function (dom, selector) {
    // 获取 selector 对应的 dom 节点,
    // 由于 node list 是一个伪数组，使用 slice 转一次
    const slice = Array.prototype.slice
    const dom = slice.call(document.querySelectorAll(selector))
    return zepto.z(dom, selector)
  }

  var $ = function (selector) {
    return zepto.init(dom, selector)
  }

  // 暴露到全局变量
  window.$ = $

  // 添加 Z 构造方法的原型
  Z.prototype = $.fn = {
    css: function (key, value) {
      return 'css'
    },
    html: function (value) {
      return 'html'
    }
  }
})(window)
```

## jquery 的原型的简单实现

由于`zepto`是借鉴`jquery`的，所以`jquery` 的原型实现效果与`zepto`一致，只是实现的方式有点不一样。

```js
(function (window) {
  var jQuery = function (selector) {
    return new jQuery.fn.init(selector)
  }

  // jquery 原型
  jQuery.fn = {
    css: function (key, value) {
      return 'css'
    },
    html: function (value) {
      return html
    }
  }

  var init = jQuery.fn.init = function (selector) {
    var slice = Array.prototype.slice
    var dom = slice.call(document.querySelectorAll(selector))
    var i, len = dom ? dom.length : 0

    for (i = 0; i < len; i++) {
      this[i] = dom[i]
    }
    this.length = len || 0
    this.selector = selector || ''
    return this
  }

  // 添加构造方法的原型
  init.prototype = jQuery.fn

  // 暴露变量
  window.$ = jQuery
})(window)
```

## 原型的扩展性

通过上面`jquery`和`zepto`的原型的简单实现代码，可发现代码中都有`$.fn`的属性，并且改属性为构造器的原型方法。就上面代码看，可以完全不需要`$.fm`这以属性，这是做什么用的呢？因为这样可以实现插件扩展性，由于`jquery`和`zepto`都是执行在自执行函数中，这样的好处是避免变量的全局污染问题，只暴露给全局`window`中的`$`变量，像`init`、`Z`等都是外面可能会出现的变量，如果不在自执行函数中声明，会与其他的同名变量冲突，为了避免，将构造函数的原型绑定在`$`变量的属性中。这样外面可直接使用`$.fn`来添加方法（插件），在`$.fn`中添加的方法在所有实例中都可访问。

此方法的好处：

1. 只有`$`会暴露在`window`全局变量
2. 将插件扩展统一到`$.fn.xx`这一接口，方便使用

```html
<!-- 普通标签节点 -->
<p>1</p>

<!-- 引入jquery -->
<script type="text/javascript" src="jquery.js"></script>

<!--添加 jquery 插件-->
<script type="text/javascript">
  // 添加 jquery 插件
  $.fn.getNodeName = function () {
    return this[0].nodeName
  }
</script>

<!-- 验证添加的插件 -->
<script>
  var dom = $('p')
  console.log(dom.getNodeName()) // p
</script>
```
