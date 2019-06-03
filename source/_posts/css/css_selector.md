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

![css 规则结构](https://ws1.sinaimg.cn/large/b3ad6cffly1g3o1fjr6nhj20hj01yaa4.jpg)

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
