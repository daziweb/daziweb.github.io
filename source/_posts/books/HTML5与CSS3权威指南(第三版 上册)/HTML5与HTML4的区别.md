---
title: HTML5与HTML4的区别
tags:
  - CSS HTML
categories:
  - 书籍阅读
comments: true
---

## 迎接新的 Web 时代

### HTML5 的目标

为了能够创建更简单的 Web 程序，书写出更简洁的 HTML 代码。提供了很多新的 API，开发了新的属性、新的元素，使得页面变得非常清楚直观、容易理解。

<!--more-->

## HTML5 与 HTML4 的区别

### HTML5 中的标记方法

- **内容类型(ContentType)**

HTML5 的文件扩展符与内容类型保持不变。扩展符仍然为`.html`或`.htm`，内容类型(ContentType)仍然为`text/html`。

- **DOCTYPE 声明**

要建立符合标准的网页，DOCTYPE 声明是必不可少的关键组成部分，位于文件第一行。DOCTYPE 是 document type（文档类型）的简写，用来说明你用的 XMHTML 或者 HTML 是什么版本。缺少或错误的 document 将会造成 css 失效或半失效。

在 HTML5 中，刻意不使用版本声明，一份文档将会适用所有版本的 HTML。

```javascript
// HTML4
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
```

```javascript
// HTML5 不区分大小写 不区分单双引号
<!DOCTYPE html>
```

- 指定字符编码

```javascript
// HTML4
<meta http-equiv="Content-Type" content="text/html;charset=UTF-8">
```

```javascript
// HTML5
<meta charset="UTF-8">
```

两种写法都有效，但是不能同时混合使用两种方式。

```javascript
// 错误的写法
<meta charset="UTF-8" http-equiv="Content-Type" content="text/html;charset=UTF-8">
```

**注意：此写法为错误写法。**
**建议：从 HTML5 开始，对于文件的字符编码推荐使用 UTF-8。**

### HTML5 确保的兼容性

- 可以省略标记的元素

  - 不允许写结束标签的元素：area、base、br、col、command、embed、hr、img、input、keygen、link、meta、param、source、track、wbr。

  - 可以省略结束标记的元素有：li、dt、dd、p、rt、rp、optgroup、option、colgroup、thead、tbody、tfoot、tr、td、th。

  - 可以省略全部标记的元素有：html、head、body、colgroup、tbody。

- 具有 boolean 值的属性

  - 对于具有 boolean 值的属性，当只写属性而不指定属性值时，表示属性值为 true，如果想要将属性值设为 false，则可以不使用该属性。如果想将属性值设定为 true 时，可以将属性名设定为属性值，或将空字符串设定为属性值。

```javascript
<input type="checkbox" checked>
```
