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

HTML5 的文件扩展符与内容类型保持不变。扩展符仍然为 `.html` 或 `.htm`，内容类型(ContentType)仍然为 `text/html`。

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

- **指定字符编码**

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

**_建议：从 HTML5 开始，对于文件的字符编码推荐使用 UTF-8。_**

### HTML5 确保的兼容性

#### 可以省略标记的元素

- 不允许写结束标签的元素：area、base、br、col、command、embed、hr、img、input、keygen、link、meta、param、source、track、wbr。

- 可以省略结束标记的元素有：li、dt、dd、p、rt、rp、optgroup、option、colgroup、thead、tbody、tfoot、tr、td、th。

- 可以省略全部标记的元素有：html、head、body、colgroup、tbody。

#### 具有 boolean 值的属性

- 对于具有 boolean 值的属性，当只写属性而不指定属性值时，表示属性值为 true，如果想要将属性值设为 false，则可以不使用该属性。如果想将属性值设定为 true 时，可以将属性名设定为属性值，或将空字符串设定为属性值。

```javascript
// 只写属性不写属性值代表属性为 true
<input type="checkbox" checked>

// 不写属性代表属性为 false
<input type="checkbox">

// 属性值 = 属性名 代表属性为 true
<input type="checkbox" checked="checked">

// 属性值 = 空字符串 代表属性为 true
<input type="checkbox" checked="">
```

#### 省略引号

在指定属性值的时候，属性值两边可使用单引号或双引号。

在 HTML5 中，若属性值不包括空字符串、"<"、">"、"="、单引号、双引号等字符时，属性值两边的引号可以省略。

```javascript
<input type="text">
<input type='text'>
<input type=text>
```

### HTML5 示例代码

```javascript
<!DOCTYPE html>
<meta charset="UTF-8">
<title>HTML5 标记代码示例</title>
<p> 这段代码是根据 HTML5 语法
<br/> 编写出来的。
```

### 新增的元素和废除的元素

#### 新增的结构元素

（1）section 元素

section 元素表示页面中的一个内容区块，比如章节、页眉、页脚或页面中的其他部分。呆以和其他元素结合使用，标示文档结构。

```javascript
// HTML5
<section>...</section>
```

```javascript
// HTML4
<div>...</div>
```

（2）article 元素

article 元素表示页面中的一块和上下文不相关的独立内容，譬如博客中的一篇文章或报纸中的一篇文章。

```javascript
// HTML5
<article>...</article>
```

```javascript
// HTML4
<div>...</div>
```

（3）aside 元素

aside 元素表示 article 元素的内容之外的、与 article 元素的内容相关的辅助信息。

```javascript
// HTML5
<aside>...</aside>
```

```javascript
// HTML4
<div>...</div>
```

（4）header 元素

header 元素表示页面中一个内容区块或整个页面的标题。

```javascript
// HTML5
<header>...</header>
```

```javascript
// HTML4
<div>...</div>
```

（5）footer 元素

footer 元素表示整个页面或页面中一个内容区块的脚注。一般来说，它会包含创作的相关信息。

```javascript
// HTML5
<footer>...</footer>
```

```javascript
// HTML4
<div>...</div>
```

（6）nav 元素

nav 元素表示页面中导航链接的部分。

```javascript
// HTML5
<nav>...</nav>
```

```javascript
// HTML4
<ul>...</ul>
```

（7）figure 元素

figure 元素表示一段独立的流内容，一般表示文档主体流内容中的一个独立单元。使用 figcaption 元素为 figure 元素组添加标题。

```javascript
// html5
<figure>
  <figcaption>PRC</figcaption>
  <p> 这是P标签
</figure>
```

```javascript
// html4
<dl>
  <h1>PRC</h1>
  <p>这是P标签</p>
</dl>
```

（8）main 元素

main 元素表示网页中的主要内容。主内容区域意指与网页标题或应用程序中本页面主要功能直接相关或进行扩展的内容。

