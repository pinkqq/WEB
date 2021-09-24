## 前言

这是一篇剔除了计算机绘图通用知识点后的纯 SVG 教程，比如网格坐标、path 路径、填充描边等，可以在 [Canvas 保姆级教程（上）：绘制篇](https://juejin.cn/post/7008064185972031524) 得到补充或者更详细的介绍。

## 什么是 SVG

SVG 指可伸缩矢量图形 (Scalable Vector Graphics)

## 优势

- 实现了 DOM 接口（比 Canvas 方便）
- 不需要安装第三方扩展（如 flash）
- 可被非常多的工具读取和修改（比如记事本）
- 与 JPEG 和 GIF 图像比起来，尺寸更小，且可压缩性更强。
- 是可伸缩的
- 图像可在任何的分辨率下被高质量地打印
- 可在图像质量不下降的情况下被放大
- 图像中的文本是可选的，同时也是可搜索的（很适合制作地图）
- 可以与 Java 技术一起运行
- 是开放的标准
- 文件是纯粹的 XML

## SVG vs Canvas

- **描述图形的方式不同：**

  - SVG 是一种使用 **XML** 描述 2D 图形的语言。每个被绘制的图形均被视为对象，如果 SVG 对象的属性发生变化，那么浏览器能够自动重现图形。
  - Canvas 通过 **JavaScript** 来绘制 2D 图形。一旦图形被绘制完成，它就不会继续得到浏览器的关注。如果其位置发生变化，那么整个场景也需要重新绘制。

- **是否可操控：**

  - SVG 基于 XML，这意味着它实现了 DOM 接口，每个元素都是可获得的，可以为某个元素附加 JavaScript 事件处理器。
  - Canvas 是逐像素进行渲染的。

- **适用场景**：

  - Canvas 提供的功能更原始，适合像素处理，动态渲染和大数据量绘制

  - SVG 功能更完善，适合静态图片展示，高保真文档查看和打印的应用场景

| Canvas                                               | SVG                                                     |
| ---------------------------------------------------- | ------------------------------------------------------- |
| 依赖分辨率                                           | 不依赖分辨率                                            |
| 不支持事件处理器                                     | 支持事件处理器                                          |
| 单个 Canvas 元素                                     | 多种图形元素（react、path...）                          |
| 只能脚本渲染                                         | 支持脚本和 CSS                                          |
| 用户交互到像素（x,y）                                | 用户交互到图形元素（path、rect）                        |
| 能够以 .png 或 .jpg 格式保存结果图像                 | 复杂度高会减慢渲染速度（任何过度使用 DOM 的应用都不快） |
| 适合小面积，大数量应用场景（最适合图像密集型的游戏） | 适合大面积，小数量应用场景（比如谷歌地图）              |

## 一个简单的示例

```js
<svg
  version="1.1"
  baseProfile="full"
  width="300"
  height="200"
  xmlns="http://www.w3.org/2000/svg"
>
  <rect width="100%" height="100%" fill="red" />

  <circle cx="150" cy="100" r="80" fill="green" />

  <text x="150" y="125" font-size="60" text-anchor="middle" fill="white">
    SVG
  </text>
</svg>
```

1. **从 `<svg/>` 根元素开始：**

   - 应舍弃来自 (X)HTML 的 doctype 声明，因为基于 SVG 的 DTD 验证导致的问题比它能解决的问题更多。
   - SVG 2 之前，`version` 属性和 `baseProfile` 属性用来供其他类型的验证识别 SVG 的版本。SVG 2 不推荐使用 `version` 和 `baseProfile` 这两个属性。
   - 作为 XML 的一种方言，SVG 必须正确的绑定 **命名空间** （在 xmlns 属性中绑定）。
   - `width` 和 `height` 属性可设置此 SVG 文档的宽度和高度。

2. **`<rect/>`**：绘制一个完全覆盖图像区域的矩形，把背景颜色设为红色。
3. **`<circle/>`**：一个半径 80px 的绿色圆圈绘制在红色矩形的正中央 （向右偏移（`cx`）150px，向下偏移（`cy`）100px，即圆心位置是 `(cx,cy)`）。
4. **绘制文字**：填充为白色，设置居中的锚点（在这里，中心点对应于绿色圆圈的中点）

## 基本形状

这张图展示了所有的基本形状，以及生成它们的代码：

![shape](https://developer.mozilla.org/@api/deki/files/359/=Shapes.png)

```xml
<svg width="200" height="250" version="1.1" xmlns="http://www.w3.org/2000/svg">

  <rect x="10" y="10" width="30" height="30" stroke="black" fill="transparent" stroke-width="5"/>
  <rect x="60" y="10" rx="10" ry="10" width="30" height="30" stroke="black" fill="transparent" stroke-width="5"/>

  <circle cx="25" cy="75" r="20" stroke="red" fill="transparent" stroke-width="5"/>
  <ellipse cx="75" cy="75" rx="20" ry="5" stroke="red" fill="transparent" stroke-width="5"/>

  <line x1="10" x2="50" y1="110" y2="150" stroke="orange" fill="transparent" stroke-width="5"/>
  <polyline points="60 110 65 120 70 115 75 130 80 125 85 140 90 135 95 150 100 145"
      stroke="orange" fill="transparent" stroke-width="5"/>

  <polygon points="50 160 55 180 70 180 60 190 65 205 50 195 35 205 40 190 30 180 45 180"
      stroke="green" fill="transparent" stroke-width="5"/>

  <path d="M20,230 Q40,205 50,230 T90,230" fill="none" stroke="blue" stroke-width="5"/>
</svg>
```

### 矩形 rect

```xml
<!-- 矩形 -->
<rect x="10" y="10" width="30" height="30"/>
<!-- 带圆角的矩形 -->
<rect x="60" y="10" rx="10" ry="10" width="30" height="30"/>
```

- **`x / y`**：矩形左上角的 x/y 位置
- **`width / height`**：矩形的宽度/高度
- **`rx / ry`**：圆角的 x/y 方位的半径，默认为 0

### 圆形 circle

```xml
<circle cx="25" cy="75" r="20" />
```

- **`r`**：圆的半径
- **`cx / cy`**：圆心的 x/y 位置

### 椭圆 ellipse

`ellipse` 是 `circle` 元素更通用的形式，你可以分别缩放圆的 x 半径和 y 半径：

```xml
<ellipse  cx="75" cy="75" rx="20" ry="5"/>
```

- **`cx / cy`**：圆心的 x/y 位置
- **`rx / ry`**：圆的 x/y 半径

### 线 line

```xml
<line x1="10" x2="50" y1="110" y2="150"/>
```

- `x1 / y1`：起点位置
- `x2 / y2`：终点位置

### 折线 polyline

折线的的所有点位置都放在一个 points 属性中：

```xml
<polyline points="60 110, 65 120, 70 115, 75 130, 80 125, 85 140, 90 135, 95 150, 100 145"/>
```

- **`points`**：每个点必须包含 2 个数字，一个是 x 坐标，一个是 y 坐标。

注意要把填充色设置为透明，`fill="transparent"`

### 多边形 polygon

和折线很像，它们都是由连接一组点集的直线构成。不同的是，polygon 的路径**在最后一个点处自动回到第一个点**。

```xml
<polygon points="50 160, 55 180, 70 180, 60 190, 65 205, 50 195, 35 205, 40 190, 30 180, 45 180"/>
```

### 路径 path

SVG 中最常见的形状，也是最强大的，它可以创建所有的形状。

另外，path 只需要设定很少的点，就可以创建平滑流畅的线条（比如曲线）。虽然 polyline 元素也能实现类似的效果，但是必须设置大量的点（点越密集，越接近连续，看起来越平滑流畅），并且这种做法不能够放大（放大后，点的离散更明显）。所以对路径的良好理解很重要。

```xml
<path d="M 20 230 Q 40 205, 50 230 T 90230"/>
```

- **`d`**：一个点集数列以及其它关于如何绘制路径的信息，它的规则是 “命令+参数”。

  - `M x y = moveto`，移动笔触到 `(x,y)`
  - `L x y = lineto`，会将当前位置和新位置 `(x,y)` 连线
  - `H x = horizontal lineto`，在水平方向上移动并连线
  - `V y = vertical lineto`，在垂直方向上移动并连线
  - `C x1 y1, x2 y2, x y = curveto`，三次贝塞尔曲线
    - `(x1,y1)` 是起点的控制点
    - `(x2,y2)` 是终点的控制点
    - `(x,y)` 是曲线的终点
  - `S x2 y2, x y = smooth curveto`，简写的三次贝塞尔曲线
    - 如果跟在一个 C 或 S 命令后面，则它的第一个控制点会被假设成前一个命令曲线的第二个控制点的中心对称点
    - 如果单独使用，即前面没有 C 或 S 命令，那当前点将作为第一个控制点。
  - `Q x1 y1, x y = quadratic Bézier curve`，二次贝塞尔曲线
    - `(x1,y1)` 是起点
    - `(x,y)` 是曲线的终点
  - `T x y = smooth quadratic Bézier curveto`，简写的二次贝塞尔曲线
    - 如果跟在一个 Q 或 T 命令后面，它会通过前一个控制点，推断出一个新的控制点
    - 如果单独使用，那么控制点就会被认为和终点是同一个点，即将是一条直线。
  - `A = elliptical Arc`，弧形，详情见下方
  - `Z = closepath`，闭合路径，从当前点画一条直线到路径的起点，不区分大小写

每一个命令都有两种表示方式，

- **大写字母**，表示采用**绝对定位**。
- **小写字母**，表示采用**相对定位**（例如：从上一个点开始，向上移动 10px，向左移动 7px）。

[通过 canvas 中的 path 路径](https://juejin.cn/post/7008064185972031524)，也可以增加对 path 路径的更多了解。

#### 弧形

```
A rx ry x-axis-rotation large-arc-flag sweep-flag x y
```

- `rx / ry`：x/y 轴半径
- `x-axis-rotation`：旋转情况，正数为顺时针
- `large-arc-flag`：角度大小，以 180 度为分界线，
  - 0 表示小角度弧，
  - 1 表示大角度弧。
- `sweep-flag`：弧线方向
  - 0 表示逆时针
  - 1 表示顺时针
- `x / y`：弧形的终点

`large-arc-flag` 和 `sweep-flag` 结合例子理解比较好：

![01.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/5e6e0bb335da4eacb2552d94f24f40a0~tplv-k3u1fbpfcp-watermark.image?)

```xml
<!-- 小角度弧 & 逆时针 -->
<path
  d="M80 80
      A 45 45, 0, 0, 0, 125 125
      L 125 80 Z"
  fill="green"
/>
<!-- 大角度弧 & 逆时针 -->
<path
  d="M230 80
      A 45 45, 0, 1, 0, 275 125
      L 275 80 Z"
  fill="red"
/>
<!-- 小角度弧 & 顺时针 -->
<path
  d="M80 230
      A 45 45, 0, 0, 1, 125 275
      L 125 230 Z"
  fill="purple"
/>
<!-- 大角度弧 & 顺时针 -->
<path
  d="M230 230
      A 45 45, 0, 1, 1, 275 275
      L 275 230 Z"
  fill="blue"
/>
```

## 填充和描边

### 不透明度

`fill-opacity` 和 `stroke-opacity` 控制填充和描边的不透明度。

当然，你也可以用 rgba 来指定透明度。

### 描边

和 canvas 中的线条样式类似，SVG 中也有 `stroke-width`、`stroke-linecap`、`stroke-linejoin`、`stroke-miterlimit`、`stroke-dasharray` 和 `stroke-dashoffset`。

### 使用 CSS

我们是可以通过 CSS 来定义样式的，只不过你要把 ` background-color``、border ` 改成 `fill` 和 `stroke`。

上色和填充的部分一般是可以用 CSS 来设置的，比如 `fill`，`stroke`，`stroke-dasharray` 等，但是不包括下面会提到的渐变和图案等功能。另外，`width`、`height`，以及路径的命令等等，都不能用 css 设置。

像这样的内联样式：

```xml
<rect x="10" height="180" y="10" width="180" style="stroke: black; fill: red;"/>
```

或者将样式放入 `<defs>` 元素中，可以重复使用同一组样式：

```xml
<svg width="200" height="200" xmlns="http://www.w3.org/2000/svg" version="1.1">
  <defs>
    <style type="text/css"><![CDATA[
       #MyRect {
         stroke: black;
         fill: red;
       }
    ]]></style>
  </defs>
  <rect x="10" height="180" y="10" width="180" id="MyRect"/>
</svg>
```

### 渐变

渐变的实现逻辑和 canvas 很相似，但 svg 是节点形式实现的：

![01.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/b708f303d01a4603821bb27161e50d27~tplv-k3u1fbpfcp-watermark.image?)

```xml
    <svg width="400" height="500" xmlns="http://www.w3.org/2000/svg">
      <defs>
        <linearGradient id="Gradient1" x1="0" x2="0" y1="0" y2="1">
          <stop offset="0%" stop-color="red" />
          <stop offset="50%" stop-color="black" stop-opacity="0" />
          <stop offset="100%" stop-color="blue" />
        </linearGradient>
        <radialGradient
          id="RadialGradient1"
          cx="0.5"
          cy="0.5"
          r="0.5"
          fx="0.25"
          fy="0.25"
        >
          <stop offset="0%" stop-color="red" />
          <stop offset="100%" stop-color="blue" />
        </radialGradient>
      </defs>
      <rect
        x="10"
        y="10"
        rx="15"
        ry="15"
        width="100"
        height="100"
        fill="url(#RadialGradient1)"
      />
      <rect
        x="120"
        y="10"
        rx="15"
        ry="15"
        width="100"
        height="100"
        fill="url(#Gradient1)"
      />
    </svg>
```

`linearGradient` 默认是水平方向的，通过设置起点 `(x1,y1)` 和终点 `(x2,y2)` 可以修改渐变方向。

`radialGradient` 的规则略微复杂一点，

- 中心点 `(cx,cy)` 和 半径 `r`，描述了渐变边缘位置
- 焦点 `(fx,fy)`，描述了渐变的中心

![01.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/14946c545bb94eb3be34002f2c747b13~tplv-k3u1fbpfcp-watermark.image?)

`<stop>` 节点通过指定位置的 `offset`（偏移）属性和 `stop-color`（颜色中值）属性来说明在渐变的特定位置上应该是什么颜色

#### 渐变方式 spreadMethod

![01.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/c62cffbcb6b04792bf2c75e2b353348e~tplv-k3u1fbpfcp-watermark.image?)

- `pad`：默认效果，当渐变到达终点时，最终的偏移颜色被用于填充对象剩下的空间
- `reflect`，以折返的方式重复渐变效果，也就是说以 100%偏移位置的颜色开始，逐渐偏移到 0%位置的颜色，然后再回到 100%偏移位置的颜色。
- `repeat`，和 `reflect` 的区别是跳回到最初的颜色然后继续渐变。

### 图案

跟渐变一样，`<pattern>` 需要放在 SVG 文档的 `<defs>` 内部。

在 pattern 元素内部你可以包含任何其它基本形状，比如绘制两个矩形和一个圆：

```xml
<svg width="200" height="200" xmlns="http://www.w3.org/2000/svg" version="1.1">
  <defs>
    <linearGradient id="Gradient1">
      <stop offset="5%" stop-color="white"/>
      <stop offset="95%" stop-color="blue"/>
    </linearGradient>
    <linearGradient id="Gradient2" x1="0" x2="0" y1="0" y2="1">
      <stop offset="5%" stop-color="red"/>
      <stop offset="95%" stop-color="orange"/>
    </linearGradient>

    <pattern id="Pattern" x="0" y="0" width=".25" height=".25">
      <rect x="0" y="0" width="50" height="50" fill="skyblue"/>
      <rect x="0" y="0" width="25" height="25" fill="url(#Gradient2)"/>
      <circle cx="25" cy="25" r="20" fill="url(#Gradient1)" fill-opacity="0.5"/>
    </pattern>

  </defs>

  <rect fill="url(#Pattern)" stroke="black" x="0" y="0" width="200" height="200"/>
</svg>
```

## 文本

### \<text>

`<text>` 元素用于定义文本。

```xml
<text x="10" y="10" text-anchor="start">Hello World!</text>
```

- `x/y`：决定了文本在视口中显示的位置
- `text-anchor`：文本的水平对齐位置，取值 `start`、`middle`、`end` 或 `inherit`
- `rotate`：把所有的字符旋转一个角度。如果是一个数列，则使每个字符旋转分别旋转到那个值，剩下的字符根据最后一个值旋转。

下列每个属性可以被设置为一个 SVG 属性或者成为一个 CSS 声明：`font-family`、`font-style`、`font-weight`、`font-variant`、`font-stretch`、`font-size`、`font-size-adjust`、`kerning`、`letter-spacing`、`word-spacing` 和 `text-decoration`。

### \<tspan>

用来标记大块文本的子部分，它必须是一个 text 元素或别的 tspan 元素的子元素。

```xml
<text x="10" y="20">
  Hello SVG! this is
  <tspan font-weight="bold">bold</tspan>
  and
  <tspan fill="red">red</tspan>
  .
</text>
```

### \<textPath>

根据 `<path>` 元素的形状来放置文字。利用 `xlink:href` 属性取得一个任意路径，把字符对齐到路径，于是字体会环绕路径、顺着路径走：

```xml
<defs>
  <path
    id="MyPath"
    d="M 100 200
             C 200 100 300   0 400 100
             C 500 200 600 300 700 200
             C 800 100 900 100 900 100"
  />
</defs>
<text font-family="Verdana" font-size="42.5">
  <textPath xlink:href="#MyPath">
    We go up, then we go down, then up again
  </textPath>
</text>
```

## Transform 基本变换

transform 属性是变换规则的集合，变换可以连缀，用空格隔开。

### \<g>

添加到 g 元素上的变换会应用到其所有的子元素上。实际上，这是它唯一的目的。

```xml
<g fill="red">
  <rect x="0" y="0" width="10" height="10" />
  <rect x="20" y="0" width="10" height="10" />
</g>
```

输出两个红色矩形。

### 平移

```
translate(<x> [<y>])
```

通过 x 向量和 y 向量移动元素，如果 y 向量没有被提供，那么默认为 0

```xml
<rect x="0" y="0" width="10" height="10" transform="translate(30,40)" />
```

矩形会移到点(30,40)。

### 旋转

```
rotate(<a> [<x> <y>])
```

通过一个给定角度对一个指定的点进行旋转变换。如果 x 和 y 没有提供，那么默认为当前元素坐标系原点。否则，就以(x,y)为原点进行旋转。

```xml
<rect x="20" y="20" width="20" height="20" transform="rotate(45)" />
```

一个方形，旋转了 45 度。

### 缩放

```
scale(<x> [<y>])
```

通过 x 和 y 指定一个比例放大缩小操作，如果 y 没有被提供，那么假定为等同于 x。

```xml
<!-- uniform scale -->
<circle cx="0" cy="0" r="10" fill="red" transform="scale(4)" />

<!-- vertical scale -->
<circle cx="0" cy="0" r="10" fill="yellow" transform="scale(1,4)" />

<!-- horizontal scale -->
<circle cx="0" cy="0" r="10" fill="pink" transform="scale(4,1)" />
```

### 斜切

指定了沿 x 轴倾斜 a° 的倾斜变换。

```
skewX(<a>)
```

指定了沿 y 轴倾斜 a° 的倾斜变换。

```
skewY(<a>)
```

利用一个矩形制作一个斜菱形。

```xml
<rect x="-3" y="-3" width="6" height="6" fill="red" transform="skewX(30)" />
```

## 剪切和遮罩

**剪切** 用来移除在别处定义的元素的部分内容，它只能要么显示要么不显示（没有半透明效果）。

**遮罩** 允许使用透明度和灰度值遮罩计算得的软边缘。

### 裁剪

通过 `<clipPath>` 定义一条剪切路径（作为可见范围，样式无效），将其作为需要裁剪的元素的 `clip-path` 属性值，如果图形超出了当前剪切路径所包围的区域，那么超出部分将不会绘制。

比如，我们裁剪一个半圆：

```xml
<defs>
  <clipPath id="cut-off-bottom">
    <rect x="0" y="0" width="200" height="100" />
  </clipPath>
</defs>

<circle cx="100" cy="100" r="100" clip-path="url(#cut-off-bottom)" />
```

### 遮罩

`<mask>` 元素用于定义这样的遮罩元素，透明遮罩层可以是任何其他图形对象或者 `<g>` 元素。

比如，用遮罩实现元素的淡出效果：

```xml
<svg
  version="1.1"
  xmlns="http://www.w3.org/2000/svg"
  xmlns:xlink="http://www.w3.org/1999/xlink"
>
  <defs>
    <linearGradient id="Gradient">
      <stop offset="0" stop-color="white" stop-opacity="0" />
      <stop offset="1" stop-color="white" stop-opacity="1" />
    </linearGradient>
    <mask id="Mask">
      <rect x="0" y="0" width="200" height="200" fill="url(#Gradient)" />
    </mask>
  </defs>

  <rect x="0" y="0" width="200" height="200" fill="red" mask="url(#Mask)" />
</svg>
```

![01.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/d843a1823f0645e58d2ffd9a4da427a3~tplv-k3u1fbpfcp-watermark.image?)
