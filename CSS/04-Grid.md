说到 CSS 的布局，肯定少不了 **Flex（弹性盒子）** 和 **Grid（网格布局）。**

我们通常说 **Flexbox** 是一种一维的布局，因为一个 **Flexbox** 一次只能处理一个维度上的元素布局，一行或者一列，也正因为这个特点，**Flexbox** 更适合用在组件和小规模的布局中。[关于 Flexbox 的详细用法，之前也做过总结了。](https://juejin.cn/post/6996858497576992805)

那么，面对更复杂或整体规模的布局，**Grid** 则更擅长了。

## 什么是 Grid

![01.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/238670d178034df7b8a620e2f91922c8~tplv-k3u1fbpfcp-watermark.image)

**Grid 和 Flexbox 最大的不同在于 Grid 是二维布局，它可以同时处理行和列上的布局，** 这使 Grid 更灵活更强大。

**Grid Layout** 擅长于将一个页面划分为几个主要区域，以及定义这些区域的大小、位置、层次等关系（前提是 HTML 生成了这些区域）。

“按行或列来对齐元素”，这听起来似乎像表格一样。 然而在布局上，网格比表格更灵活或更简单。 例如，网格容器的子元素可以自己定位，以便它们像 CSS 定位的元素一样，真正的有重叠和层次。

## 属性

Grid 和 Flexbox 一样，也有容器（`container`）和项目（`item`）的概念，并且属性也分为了两大类：**容器属性**（作用于容器之上）和 **项目属性**（作用于项目之上）。

## 容器属性

### display

通过 `display`，指定一个容器采用网格布局。

- `grid`，容器元素外部显示类型是**块级元素**，内部显示类型是 grid
- `inline-grid`，容器元素外部显示类型是**内联元素**，内部显示类型是 grid

当内部显示类型变成 grid 后，所有的项目显示类型为 grid box。

### grid-template-columns & grid-template-rows

`grid-template-\*`，划分行和列，定义网格线的名称和网格轨道的尺寸大小。

- `grid-template-columns`，基于列
- `grid-template-rows`，基于行

#### 长度/百分比

一个三行三列的网格，列宽和行高都是 100px：

- 长度
  ```css
  .container {
    display: grid;
    grid-template-columns: 100px 100px 100px;
    grid-template-rows: 100px 100px 100px;
  }
  ```
- 百分比
  ```css
  .container {
    display: grid;
    grid-template-columns: 33.33% 33.33% 33.33%;
    grid-template-rows: 33.33% 33.33% 33.33%;
  }
  ```

![01.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/5bf4a7e118fb4d6c91e94fa4b80bcb80~tplv-k3u1fbpfcp-watermark.image)

#### repeat()

`repeat()`，表示网格轨道的重复部分，以一种更简洁的方式去表示大量而且重复列的表达式。

接受两个参数，第一个参数是重复的次数，第二个参数是所要重复的值（可以是一组值）。

```css
.container {
  display: grid;
  grid-template-columns: repeat(3, 100px);
  grid-template-columns: repeat(
    2,
    100px 20px 80px
  ); /* 100px 20px 80px 会重复二次*/
}
```

如果网格容器在相关轴上具有确定的大小或最大大小，希望每一行（或每一列）容纳尽可能多的单元格，这时可以使用 `auto-fill` 关键字表示自动填充。

```css
.container {
  display: grid;
  grid-template-columns: repeat(
    auto-fill,
    100px
  ); /* 每列宽度100px，自动填充至容器不能放置更多的列。*/
}
```

![01.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/11fd6b8d897845d893352e5aa7a38d5e~tplv-k3u1fbpfcp-watermark.image)

#### fr（fraction，片段）

`fr`，非负值，定义网格轨道大小的弹性系数。每个使用了 `fr` 单位的网格轨道会按比例分配剩余的可用空间。

例如，第二列的宽度是第一列的两倍：

```css
.container {
  display: grid;
  grid-template-columns: 1fr 2fr;
}
```

弹性系数可以和绝对长度一起使用。例如，第一列宽为 100px，第二列的宽度是第三列的一半：

```css
.container {
  display: grid;
  grid-template-columns: 100px 1fr 2fr;
}
```

#### minmax()

`minmax()`，定义了一个长宽范围的闭区间，第一个参数是最小值，第二个参数是最大值。

例如，`minmax(100px, 1fr)` 表示列宽不小于 `100px`，不大于 `1fr`：

#### 网格线的名称

使用方括号，指定每一根网格线的名字，方便以后的引用。同一根线允许有多个名字，比如 `[fifth-line row-5]`。

3 x 3 的网格布局中，有 4 根垂直网格线和 4 根水平网格线：

```css
.container {
  display: grid;
  grid-template-columns: [c1] 100px [c2] 100px [c3] auto [c4];
  grid-template-rows: [r1] 100px [r2] 100px [r3] auto [r4];
}
```

### gap（grip-gap）

`gap`，用来设置网格行与列之间的间隙，该属性是 `row-gap` 和 `column-gap` 的简写形式。

> 起初是用 `grid-gap` 属性来定义的，目前逐渐被 `gap` 替代。

**语法：**

```
gap: row-gap column-gap?
```

`column-gap` 是可选的，缺失时则会被设置成跟 `row-gap` 一样的的值。

设置行间距为 `10px`，列间距为 `20px`：

```css
gap: 20px 10px;
```

![01.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/7109a4867246425b8e172ca3bbd5600d~tplv-k3u1fbpfcp-watermark.image)

#### column-gap（grip-column-gap）

`column-gap`，设置列元素之间的间隙大小。（列间距）

#### row-gap（grip-row-gap）

`row-gap`，设置行元素之间的间隙大小。（行间距）

### grid-template-areas

`grid-template-areas`，用于定义区域，一个区域由单个或多个单元格组成。

![01.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/ed7c6483ca404d3c9f3e909cc2aad160~tplv-k3u1fbpfcp-watermark.image)

操练一个常规布局（如上图）：顶部是 Header（高 50px，宽撑满容器），左侧是侧导航（宽 150px，高撑满容器剩余空间），右侧上下分成内容区域（自动撑满）和 Footer（高 30px，宽自动撑满）。

- 设置 `display: grid`
- 区域划分：三行两列 `grid-template-areas`
- 行/列的宽高设置 ` grid-template-rows/columns`
- 指定当前元素所在的区域位置 `grid-area`

```css
.contianer {
  display: grid;
  grid-template-areas:
    "head head"
    "nav main"
    "nav foot";
  grid-template-rows: 50px 1fr 30px;
  grid-template-columns: 150px 1fr;
}

.container > nav {
  grid-area: nav;
  background-color: lightsalmon;
}
```

如果某些区域不需要利用，则使用"点"（`.`）表示。

```css
grid-template-areas:
  "a . c"
  "d . f"
  "g . i";
```

### grid-auto-flow

`grid-auto-flow`，控制项目布局的策略（排列反向、是否紧密 `dense`）

**此属性有两种形式：**

- 单个关键字：`row`、`column`，或 `dense` 中的一个。
- 两个关键字：`row dense` 或 `column dense`。

**取值：**

- `row`：默认值，逐行排列元素，在必要时增加新行。
- `column`：逐列排列元素，在必要时增加新列。
- `dense`：**使用“稠密”堆积算法**，如果后面出现了稍小的元素，则会试图去填充网格中前面留下的空白。这样做会填上稍大元素留下的空白，但同时也可能导致原来出现的次序被打乱。

![未命名文件.jpg](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/aeebf90547a84224ba8ba699076b66a6~tplv-k3u1fbpfcp-watermark.image)

### \*-items

#### justify-items & align-items

`justify-items`，设置单元格内容的水平位置；`align-items`，设置单元格内容的垂直位置。

`justify-items` & `align-items` 取值情况相同，属性值有这几种情况：

- 关键词： `normal`，`auto`，或 `stretch` 任选其一
- 基线对齐：`baseline`， 可选 `first` 或 `last` 之一为前缀
- 位置对其：`center`，`start`，`end`，`flex-start`，`flex-end`，`self-start`，`self-end`，`left` 或 `right` 任选其一，可选 `safe` 或 `unsafe` 之一为前缀

常用属性值演示：

- `start`

![01.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/f7d116fb65844a38bc5fa11a4d11ce8a~tplv-k3u1fbpfcp-watermark.image)

- `end`

![02.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/4c904126410f49c5a25d876003636104~tplv-k3u1fbpfcp-watermark.image)

- `center`

![03.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/4ca6d2740b3f411092cbc43c05dee0fe~tplv-k3u1fbpfcp-watermark.image)

- `stretch`

![04.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/132e6f23548e4b6291158996f330d31f~tplv-k3u1fbpfcp-watermark.image)

#### place-items

`place-items` 是 `align-items` 和 `justify-items` 的合并简写形式。

```css
place-items: <align-items> <justify-items>;
```

### \*-content

#### justify-content & align-content

`justify-content` 是整个内容区域在容器里面的水平位置，`align-content` 是整个内容区域的垂直位置。

`justify-content` & `align-content` 的取值和 `\*-items` 的完全一样。

常见属性值展示：

- `start`

![01.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/00bc4a767b844c3aa47bfa0a270f1bb4~tplv-k3u1fbpfcp-watermark.image)

- `end`

![02.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/334c1486dd6c415b87eb9b3fef78b29a~tplv-k3u1fbpfcp-watermark.image)

- `center`

![03.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/345caa6f1b9b414685ef766fafa1e2ae~tplv-k3u1fbpfcp-watermark.image)

- `stretch`

![04.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/7ad6d2aa7e14493b966bdc659f3a15b5~tplv-k3u1fbpfcp-watermark.image)

- `space-between`

![05.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/00f9d8c9df4a4209b4d14d58b17d7c2f~tplv-k3u1fbpfcp-watermark.image)

- `space-around`

![06.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/9c53f5f775f24359b67471c4305d437f~tplv-k3u1fbpfcp-watermark.image)

- `space-evenly`

![07.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/486b80699e3144c39cdbc07b027fa0d8~tplv-k3u1fbpfcp-watermark.image)

#### place-content

`place-content` 是 `align-content` 和 `justify-content` 的简写。

```css
place-content: <align-content> <justify-content>;
```

### grid-auto-columns & grid-auto-rows

假设我们用 grid-template-columns 和 grid-template-rows 指定了一个 2X2 的网格布局，当我们指定其中一个项目在第二行第五列（超出现有网格）时，浏览器会自动生成多余的网格，以便放置该项目。

```css
/* 2X2 的网格布局 */
.container {
  grid-template-columns: 60px 60px;
  grid-template-rows: 90px 90px;
}
/* 网格内的项目 */
.item-a {
  grid-column: 1 / 2;
  grid-row: 2 / 3;
}
/* 网格外的项目 */
.item-b {
  grid-column: 5 / 6;
  grid-row: 2 / 3;
}
```

![01.svg](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/ed9412fff9624d6d8826e0b5992a96da~tplv-k3u1fbpfcp-watermark.image)

那么，网格外的项目宽高是多少呢？

`grid-auto-columns` & `grid-auto-rows` 就是用来指定网格外项目的尺寸的。如果不指定这两个属性，浏览器完全根据单元格内容的大小，决定新增网格的列宽和行高。

```css
.container {
  grid-auto-columns: 60px;
}
```

![01.svg](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/de75cafaf1ee4fb4a2469f3fe9338634~tplv-k3u1fbpfcp-watermark.image)

## 项目属性

### grid-column-\* & grid-row-\*

`grid-column-start`、`grid-column-end`、`grid-row-start` 和 `grid-row-end`，这四个属性分别指定了项目的四个边框，以确定某个网格项目的大小和具体位置。

- `grid-column-start`：左边框所在的垂直网格线
- `grid-column-end`：右边框所在的垂直网格线
- `grid-row-start`：上边框所在的水平网格线
- `grid-row-end`：下边框所在的水平网格线

取值：

- `<line>`，可以是 **网格线的数字编号**（0 无效，负整数，则从显式网格的末端开始，反向计数）或者是 **网格线的名字**
- `span <number> `，指定跨越网格线的数量，默认为 1，负整数或 0 无效。
- `span <name>`，一直跨越，直到触碰指定的网格线（`<name>`）
- `auto`，表示自动放置，自动跨度或默认跨度为 1。

```css
.item-b {
  grid-column-start: 1;
  grid-column-end: span col4-start;
  grid-row-start: 2;
  grid-row-end: span 2;
}
```

![01.svg](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/7b7fe4e3c9f2443a98f322f0009c020c~tplv-k3u1fbpfcp-watermark.image)

使用这四个属性，如果产生了项目的重叠，则使用 `z-index` 属性指定项目的重叠顺序。

### grid-column & grid-row

**`grid-column`** 属性是 `grid-column-start` 和 `grid-column-end` 的合并简写形式；**`grid-row`** 属性是 `grid-row-start` 属性和 `grid-row-end` 的合并简写形式。

```css
grid-column: <start-line> / <end-line>;
grid-row: <start-line> / <end-line>;
```

### grid-area

`grid-area` ，用于指定项目放置区域。

它有两种取值方式：

- 作为 `grid-row-start`、`grid-column-start`、`grid-row-end`、`grid-column-end` 的合并简写形式：

  ```css
  grid-area: <row-start> / <column-start> / <row-end> / <column-end>;
  ```

- 区域名：
  ```css
  grid-area: <name>;
  ```

### \*-self

#### justify-self & align-self

`justify-self` / `align-self` 的作用和取值和 `justify-items` / `align-items` 是一样的，唯一的区别在于前者只作用于单个项目。

还是展示一下常用属性：

- `start`

![01.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/8e8adb50b9654f9ca688ba2bd0f636a6~tplv-k3u1fbpfcp-watermark.image)

- `end`

![02.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/a32f8ffaaf254cc99c3f0bf6b1d4d426~tplv-k3u1fbpfcp-watermark.image)

- `center`

![03.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/aab181660b3646edb8acaa8b5490e436~tplv-k3u1fbpfcp-watermark.image)

- `stretch`

![0-stretch.svg](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/f941e7e3c7e2438ca7112f79e6a886aa~tplv-k3u1fbpfcp-watermark.image)

#### place-self

`place-self` 是 `align-self` 和 `justify-self` 的合并简写形式。

```css
place-self: <align-self> <justify-self>;
```

如果第二个值被忽略，则表示两个值相等。
