Flex 布局，是一种当页面需要适应不同的屏幕大小以及设备类型时确保元素拥有恰当的行为的布局方式。

## 基本概念

采用 Flex 布局的元素（`display: flex`），叫 flex container （容器）；容器内所有子元素，叫 flex item（项目）。

flex 布局的方向由 main axis （主轴）和 cross axis （交叉轴）确定，主轴方向取决于 `flex-direction` 的值（默认：从右到左），交叉轴方向取主轴逆时针旋转 90 度的方向。

![01.svg](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/1e67d8565ecc4cb8b3b438fe1760a4ba~tplv-k3u1fbpfcp-watermark.image)

## 常见属性

有些属性只作用于 flex container（容器）；有些属性只作用于 felx item（项目）。

![01.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/22a06b0cd4b642c78dfa5060b6582a73~tplv-k3u1fbpfcp-watermark.image)

### flex container（容器）

#### flex-direction

`flex-direction` 属性定义了主轴的方向，指定了内部元素是如何在 flex 容器中布局的。

- `row`（默认）：和容器的 `direction` 属性（文本方向）相同。
  - 如果 `direction` 属性是 `ltr`，`row` 表示从左到右定向的水平轴;
  - 如果 `direction` 属性是 `rtl`，`row` 表示从右到左定向的轴。
- `row-reverse`：和 `row` 相反
- `column`：从上到下
- `column-reverse`：从下到上

![01.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/2c3adffd42e54b559b8bcc05b45bdfe6~tplv-k3u1fbpfcp-watermark.image)

#### flex-wrap

`flex-wrap` 指定 flex 元素单行显示还是多行显示 。如果允许换行，还可以指定行的堆叠方向。

- `no-wrap`（默认）：不换行
- `wrap`：换行
- `wrap-reverse`：换行，交叉轴发生 reverse（cross-start 和 cross-end 互换）。

![02.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/edad1996598b43f9855a9b0d3ed14d22~tplv-k3u1fbpfcp-watermark.image)

#### justify-content

`justify-content` 定义了元素在主轴方向上的分布规则。

常用属性值：

- `flex-start`：从行首位置开始排列
- `flex-end`：从行尾位置开始排列
- `center`：居中排列
- `space-between`：均匀排列每个元素，首个元素放置于起点，末尾元素放置于终点
- `space-around`：均匀排列每个元素，每个元素周围分配相同的空间
- `space-evenly`：均匀排列每个元素，每个元素之间的间隔相等

![01.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/f6842813eb414f35b6bf0572e51b7339~tplv-k3u1fbpfcp-watermark.image)

#### align-items

`align-items` 会统一设置所有直接子元素在交叉轴方向上的行内对齐方式。

常用属性值：

- `flex-start`：元素向交叉轴起点对齐
- `flex-end`：元素向交叉轴终点对齐
- `center`：在交叉轴居中，如果元素在交叉轴上的高度高于其容器，那么在两个方向上溢出距离相同
- `stretch`：沿交叉轴方向的尺寸拉伸至与容器一致。
- `baseline`：基线对齐，这里的 `baseline` 默认是指首行文字，即 `first baseline`，交叉轴起点到元素基线距离最大的子容器将会与交叉轴起始端相切以确定基线。

![01.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/a3fce9e1d307446c9d6f6354c2f9a8a4~tplv-k3u1fbpfcp-watermark.image)

#### align-content

`align-content` 定义了行在交叉轴方向的分布规则。

> **该属性对单行弹性盒子模型无效。**（即：带有 flex-wrap: nowrap）。

常用属性值同 `justify-content`。

![01.svg](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/782f932bb2a847e5a02278b8bd8f973a~tplv-k3u1fbpfcp-watermark.image)

#### flex-flow

`flex-flow` 属性是 `flex-direction` 和 `flex-wrap` 的简写。

### flex item（项目）

#### align-self

`align-self` 和 `align-items` 一样，定义元素在交叉轴方向上的行内对齐方式。但是 `align-self` 只对当前子元素生效，且优先级高于 `align-items`。

常见属性值，见 `align-items`。

![01.svg](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/1c771a2ebbcb4ff78850b9562f665ec5~tplv-k3u1fbpfcp-watermark.image)

#### flex

`flex` 是 `flex-grow`、`flex-shrink`、`flex-basis` 这三个属性的简写。

- `flex-grow`：指定了 flex 容器中剩余空间应该如何分配给项目。
- `flex-shrink`：指定了 flex 容器中超出空间应该如何压缩项目，仅在默认宽度之和大于容器的时候才会发生收缩。
- `flex-basis`：指定了 flex 元素在主轴方向上的初始大小，同时设置 `flex-basis` (除值为 `auto` 外) 和 `width/height` 时，`flex-basis` 优先。

注意：

- `<'flex-grow'>`，省略时默认值为 1。 (初始值为 0)
- `<'flex-shrink'>`，省略时默认值为 1。 (初始值为 1)
- `<'flex-basis'>`，省略时默认值为 0。(初始值为 auto)

flex 可能的组合方式：

![01.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/f9f5f3f640b24af1a672509b6bf04cfc~tplv-k3u1fbpfcp-watermark.image)

#### order

`order`，指定的是项目的排列顺序，默认值为 0，可以为负值，数值越小排列越靠前。

![01.svg](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/8a7d45de6a88466ca3f98f4f0505f4d7~tplv-k3u1fbpfcp-watermark.image)
