### 页面导入样式时，使用 link 和 @import 有什么区别?

- **从属关系的区别。**
  - `@import` 是 CSS 提供的语法规则，只有导入样式表的作用;
  - `link` 是 HTML 提供的标签，用于为当前文档加载外部资源。不仅可以加载 CSS 文件，还可以定义 `ref` 属性（`relationship`，表示该资源与当前文档的关系）、创建站点图标等。
    ```html
    <!-- 链接一个外部的样式表 -->
    <link href="main.css" rel="stylesheet" />
    <!-- 网站图标 -->
    <link rel="icon" href="favicon.ico" />
    ```
- **加载顺序区别。**
  - 在页面加载时，`link` 标签引入的资源同时被加载;
  - `@import` 引入 的 CSS 将在页面加载完毕后被加载。
- **DOM 可控性区别。**

  - 可以通过 JS 操作 DOM ，插入 `link` 标签来改变样式;
  - 由于 DOM 方法是基于文档的，无法使用 `@import` 的方式插入样式。

  ```js
  // 在文档中添加一个link节点，然后将href属性指向外部样式表的URL
  const link = document.createElement("link");
  link.type = "text/css";
  link.rel = "stylesheet";
  link.href = "reset-min.css";
  document.head.appendChild(link);
  ```

- **兼容性区别。** ()
  - `@import` 是 CSS2.1 才有的语法，故只可在 IE5+ 才能识别;
  - `link` 标签作为 HTML 元素，不存在兼容性问题。
