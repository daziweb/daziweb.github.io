---
title: CSS选择器
tags:
  - CSS
categories:
  - 书籍阅读
comments: true
---

## css 规则结构

选择器(selector)和声明块(declaration block)。声明块由一个或多个声明(declaration)组成，每个声明则是一个属性-值对(property-value)。

![css 规则结构](http://ww1.sinaimg.cn/large/b3ad6cffly1g3o3b1u6i5j20hj01yaa4.jpg)

<!--more-->

## 元素选择器

最常见的元素选择器就是 HTML 元素。

## 通配选择器

通配选择器(universal selector)显示为一个星号(\*)。这个选择器可以与任何元素匹配，就像是一个通配符。

```css
* {
  color: red;
}
```

## 类选择器和 ID 选择器

类选择器(class selector)和 ID 选择器(ID selector)，它们允许以一种独立于文档元素的方式来指定样式。

类选择器要应用样式而不考虑具体涉及的元素，最常用的方法就是使用类选择器。类选择器要正常工作，需要直接引用一个元素的 class 属性中的值。这个引用前面往往有一个点号(.)，标记这是一个类选择器。通过这个点号，可以帮助类选择器与它可能结合的其他部分分隔开。

## 多类选择器

在 HTML 中，一个 class 值中有可能包含一个词列表，各个词之间用空格分隔。

```html
<p class="urgent warning">Hello Style</p>
```

```css
p.warning.urgent {
  background: red;
}
```

## ID 选择器

ID 选择器前面有一个(#)号。引用 id 属性中的值。在 HTML 文档中，ID 选择器会使用一次，而且仅一次。

## 属性选择器

属性选择器(attribute selector)可以根据元素的属性及属性值来选择元素。

### 简单属性选择器

如果希望选择有某个属性的元素，而不论该属性的值是什么，可以使用一个简单属性选择器。还可以根据多个属性进行选择，只需要将属性选择链接在一起即可。

```css
img[alt] {
  border: 2px solid red;
}

a[href][title] {
  font-weight: bold;
}
```

### 根据具体属性值选择

除了选择有某些属性的元素，还可以进一步缩小选择范围，只选择有特定属性值的元素。可以为任何元素指定属性和值组合，也可以把多个属性-值选择器链接在一起来选择一个文档。
**注意，这种格式要求必须与属性值完全匹配。**

```css
a[href='www.baidu.com'] {
  color: black;
  font-size: 18px;
}

a[herf='www.baidu.com'][title='百度一下，你就知道'] {
  color: red;
  font-size: 18px;
  font-weight: bold;
}

p[class='urgent warning'] {
  font-weight: bold;
}
```

### 根据部分属性值选择

如果属性能够接受词列表(词之间用空格分隔)，可以根据其中的任意一个词进行选择。为了不完全匹配，选择器使用波浪号(~)。

```css
p[class~='barren'] {
  font-weight: bold;
}
```

### 子串匹配属性选择器

```css
[foo^='bar'] /* 选择foo属性值以 bar 开头的所有元素 */
[foo$='bar'] /* 选择foo属性值以 bar 结尾的所有元素 */
[foo*='bar'] /* 选择foo属性值中包含子串 bar 的所有元素 */
```

### 特定属性选择类型

```css
img[src|='figure'] {
  border: 1px solid red;
}
```

### 后代选择器

```css
ul ol ul em {
  color: gray;
}

td.main a:link {
  color: blue;
}
```

### 选择子元素

```css
h1 > strong {
  color: red;
}
```

### 选择相邻兄弟元素

```css
h1 + p {
  margin-top: 0;
}
```

### 伪类选择器

```css
a:link {
  color: gray;
}

a:hover {
  color: blue;
}

a:visited {
  color: red;
}

a:active {
  color: pink;
}

input:focus {
  background: silver;
  font-weight: bold;
}
```

### 选择第一个子元素

```css
li:first-child {
  text-transform: uppercase;
}
```

### 选择首字母样式

```css
h2:first-letter {
  font-size: 200%;
}
```

### 设置之前和之后元素的样式

```css
h2:before {
  content: '';
  color: red;
}

body:after {
  content: '  The End.  ';
}
```