```javascript
// html5
<main>...</main>
```

```javascript
// html4
<div>...</div>
```

#### 新增的其他元素

（1）video 元素

video 元素用于定义视频。

```javascript
// html5
<video src="movie.ogg" controls="controls">
  video 元素
</video>
```

```javascript
// html4
<object type="video/ogg" data="movie.ogv">
  <param name="src" value="movie.ogv">
</object>
```

（2）audio 元素

audio 元素用于定义音频。

```javascript
// html5
<audio src="someaudio.wav">audio 元素</audio>
```

```javascript
// html4
<object type="application/ogg" data="someaudio.wav">
  <param name="src" value="someaudio.wav">
</object>
```

（3）embed 元素

embed 元素用来插入各种多媒体，格式可以是 Midi、Wav、AIFF、AU、MP3 等。

（4）mark 元素

mark 元素主要用来在视觉上向用户呈现那些需要突出显示或高亮显示的文字。mark 元素的一个比较典型的应用就是在搜索结果中向用户高亮显示搜索关键词。

```javascript
// html5
<mark>...</mark>
```

```javascript
// html4
<span>...</span>
```

（5）progress 元素(HTML5 中新增元素)

progress 元素表示运行中的进程，可以使用 progress 元素来显示 JavaScript 中耗费时间的函数的进程。

```javascript
// html5
<progress />
```

（6）meter 元素(HTML5 中新增元素)

meter 元素表示度量衡，仅用于已知最大值和最小值的度量。必须定义度量的范围，既可以在元素的文本中，也可以 min/max 属性中定义。

```javascript
// html5
<meter value="30" min="0" max="100">
  30%
</meter>
```

（7）time 元素(HTML5 中新增元素)

time 元素用于表示日期或时间，也可以同时表示两者。

```javascript
// html5
<time />
```

（8）wbr 元素

wbr 元素表示软换行。wbr 元素与 br 元素的区别是：br 元素是此处必须换行，而 wbr 元素意思就是浏览器窗口或父级元素的宽度足够宽时(没必要换行时)，不进行换行，而当宽度不够时，主动在此处进行换行。wbr 元素好像对字符型的语言用处比较大，但是对于中文，貌似没多大用处。

（9）canvas 元素

canvas 元素表示图形，比如图表和其他图像。

```javascript
// html5
<canvas id="myCanvas" width="200" height="200" />
```

```javascript
// html4
<object data="inc/hdr.svg" type="image/svg+xml" width="200" height="200" />
```

（10）dialog 元素

dialog 元素表示对话框。

#### 新增的 input 元素的类型

- email: 表示必须输入 e-mail 地址的文本输入框。
- url: 表示必须输入 URL 地址的文本输入框。
- number: 表示必须输入数值的文本输入框。
- range: 表示必须输入一定范围内数字值的文本输入框。
- Date Pickers: HTML5 选取日期和时间
  - date: 年 月 日
  - month: 年 月
  - week: 年 周
  - time: 时间（小时和分钟）
  - datetime: 年 月 日 时间（UTC 时间）
  - datetime-local: 年 月 日 时间（本地时间）

#### 废除的元素

1 能使用 CSS 替代的元素

对于 basefont、big、center、font、s、strike、tt、u 等元素，由于它们的功能都是纯粹为画面展示服务的，而 HTML5 中提倡把画面展示性功能统一放在 CSS 样式表中统一编辑，所以将这些元素废除，使用编辑 CSS、添加 CSS 样式表的方式进行替代。

2 不再使用 frame 框架

对于 frameset 元素、frame 元素与 noframes 元素，由于 frame 框架对网页可用性存在负面影响，在 HTML5 中已不支持 frame 框架，只支持 iframe 框架，或者由服务器方创建的由多个页面组成的复合页面的形式。

3 只有部分浏览器支持的元素

marquee 元素只被 Internet Explorer 支持。
