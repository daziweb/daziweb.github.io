---
title: JavaScript 中的 this、call 和 apply
tags:
  - this, call, apply
categories:
  - javascript
comments: true
---

## this

JavaScript 的 this 指向一个对象，而具体指向哪个对象是在运行时基于函数的执行环境动态绑定的，而非函数被声明时的环境。

### this 的指向

`this` 的指向大致可以分为以下 4 种。

- 作为对象的方法调用
- 作为普通函数调用
- 构造器调用
- Function.prototype.call 或 Function.prototype.apply 调用

<!-- more -->

#### 作为对象的方法调用

当函数作为对象的方法被调用时，this 指向该对象。

```javascript
var obj = {
  a: 1,
  getA: function() {
    alert(this === obj); // 输出 true
    alert(this.a); // 输出 1
  }
};
obj.getA();
```

#### 作为普通函数调用

当函数不作为对象的属性被调用时，也就是我们常说的普通函数方式，此时的 this 总是指向全局对象。在浏览器的 JavaScript 里，这个全局对象是 window 对象。

```javascript
window.name = 'globalName';

var getName = function() {
  return this.name;
};

console.log(getName()); // 输出 golbalName

// 或者：
window.name = 'globalName';

var myObject = {
  name: 'sven',
  getName: function() {
    return this.name;
  }
};

var getName = myObject.getName;
console.log(getName()); // globalName
```

有时候我们会遇到一些困扰，比如在 `div` 节点的事件函数内部，有一个局部的 `callback` 方法， `callback` 被作为普通函数调用时，`callback` 内部的 this 指向了 window，但我们往往是想让它指向该 `div` 节点，见如下代码:

```html
<html>
  <body>
    <div id="div1">我是一个div</div>
  </body>
  <script>
    window.id = 'window';

    document.getElementById('div1').onclick = function() {
      alert(this.id); // 输出 div1
      var callback = function() {
        alert(this.id); // 输出 window
      };
      callback();
    };
  </script>
</html>
```

此时有一种简单的解决方案，可以用一个变量保存 `div` 节点的引用:

```javascript
document.getElementById('div1').onclick = function() {
  var that = this;
  var callback = function() {
    alert(that.id); // 输出 div1
  };
  callback();
};
```

在 ES6 的 strict 模式下，这种情况下的 this 已经被规定为不会指向全局对象，而是 undefined ：

```javascript
function func() {
  'use strict';
  alert(this); // 输出 undefined
}
func();
```

#### 构造器调用

JavaScript 中没有类，但是可以从构造器中创建对象，同时也提供了 new 运算符，使得构造器看起来更像一个类。

除了宿主提供的一些内置函数，大部分 JavaScript 函数都可以当作构造器使用。构造器的外表跟普通函数一样，它们的区别在于被调用的方式。当用 new 运算符调用函数时，该函数总会返回一个对象，通常情况下，构造器里的 this 就指向返回的这个对象，见如下代码:

```javascript
var MyClass = function() {
  this.name = 'sevn';
};

var obj = new MyClass();
alert(obj.name); // sevn
```

但用 new 调用构造器时，还要注意一个问题，如果构造器显式地返回了一个 object 类型的对象，那么此次运算结果最终会返回这个对象，而不是我们之前期待的 this :

```javascript
var MyClass = function() {
  this.name = 'sevn';
  return {
    name: 'lili'
  };
};

var obj = new MyClass();
alert(obj.name); // lili
```

如果构造器不显式地返回任何数据，或者是返回一个非对象类型的数据，就不会造成上述问题:

```javascript
var MyClass = function() {
  this.name = 'sevn';
  this.getName = function() {
    return this.name;
  };
  return 'lili'; // 返回其他类型的数据 string
};

var obj = new MyClass();
alert(obj.name); // sevn
```

#### Function.prototype.call 或 Function.prototype.apply 调用

跟普通的函数调用相比，用 Function.prototype.call 或 Function.prototype.apply 可以动态地改变传入函数的 this :

```javascript
var obj1 = {
  name: 'sevn',
  getName: function() {
    return this.name;
  }
};

var obj2 = {
  name: 'lili'
};

console.log(obj1.getName()); // sevn
console.log(obj1.getName.call(obj2)); // lili
console.log(obj1.getName.apply(obj2)); // lili
```

call 和 apply 方法能很好地体现 JavaScript 的函数式语言特性，在 JavaScript 中，几乎第一次编写函数式语言风格的代码，都离不开 call 和 apply 。在 JavaScript 诸多版本的设计模式中，也用到了 call 和 apply 。

### 丢失的 this

这是一个经常遇到的问题，我们来看下面的代码:

