## 什么是 HTML5

**HTML5** 是 W3C 与 WHATWG（Web Hypertext Application Technology Working Group）合作的结果，WHATWG 致力于 web 表单和应用程序，而 W3C 专注于 XHTML 2.0。

14 年 10 月，HTML5 完成了制定，他也是迄今为止最新的 HTML 稳定版本，**HTML5 将 HTML 从用于构造一个文档的一个简单标记，到一个完整的应用程序开发平台**。

## HTML5 新特性

在互联网应用迅速发展的时候，为了符合当代的网络需求，HTML5 带来了什么变化呢？

- 用于绘画的 `canvas` 元素
- 用于媒介播放的 `video` 和 `audio` 元素
- 对本地离线存储的更好的支持
- 语义化标签，比如 `article`、`footer`、`header`、`nav`、`section` 等
- 新的表单控件，比如 `calendar`、`date`、`time`、`email`、`url`、`search` 等
- 完全支持 CSS3
- Web 应用

`video`、`audio` 和 `canvas` 的加入可以减少浏览器对于插件的需求，而自己实现丰富的网络应用服务，用户可以更容易地在网页中添加和处理多媒体和图片内容（一般由 JavaScript 处理）。

语义化的标签和新属性的添加更丰富了文档的数据内容，有利于搜索引擎的索引整理、小屏幕设备和视障人士使用。

HTMl5 的存储，更加的安全与快速，这些数据不会被保存在服务器上，这些数据只用于用户请求网站数据上，并且它可以存储大量的数据，而不影响网站的性能。

## HTML5 模版

文档的声明不再需要长篇大论了，只要简单的 `<!DOCTYPE html>`。

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <title>文档标题</title>
  </head>

  <body>
    文档内容......
  </body>
</html>
```

- `<!DOCTYPE html>` 声明为 HTML5 文档，告诉浏览器正确的 HTML 版本，有助于正确显示网页内容（不区分大小写）。
- `<html>`，HTML 页面的根元素。
- `<head>` 是页面的头部元素，它包含了所有你想包含在 HTML 页面中但**不想在 HTML 页面中显示的内容**。这些内容包括你想在搜索结果中出现的关键字和页面描述，CSS 样式，字符集声明等等。
  - `<meta charset="utf-8">` 定义网页编码格式为 `utf-8`。
  - `<title>` 元素描述了文档的标题，出现在浏览器标签上。
- `<body>` 元素包含了可见的页面内容。

> 对于中文网页需要使用 `<meta charset="utf-8">` 声明编码，否则会出现乱码。

## 浏览器支持

现代的浏览器都支持 HTML5。

各大主流浏览器在 HTML Test[^htmltest] 上的得分情况[^score]：
[^htmltest]: HTML5 Test 网站，是用以测试对浏览器对热门新功能的支持。测试的满分是 555 分。
[^score]:截至 2018 年 7 月 15 日，整体上已经有较佳的支持

| 浏览器          | 正式版本 | 分数 |
| --------------- | -------- | ---- |
| Google Chrome   | 68       | 528  |
| Opera           | 55       | 528  |
| Microsoft Edge  | 18       | 496  |
| Mozilla Firefox | 62       | 497  |
| Apple Safari    | 11.2     | 477  |

### 完美的 Shiv 解决方案

针对古老的 IE 浏览器，`html5shiv` 是比较好的解决方案。`html5shiv` 主要解决 HTML5 提出的新的元素不被 IE6-8 识别，这些新元素不能作为父节点包裹子元素，并且不能应用 CSS 样式。

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <title>渲染 HTML5</title>
    <!--[if lt IE 9]>
      <script src="http://cdn.static.runoob.com/libs/html5shiv/3.7/html5shiv.min.js"></script>
    <![endif]-->
  </head>

  <body>
    <h1>我的第一篇文章</h1>
    <article>我能被 IE 识别吗？</article>
  </body>
</html>
```
