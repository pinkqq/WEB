## BFC & IFC

什么是 BFC ？块格式化上下文（block level formatting context）。

什么是 IFC ？行内格式化上下文（inline level formatting context）。

了解 BFC 和 IFC 之前，我们先看看 **Formatting context** 是什么？

**Formatting context（格式化上下文）** 是页面中的一块渲染区域，并且有一套渲染规则，它决定了其子元素将如何定位，以及和其他元素的关系和相互作用。

## BFC

块格式化上下文（`Block Formatting Context`，BFC） 是 Web 页面的可视 CSS 渲染的一部分，是块盒子的布局过程发生的区域，也是浮动元素与其他元素交互的区域。

### 关于 Block

- **块级盒子**：`block-level box`，由块级元素生成。一个块级元素至少会生成一个块级盒子，但也有可能生成多个（例如列表项元素）。

- **块容器盒子**：`block container box`，块容器盒子侧重于当前盒子作为“容器”的这一角色，它不参与当前块的布局和定位，它所描述的仅仅是当前盒子与其后代之间的关系。换句话说，块容器盒子主要用于确定其子元素的定位、布局等。
- **块盒子**：`block box`，如果一个块级盒子同时也是一个块容器盒子，则称其为块盒子。

![block box](https://developer.mozilla.org/@api/deki/files/5995/=venn_blocks.png)

他们和 BFC 之间的关系，简单而言就是：

- `Block Container Box`：里面有 BFC
- `Block-level Box`：外面有 BFC
- `Block box`：Block Container Box + Block-level Box（里外都有 BFC）

### 触发 BFC

只要元素满足下面任一条件即可触发 BFC 特性：

- body 根元素
- 浮动元素：float 除 none 以外的值
- 绝对定位元素：position (absolute、fixed)
- display 取值为（block container box that are not block boxes）：
  - inline-block
  - table-cells
  - table-captions
  - flex items
  - grid cell
- overflow 除了 visible 以外的值 (hidden、auto、scroll)

### 能解决什么问题 ？

#### 一、外边距折叠（margin-collapse）

同一个 BFC 下外边距会发生折叠。只在自然流的 BFC 中存在，在排版思想中 margin 定义是周围留白空间，不是与其他元素的间距，所以并不是 CSS 的 bug。

![01.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/4a0415c2256d421ca97034bd1f79e6de~tplv-k3u1fbpfcp-watermark.image)

在例子里，我们为 `box1` 和 `box2` 都设置了 `margin:20px`，因为他们同处一个 BFC 发生外边距折叠，所以他们的间距是 20px。如果希望他们之间的间距为 40px，可以将其放入不同的 BFC 中。

```html
<div class="container">
  <div class="bfc">
    <div class="box">I am box1!</div>
  </div>
  <div class="box">I am box2!</p>
</div>
```

```css
.container {
  background-color: rgb(224, 206, 247);
  border: 5px solid rebeccapurple;
}

.box {
  width: 200px;
  height: 150px;
  background-color: white;
  border: 1px solid black;
  padding: 10px;
  margin: 20px;
}

.bfc {
  overflow: auto;
}
```

#### 二、BFC 可以包含浮动的元素，用于清除浮动

![01.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/e511919f995f42f3a67824b7cf69a3e9~tplv-k3u1fbpfcp-watermark.image)

在这个例子中，浮动元素 `.float` 超出了 `.box` 的边框，怎么才能让 `.box` 可以包含浮动元素呢？我们可以为 `.box` 触发 BFC，比如添加 `overflow: auto` 等。

![01.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/90f36f79473645eb8cc8cc817711dc1d~tplv-k3u1fbpfcp-watermark.image)

```html
<div class="box">
  <div class="float">I am a floated box!</div>
  <p>I am content inside the container.</p>
</div>
```

```css
.box {
  background-color: rgb(224, 206, 247);
  border: 5px solid rebeccapurple;
  /* overflow: auto; */
}

.float {
  float: left;
  width: 200px;
  height: 150px;
  background-color: white;
  border: 1px solid black;
  padding: 10px;
}
```

#### 三、BFC 可以阻止元素被浮动元素覆盖

![01.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/e511919f995f42f3a67824b7cf69a3e9~tplv-k3u1fbpfcp-watermark.image)

对上一个例子稍作修改，`.float` 和 `.box` 成为兄弟元素，怎么才能让 `.box` 不被 `.float` 遮盖呢？还是为 `.box` 触发 BFC。

![01.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/5db6e691d1c7425ea7bb39ecfef59863~tplv-k3u1fbpfcp-watermark.image)

```html
<div class="float">I am a floated box!</div>
<div class="box">
  <p>I am content inside the container.</p>
</div>
```

## IFC

行内格式化上下文（`inline level formatting context`）是一个网页的渲染结果的一部分。其中，各行内框（inline boxes）一个接一个地排列，其排列顺序根据书写模式（writing-mode）的设置来决定：

- 对于水平书写模式，各个框从左边开始水平地排列
- 对于垂直书写模式，各个框从顶部开始垂直地排列

![01.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/0dfa2140a79e42f291f4d40f3b63d4b6~tplv-k3u1fbpfcp-watermark.image)

### 行模型

![01.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/ab394b807be44dcda82dcee86d44fb24~tplv-k3u1fbpfcp-watermark.image)

- 行高（`line-height`）：top / bottom
- `font-size`：text-top / text-bottom

## 参考资料

- 视觉格式化模型：https://developer.mozilla.org/zh-CN/docs/Web/CSS/Visual_formatting_model