```javascript
var obj = {
  myName: 'lili',
  getName: function() {
    return this.myName;
  }
};

console.log(obj.getName()); // lili

var getName2 = obj.getName;
console.log(getName2()); // undefined
```

当调用 `obj.getName` 时，`getName` 方法是作为 `obj` 对象的属性被调用的，此时的 this 指向 **obj** 对象，所以 `obj.getName()` 输出 `lili` 。

当用另外一个变量 `getName2` 来引用 `obj.getName`，并且调用 `getName2` 时，此时是普通函数调用方式，this 是指向全局 window 的，所以程序的执行结果是 undefined。

再看另一个例子，`document.getElementById` 这个方法名实在有点过长，我们大概尝试过用一个短的函数来代替它:

```javascript
var getId = function(id) {
  return document.getElementById(id);
};

getId('div1');
```

我们也许思考过为什么不能用下面这种更简单的方式:

```javascript
var getId = document.getElementById;
getId('div1');
```

现在不妨花 1 分钟时间，让这段代码在浏览器中运行一次:

```html
<html>
  <body>
    <div id="div1">我是一个 div</div>
  </body>
  <script>
    var getId = document.getElementById;
    getId('div1');
  </script>
</html>
```

在浏览器中执行过后就会发现，这段代码抛出了一个异常。这是因为许多引擎的 `document.getElementById` 方法的内部实现中需要用到 this 。这个 this 本来被期望指向 document，当 `getElementById` 方法作为 document 对象的属性被调用时，方法内部的 this 确实是指向 document 的。

但当用 `getId` 来引用 `document.getElementById` 之后，再调用 `getId`，此时就成了普通函数调用，函数内部的 this 指向了 window，而不是原来的 document。

我们可以尝试利用 apply 把 document 当作 this 传入 `getId` 函数，帮助"修正" this :

```javascript
document.getELementById = (function(func) {
  return function() {
    return func.apply(document, arguments);
  };
})(document.getElementById);

var getId = document.getElementById;
var div = getId('div1');

alert(div.id); // div1
```

## call 和 apply

ECMAScript 3 给 Function 的原型定义了两个方法，它们是 Function.prototype.call 和 Function.prototype.apply。在实际开发中，特别是在一些函数式风格的代码编写中，call 和 apply 方法尤为有用。在 JavaScript 版本的设计模式中，这两个方法的应用也非常广泛，能熟练运用这两个方法，是我们真正成为一句 JavaScript 程序员的重要一步。

### call 和 apply 的区别

Function.prototype.call 和 Function.prototype.apply 都是非常常用的方法。它们的作用是一样的，区别仅在于传入参数形式的不同。

apply 接受两个参数，第一个参数指定了函数体内 this 对象的指向，第二个参数为一个带下标的集合，这个集合可以为数组，也可以为类数组，apply 方法把这个集合中的元素作为参数传递给被调用的函数:

```javascript
var func = function(a, b, c) {
  alert([a, b, c]); // [1, 2, 3]
};

func.apply(null, [1, 2, 3]);
```

在这段代码中，参数 1，2，3 被放在数组中一起传入 func 函数，它们分别对应 func 参数列表中的 a、b、c。

call 传入的参数数量不固定，跟 apply 相同的是，第一个参数也是代表函数体内的 this 指向，从第二个参数开始往后，每个参数被依次传入函数。

```javascript
var func = function(a, b, c) {
  alert([a, b, c]); // [1, 2, 3]
};

func.call(null, 1, 2, 3);
```

当调用一个函数时，JavaScript 的解释器并不会计较形参和实参在数量、类型以及顺序上的区别，JavaScript 的参数在内部就是用一个数组来表示的。从这个意义上说，apply 比 call 的使用率更高，我们不必关心具体有多少参数被传入函数，只要用 apply 一股脑地推过去就可以了。

call 是包装在 apply 上面的一颗语法糖，如果我们明确地知道函数接受多少个参数，而且想一目了然地表达形参和实参的对应关系，那么也可以用 call 来传送参数。

当使用 call 或者 apply 的时候，如果我们传入的第一个参数为 null，函数体内的 this 会指向默认的宿主对象，在浏览器中则是 window :

```javascript
var func = function(a, b, c) {
  alert(this === window); // 输出 true
};

func.apply(null, [1, 2, 3]);
```

但如果是在严格格式下，函数体内的 this 还是为 null :

```javascript
var func = function(a, b, c) {
  'use strict';
  alert(this === null);
};

func.apply(null, [1, 2, 3]);
```

我们有时候使用 call 或者 apply 的目的不在于指定 this 指向，而是另有用途，比如借用其他对象的方法。那么我们可以传入 null 来代替某个具体的对象:

```javascript
Math.max.apply(null, [1, 2, 3, 4, 5]);
```
