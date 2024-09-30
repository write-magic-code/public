# HTML, CSS, and JavaScript

## 三件套是什么

### HTML

HTML 是一种标记语言，即没有编程能力，只能描述内容的结构。HTML 由一系列标签组成，每个标签代表一个元素，元素之间可以嵌套。HTML 用于描述网页的结构，包括标题、段落、列表、表格、图片、链接等。是网页的骨架，决定了网页逻辑上的结构。

### CSS

CSS 是一种样式表，用于描述网页的外观，包括 HTML 元素的位置、间隔、大小、装饰和交互等等，是网页的皮肤，决定了网页视觉上的表现。CSS 通过选择器选中元素，然后为这些元素设置样式。

### JS

JS 是一种用途广泛的脚本语言，在网页开发中，主要用于实现网页的交互功能，比如界面的动态效果、用户的交互行为、数据的获取与处理等等，网页大部分的交互、功能都通过 js 实现。是网页的肌肉，决定了网页的功能。

## 常规业务如何实现

一些古早的，没有前端框架时的网页开发，基本有如下几个步骤：

1. 编写 HTML 结构，描述网页的内容
2. 编写 CSS 样式，描述网页的外观
3. 编写 JS 脚本，实现网页的交互功能

假如要实现一个点击按钮，弹出一个提示框的功能，可以按照如下步骤实现：

### 写 HTML

```html
<!DOCTYPE html>
<html>
  <head>
    <title>点击按钮弹出提示框</title>
  </head>
  <body>
    <button id="btn" class="alert-button">点击我</button>
  </body>
</html>
```

可以理解为 HTML 是一种更易于编写的文字排版工具，可以通过标签来描述内容的结构。

HTML 自带相当数量的元素，其中大部分是描述性的，比如 `<p>...</p>` 表示段落，`<div>...</div>` 表示一个容器，`<section>...</section>` 表示文章中的一个章节，而 `section` 和 `div` 实际上并没有区别，只是编写 `section` 可以让代码更容易理清结构。同时还有少部分元素是具有功能的，`input` 表示一个输入框，`a` 表示一个链接，`img` 表示一个图片，`video` 用于播放视频等等，这些元素可以通过属性来描述更多的功能。

同时 HTML 的元素上有很多共用的属性，比如 `id` 用于标识元素（方便 JS 和 CSS 操作），`class` 辅助 CSS 的选中（这个后面会讲到），`style` 用于直接设置元素的样式，`onclick` 用于设置点击事件等等。

对于学习 HTML，不建议死记硬背，用到的时候查文档即可，文档只推荐 MDN，MDN 里面相关教程在 https://developer.mozilla.org/zh-CN/docs/Web/HTML

### 写 CSS

再根据上面写的 HTML，用 CSS 来美化外观。

每一段 CSS 中，第一行是选择器，用于选中元素，后面的花括号里面是样式，被选中的元素会应用这些样式。

```css
button.alert-button {
  background-color: #f44336;
}

button#btn {
  background-color: #4caf50;
  color: white;
  padding: 14px 20px;
  border: none;
  cursor: pointer;
}
```

CSS 在日常开发中需要使用的属性非常多而且有这样那样的特性，不建议死记硬背，用到的时候查文档即可，文档只推荐 MDN，MDN 里面相关教程在 https://developer.mozilla.org/zh-CN/docs/Web/CSS

### 写 JS

再使用 JS 来实现交互功能。

```js
// 先选中按钮元素
const btn = document.getElementById('btn');
// 给按钮添加点击事件
btn.addEventListener('click', () => {
  alert('Hello, world!');
});
```

就实现了一个点击按钮，弹出提示框的功能。

JS 是一门通用编程语言，但是在网页开发中，浏览器提供了非常多数量的 API 用于操作网页，在网页开发中大概分为两类：

1. DOM 操作：操作网页的元素，比如选中元素、添加元素、删除元素、修改元素的属性等等
2. BOM 操作：操作浏览器的功能，比如获取浏览器的信息、获取用户的位置、发送请求等等

### DOM 操作

DOM 是 Document Object Model 的缩写，即文档对象模型，是浏览器提供的一种 API，用于操作网页的元素。

要做的操作无非就是对元素增删改查，以及插入、移动、复制等等。

```js
// 创建、修改并插入到页面上。
const btn = document.createElement('button');
btn.id = 'btn';
btn.className = 'alert-button';
btn.innerText = '点击我';
btn.addEventListener('click', () => {
  alert('Hello, world!');
});
document.body.appendChild(btn);

// 对已经存在的元素进行操作
const btn = document.getElementById('btn');
btn.style.backgroundColor = '#4caf50';
```

同样无法赘述，建议用到的时候再去查 MDN。

### BOM 操作

大部分浏览器提供的，跟操作元素无关的功能，基本可以归为 BOM 操作，比如发起网络请求、获取用户位置、操作 cookie 等等。

```js
// 发起一个 GET 请求
fetch('https://api.github.com/users/').then(res => res.json()).then(console.log);

// 获取用户的位置
navigator.geolocation.getCurrentPosition();

// 操作 cookie
document.cookie = 'name=hello';
```

这个建议用到的时候直接搜索，比如搜索 `fetch`，`navigator.geolocation`，`document.cookie` 等等。
