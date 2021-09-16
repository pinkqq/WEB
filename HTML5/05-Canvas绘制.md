`<canvas>` 元素可被用来通过 JavaScript（ **Canvas API** 或 **WebGL API** ）绘制图形及图形动画，**也就是说 `<canvas>` 标签只是一个图形容器**。Canvas 可以用于动画、游戏画面、数据可视化、图片编辑以及实时视频处理等方面。

Canvas API 主要聚焦于 2D 图形。而 WebGL API 则用于绘制硬件加速的 2D 和 3D 图形。

## \<canvas>

```html
<canvas id="tutorial" width="150" height="150"></canvas>
```

`<canvas>` 标签只有两个属性—— `width` 和 `height`，当没有设置宽度和高度的时候，`<canvas>` 会初始化宽度为 `300px` 和高度为 `150px`。

**需要注意的是，** 通过 CSS 也可以定义 canvas 的尺寸，但此元素尺寸非彼画布尺寸，在绘制时图像会伸缩以适应它的画布尺寸；如果元素尺寸和画布尺寸比例不一样，绘制出来的图像是扭曲的。

> 只有同时通过 CSS 指定了 `width` 和 `height`，才会出现比例不一致，如果只定义 `width: 400px`，你会发现高度会自动变成 `200px`。

比如，尺寸比例不一致时会出现下面这个变形的圆：

```css
#myCanvas {
  width: 400px;
  height: 150px;
}
```

![01.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/ac680fba27654cf5891d1e871958150e~tplv-k3u1fbpfcp-watermark.image)

此时的画布尺寸依旧是初始值（`w:300px`, `h:150px`）。

## getContext()

`<canvas>` 元素创造了一个固定大小的画布，它公开了一个或多个**渲染上下文**，其可以用来绘制和处理要展示的内容。

canvas 起初是空白的，脚本首先需要找到渲染上下文，然后在它的上面绘制。`getContext()`，这个方法是用来获得渲染上下文和它的绘画功能。

```js
let ctx = canvas.getContext(contextType, contextAttributes?);
```

- **上下文类型（contextType）**
  - `2d`：创建一个二维渲染上下文
  - `webgl`：创建一个三维渲染上下文（WebGL 版本 1）
  - `webgl2`：创建一个三维渲染上下文（WebGL 版本 2）
  - `bitmaprenderer`：创建一个只提供将 canvas 内容替换为指定 ImageBitmap 功能的 ImageBitmapRenderingContext。（safari 还不支持）
- **上下文属性（contextAttributes）**

  - `2d`
    - `alpha`：布尔值，表明 `canvas` 包含一个 `alpha` 通道。如果设置为 `false`, 浏览器将认为 canvas 背景总是不透明的, 这样可以加速绘制透明的内容和图片.
  - `webgl`

    - `alpha`: 布尔值，表明 `canvas` 包含一个 `alpha` 通道。

    - `antialias`: 布尔值，表明是否开启抗锯齿。

    - `depth`: 布尔值，表明绘制缓冲区包含一个深度至少为 16 位的缓冲区。

    - `failIfMajorPerformanceCaveat`: 布尔值，表明在一个系统性能低的环境是否创建该上下文。

    - `powerPreference`: 指示浏览器在运行 WebGL 上下文时使用相应的 GPU 电源配置。 可能值如下:

      - `default`: 自动选择，默认值。

      - `high-performance`: 高性能模式。

      - `low-power`: 节能模式。

    - `premultipliedAlpha`: 布尔值，表明排版引擎将假设绘制缓冲区包含预混合 alpha 通道。

    - `preserveDrawingBuffer`: 布尔值，是否保存缓冲区，直到被清除或被使用者覆盖。

    - `stencil`: 布尔值，表明绘制缓冲区包含一个深度至少为 8 位的模版缓冲区。

## 一个简单的例子

![01.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/02eff0a206b0406b828da9a231b8ff93~tplv-k3u1fbpfcp-watermark.image)

在例子中绘制了两个有趣的长方形，其中的一个有着 alpha 透明度。

```html
<body>
  <canvas id="canvas" width="150" height="150"></canvas>
  <script>
    var canvas = document.getElementById("canvas"); // 得到DOM对象
    if (canvas.getContext) {
      var ctx = canvas.getContext("2d"); // 得到渲染上下文

      // 绘制第一个长方形
      ctx.fillStyle = "rgb(200,0,0)";
      ctx.fillRect(10, 10, 55, 50);

      // 绘制第二个长方形
      ctx.fillStyle = "rgba(0, 0, 200, 0.5)";
      ctx.fillRect(30, 30, 55, 50);
    }
  </script>
</body>
```

## 坐标

在我们开始画图之前，我们先了解一下 canvas 的坐标体系（x,y）。canvas 是一个二维网格，原点 (0,0) 在左上角，所有元素的位置都相对于原点定位取正数。所以图中蓝色方形左上角的坐标为距离左边（X 轴）x 像素，距离上边（Y 轴）y 像素，它的坐标就是 (x,y)：

![01.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/03fc04b833554532b29818dfe515a185~tplv-k3u1fbpfcp-watermark.image)

## 绘制图形

`<canvas>` 只支持两种形式的图形绘制：**矩形** 和 **路径**（由一系列点连成的线段）。

### 矩形

`<canvas>` 提供了三种方法绘制矩形，矩形的左上角为(x,y)，：

#### 1. fillReact

绘制一个填充的矩形，填充色为当前的 `fillStyle`。

```js
fillRect(x, y, width, height);
```

我们可以用 `fillRect()` 在开始绘制内容前，设置一个背景：

```js
const canvas = document.getElementById("canvas");
const ctx = canvas.getContext("2d");
ctx.fillRect(0, 0, canvas.width, canvas.height);
```

#### 2. strokeReact

绘制一个矩形的边框，边框色为当前的 `strokeStyle`。

```js
strokeRect(x, y, width, height);
```

#### 3. clearReact

这个方法通过把像素设置为透明，以达到擦除一个矩形区域的效果。

```js
clearRect(x, y, width, height);
```

> 请确保在调用 `clearRect()` 之后绘制新内容前调用 `beginPath()` 。

我们可以用 `clearRect` 清除整个画布：

```js
const canvas = document.getElementById("canvas");
const ctx = canvas.getContext("2d");
ctx.clearRect(0, 0, canvas.width, canvas.height);
```

### 路径

路径就像是画家手中的画笔，通过一笔一画的组合（路径列表）可以绘制出复杂而丰富的图形。

使用路径绘制图形的步骤：

- **第一步**，创建路径起始点，`beginPath()`（清空路径列表）。
- **第二步**，画出路径。
- **第三步（可选）**，闭合路径（路径生成），`closePath()`（绘制一条从当前点到开始点的直线来闭合图形）。
- **第四步**，通过描边或填充路径区域来渲染图形，`stroke()` / `fill()`。
  - 调用 `fill()`，所有没有闭合的形状都会自动闭合。
  - 调用 `stroke()`，不会自动闭合。

#### 移动笔触 moveTo

```js
moveTo(x, y); // 将笔触移动到坐标 (x,y) 上
```

这个方法完成的事情，可以想象成你在纸上作业，一支钢笔或者铅笔的笔尖从一个点到另一个点的移动过程。

我们可以用 `moveTo` 函数：

- 设置起点
- 绘制一些不连续的路径

#### 线 lineTo

```js
lineTo(x, y);
```

这个方法是用来 **绘制直线** 的，绘制一条从当前位置到指定坐标 (x,y) 的直线。

当前位置由 **之前路径的结束点** 或者 **moveTo()** 决定。

我们结合 `moveTo()`，来绘制两个三角形，填充的和描边的。

```js
// 填充三角形
ctx.beginPath();
ctx.moveTo(25, 25);
ctx.lineTo(105, 25);
ctx.lineTo(25, 105);
ctx.fill(); // 自动闭合路径

// 描边三角形
ctx.beginPath();
ctx.moveTo(125, 25);
ctx.lineTo(125, 105);
ctx.lineTo(205, 25);
ctx.closePath(); // 手动闭合路径
ctx.stroke();
```

![01.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/8d70aad9b6024152a2e3983f9d978b6d~tplv-k3u1fbpfcp-watermark.image)

#### 圆弧

绘制圆弧或者圆，可以选择 `arc()` 或者 `arcTo()`。两个方法的绘制思路是不一样的，我们通常选择 `arc()`，因为更可控。

##### arc

```js
arc(x, y, radius, startAngle, endAngle, anticlockwise);
```

绘制的过程：

- 以 `(x,y)` 为圆心
- 以 `radius` 为半径
- 从 `startAngle` 开始到 `endAngle` 结束
- 按照给定的方向（ `anticlockwise`，默认为 `false` 顺时针）来生成。

> 注意，`startAngle` 和 `endAngle` 的单位是弧度，而不是角度。
> **弧度 = ( Math.PI / 180 ) \* 角度**

我们来画个“月有阴晴月缺”：

```js
for (i = 0; i < 12; i++) {
  ctx.beginPath();
  let x = 25 + i * 50;
  let y = 25;
  let radius = 20;
  let startAngle = 0;
  let endAngle = Math.PI + (Math.PI * i) / 12; // 从半圆到全圆

  ctx.arc(x, y, radius, startAngle, endAngle);
  ctx.fill();
}
```

![01.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/2d7b8e4c449f49618ec4ff82b31833e2~tplv-k3u1fbpfcp-watermark.image)

##### arcTo

```js
arcTo(x1, y1, x2, y2, radius);
```

绘制过程：

- 连接当前位置（蓝点）和控制点 1（x1,y1）得到直线 A
- 连接控制点 1（x1,y1）和控制点 2（x2,y2）得到直线 B
- 以 radius 做为半径
- 以直线 A 和直线 B 作为切线，画出两条切线之间的弧线路径

![01.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/3e9ef929cd3a4ca9afa1acdaed8a7ab1~tplv-k3u1fbpfcp-watermark.image)

#### 矩形 rect

```js
rect(x, y, width, height);
```

绘制一个左上角坐标为 `(x,y)`，宽高为 `width` 以及 `height` 的矩形。

在该方法执行前，当前笔触自动重置回默认坐标，即 `moveTo(0,0)`；该方法结束后的笔触停留在矩形左上角 `(x,y)`。

#### 贝塞尔曲线

二次及三次贝塞尔曲线，一般用来绘制复杂有规律的图形。

因为贝塞尔曲线构建的图形很难想象，所以分享一个在线调试工具：

- [二次贝塞尔曲线调试器](http://blogs.sitepointstatic.com/examples/tech/canvas-curves/quadratic-curve.html)
- [三次贝塞尔曲线调试器](http://blogs.sitepointstatic.com/examples/tech/canvas-curves/bezier-curve.html)

![01.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/44e0b3c87c2e4c2787ed26edde4c1f10~tplv-k3u1fbpfcp-watermark.image)

##### 二次贝塞尔曲线

`quadraticCurveTo(cp1x, cp1y, x, y)`

绘制二次贝塞尔曲线，当前位置为开始点（蓝点），`(cp1x,cp1y)` 为一个控制点（红点），`(x,y)` 为结束点（蓝点）。

渲染对话气泡：

```js
ctx.beginPath();
ctx.moveTo(75, 25);
ctx.quadraticCurveTo(25, 25, 25, 62.5);
ctx.quadraticCurveTo(25, 100, 50, 100);
ctx.quadraticCurveTo(50, 120, 30, 125);
ctx.quadraticCurveTo(60, 120, 65, 100);
ctx.quadraticCurveTo(125, 100, 125, 62.5);
ctx.quadraticCurveTo(125, 25, 75, 25);
ctx.stroke();
```

![01.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/5fd72989dfbd4f12b3616f3f069176a5~tplv-k3u1fbpfcp-watermark.image)

##### 三次贝塞尔曲线

`bezierCurveTo(cp1x, cp1y, cp2x, cp2y, x, y)`

绘制三次贝塞尔曲线，当前位置为开始点（蓝点，）`(cp1x,cp1y)` 为控制点 1（红点），`(cp2x,cp2y)` 为控制点 2（红点），`(x,y)` 为结束点（蓝点）。

绘制一颗心心：

```js
ctx.beginPath();
ctx.moveTo(75, 40);
ctx.bezierCurveTo(75, 37, 70, 25, 50, 25);
ctx.bezierCurveTo(20, 25, 20, 62.5, 20, 62.5);
ctx.bezierCurveTo(20, 80, 40, 102, 75, 120);
ctx.bezierCurveTo(110, 102, 130, 80, 130, 62.5);
ctx.bezierCurveTo(130, 62.5, 130, 25, 100, 25);
ctx.bezierCurveTo(85, 25, 75, 37, 75, 40);
ctx.fill();
```

![01.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/91a33cdca63740d6a3ce7ba8ad8b574c~tplv-k3u1fbpfcp-watermark.image)

## Path2D

`Path2D` 可以用缓存或记录绘画命令，就像是把某一段路径抽成一个函数用于复用，这样做也简化了代码和提高了性能。

以上介绍的所有路径方法，在 `Path2D` 中都可以用，比如 `moveTo`, `rect`, `arc` 或 `quadraticCurveTo` 等。

### Path2D()

返回一个新初始化的 Path2D 对象，可能是某一个路径，或者包含 SVG path 数据的字符串。

```js
new Path2D(); // 空的Path对象
new Path2D(path); // 克隆Path对象
new Path2D(d); // 从SVG建立Path对象
```

#### 创建和拷贝路径

```js
var path1 = new Path2D();
path1.rect(10, 10, 100, 100);

var path2 = new Path2D(path1);
path2.moveTo(220, 60);
path2.arc(170, 60, 50, 0, 2 * Math.PI);

ctx.stroke(path2);
```

#### SVG path

Path2D API 有另一个强大的功能，可以使用 SVG path data 来初始化 canvas 上的路径，这意味着可以将 SVG 搬上 canvas。

解读一下这一条 SVG path -- `"M10 10 h 80 v 80 h -80 Z"`：

- 移动到点 (M10 10)
- 向右侧水平移动 80 个点 (h 80)
- 向下 80 个点 (v 80)
- 向左 80 个点 (h -80)
- 回到起始点 (z)

没错，最终绘制除了一个矩形：

```js
var p = new Path2D("M10 10 h 80 v 80 h -80 Z");
ctx.fill(p);
```

### addPath

```js
Path2D.addPath(path[, transform])
```

`addPath` 可以将 path 结合起来（添加一条路径到当前路径），这很适用从几个元素中来创建对象。

## 给图形上色

我们想为图形添加样式和颜色，可以用上这两个属性：

- **`fillStyle`**：设置图形的填充颜色。
- **`strokeStyle`**：设置图形轮廓的颜色。

它俩儿的默认值都是黑色（`#000000`），接受色值、渐变对象和图案对象。

### 色值

关于色值的内容比较简单，和我们在 CSS 中指定颜色也差不多：

```js
// 色值
ctx.fillStyle = "orange";
ctx.fillStyle = "#FFA500";
ctx.fillStyle = "rgb(255,165,0)";
ctx.fillStyle = "rgba(255,165,0,0.2)"; // 透明度 0.2
```

在 canvas 中，渐变对象和图案对象是有自己套路的。

### 渐变对象

也是用 **线性** 或者 **径向** 的两种渐变形式来新建一个 `canvasGradient` 对象，并且赋给图形的 `fillStyle` 或 `strokeStyle` 属性。

- `createLinearGradient(x1, y1, x2, y2)`：渐变的起点 (x1,y1) ，终点 (x2,y2)
- `createRadialGradient(x1, y1, r1, x2, y2, r2)`：定义了两个圆
  - 一个以 (x1,y1) 为原点，半径为 r1 的圆
  - 一个以 (x2,y2) 为原点，半径为 r2 的圆

创建出 `canvasGradient` 对象后，用 `addColorStop` 方法给它上色：

```js
gradient.addColorStop(position, color);
```

- `position`：必须是一个 0.0 与 1.0 之间的数值，表示渐变中颜色所在的相对位置。
- `color`：是一个有效的 CSS 颜色值。

一个简单的线性黑白渐变例子：

```js
let linearGradient = ctx.createLinearGradient(0, 0, 150, 150);
linearGradient.addColorStop(0, "#000");
linearGradient.addColorStop(1, "#fff");
```

![01.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/09ac87ed139f4e18bb8d0c38a8d180d9~tplv-k3u1fbpfcp-watermark.image)

一个径向渐变小彩球的例子：

```js
// 创建渐变
var ball = ctx.createRadialGradient(45, 45, 10, 52, 50, 30);
ball.addColorStop(0, "#A7D30C");
ball.addColorStop(0.9, "#019F62");
ball.addColorStop(1, "rgba(1,159,98,0)");

// 画图形
ctx.fillStyle = ball;
ctx.fillRect(0, 0, 150, 150);
```

让起点稍微偏离终点，这样可以达到一种球状 3D 效果。在球体的边缘处，使用同色值透明度过渡会柔和一些。

![01.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/7702a61e82084f1b9c036861ed727e61~tplv-k3u1fbpfcp-watermark.image)

### 图案对象

图案的应用跟渐变很类似的，创建出一个 `pattern` 之后，赋给 `fillStyle` 或 `strokeStyle` 属性即可。

```js
createPattern(image, type);
```

- `image`：一个 Image 对象的引用，或者另一个 canvas 对象。
- `type`：`repeat`，`repeat-x`，`repeat-y` 和 `no-repeat`。

> **注意：** 与 `drawImage` 有点不同，你需要确认 `image` 对象已经装载完毕，否则图案可能效果不对的。

```js
// 创建新 image 对象，用作图案
let img = new Image();
img.src = "someimage.png";

// 确认 image 对象加载完毕
img.onload = function () {
  // 创建图案
  let ptrn = ctx.createPattern(img, "repeat");
  ctx.fillStyle = ptrn;
  ctx.fillRect(0, 0, 150, 150);
};
```

## 线的样式

### lineWidth

设置当前绘线的粗细。默认值是 1.0，必须为正数。

绘制十条宽度从 1.0 到 10.0 的线：

```js
for (let i = 0; i < 10; i++) {
  ctx.beginPath();
  ctx.lineWidth = i + 1;
  ctx.moveTo(5 + i * 14, 5);
  ctx.lineTo(5 + i * 14, 140);
  ctx.stroke();
}
```

![01.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/43dcc7b7a2d54527a1466af3ea081a81~tplv-k3u1fbpfcp-watermark.image)

因为线宽是指给定路径的中心到两边的粗细，所以在绘制过程中会在路径的两边各绘制线宽的一半。也正是因为这个规则，**当我们的路径 Y 轴落在网格上时（像下图 2）**，两边的实际填充区域只占网格的一半，实际渲染中 canvas 会以实际笔触颜色一半色调的颜色来填充整个区域，所以，最左边的以及所有宽度为奇数的线并不能精确呈现。

所以在默认情况下，我们想要获得精确的线条，就要**让奇数宽度的中心落在网格中心，偶数宽度的中心落在网格线上**。在操作缩放时，这也是不可忽略的一点。

![01.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/9bbc28502aa14d46bcd7f12a4b68e04f~tplv-k3u1fbpfcp-watermark.image)

### lineCap

决定了线段端点显示的样子。它的取值如下图所示，从左到右分别是 `butt`（默认）、`round`、`square`。

![01.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/8101e1c98dad4abe96d0b1cec5613bdd~tplv-k3u1fbpfcp-watermark.image)

### lineJoin

决定了图形中两线段连接处所显示的样子。它的取值如下图所示，从下往上分别是 `miter`（默认）、`bevel`、`round`。其中 `miter` 的延伸效果会收到 `miterLimit` 的约束，也就是下面这个属性。

![01.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/afe975fa514143de9e642d8cac1c3b5c~tplv-k3u1fbpfcp-watermark.image)

### miterLimit

想象上图中的两线之间的夹角无限变小，那么外延交点也会无限变远。

`miterLimit` 属性就是用来设定外延交点与连接点的最大距离（默认为 10.0），如果交点距离大于此值，连接效果会变成了 `bevel`。

```
max miterLength（最长交点距离）= miterLimit * lineWidth
```

- `miterLimit` 默认为 `10.0`，这将会去除所有小于大约 `11` 度的斜接。
- `miterLimit` 为 `√2 ≈ 1.4142136 `（四舍五入）时，将去除所有锐角的斜接，仅保留钝角或直角。
- `1.0` 是合法的 `miterLimit` 值，但这会去除所有斜接。
- 小于 `1.0 `的值不是合法的 `miterLimit` 值。

### 虚线

#### setLineDash

在填充线时使用虚线模式。

```js
setLineDash(segments);
```

- `segments`，一个数组，描述线段和间距。

> 提示：如果要切换回至实线模式，将 segments 设置为一个空数组即可。

一些常见的虚线模式：

```js
function drawDashedLine(pattern) {
  ctx.beginPath();
  ctx.setLineDash(pattern);
  ctx.moveTo(0, y);
  ctx.lineTo(300, y);
  ctx.stroke();
  y += 20;
}

const canvas = document.getElementById("canvas");
const ctx = canvas.getContext("2d");
let y = 15;

drawDashedLine([]); // 实线
drawDashedLine([1, 1]);
drawDashedLine([10, 10]);
drawDashedLine([20, 5]);
drawDashedLine([15, 3, 3, 3]);
drawDashedLine([20, 3, 3, 3, 3, 3, 3, 3]);
drawDashedLine([12, 3, 3]); // Equals [12, 3, 3, 12, 3, 3]
```

![01.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/28903b855de940d6833ec2be5c26ffe7~tplv-k3u1fbpfcp-watermark.image)

#### lineDashOffset

`lineDashOffset` 可以设置虚线的偏移量，默认值为 0.0。

```js
ctx.lineDashOffset = value;
```

利用这个偏移量，我们可以让虚线“动起来”，俗称“蚂蚁线”。

```js
let offset = 0;

function draw() {
  ctx.clearRect(0, 0, canvas.width, canvas.height);
  ctx.setLineDash([4, 4, 12, 4]);
  ctx.lineDashOffset = offset;
  ctx.strokeRect(20, 20, 150, 150);
}

function march() {
  offset++;
  if (offset > 24) {
    offset = 0;
  }
  draw();
  setTimeout(march, 20);
}

march();
```

![01.gif](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/d73fe2055fd14123a92386686382b347~tplv-k3u1fbpfcp-watermark.image)

#### getLineDash()

`getLineDash()`，是获取当前线段样式的方法。返回值是一个描述线段和间隔的数组。如果数组元素的数量是奇数，数组元素会被复制并重复。

```js
ctx.setLineDash([5, 15]);
console.log(ctx.getLineDash()); // [5, 15]
ctx.setLineDash([5, 15, 5]); // 数组元素的数量是奇数
console.log(ctx.getLineDash()); // [5, 15, 5, 5, 15, 5]
```

## 阴影

- **`shadowOffsetX|Y`**
  用来设定阴影在 X 和 Y 轴的延伸距离，它们是不受变换矩阵所影响的。
  - 负值表示阴影会往上或左延伸，
  - 正值则表示会往下或右延伸，
  - 默认为 0。
- **`shadowBlur`**
  用于设定阴影的模糊程度，默认为 0。
- **`shadowColor`**
  用于设定阴影颜色效果，默认是全透明的黑色。

```js
ctx.shadowOffsetX = 10;
ctx.shadowOffsetY = 10;
ctx.shadowBlur = 10;
ctx.shadowColor = "rgba(23,23,23,0.5)";
ctx.fillRect(20, 20, 150, 150);
```

![01.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/cdd26899fb6c4007a271222b7baf5b33~tplv-k3u1fbpfcp-watermark.image)

## 填充规则

在 canvas 中，可以通过给 `fill`（或者 `clip` 和 `isPointinPath` ）传参指定填充规则，默认为 `nonzero`：

```js
ctx.fill();
ctx.fill(fillRule);
ctx.fill(path, fillRule);
```

那么，什么是填充规则呢？

只要是路径填充，都有两种规则 -- `nonzero` 和 `evenodd`，无论是 SVG 中的路径填充，还是 Canvas 中的路径填充。

两组路径的差异体现在交叉点的处理上，也就是说如果只是一个三角形，是看不出区别的：

![01.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/0ee9862813b84758b23704f506c76279~tplv-k3u1fbpfcp-watermark.image)

如果是两个发生重叠的三角形，产生了交叉点，也产生了差异：

![02.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/fa0f78cb58cf4fcbb2a8819a2a3d61ff~tplv-k3u1fbpfcp-watermark.image)

### evenodd

“奇偶规则” -- 计算交叉路径数量，计算结果是奇数，就填充；计算结果是偶数，就不填充。

比如我们在下图取一点 A，向任意方向发出一条射线（天蓝色），找到和射线发生交叉的路径，是路径 5 和路径 2，计算结果是偶数，所以不填充。

![01.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/06a5d2f222e749a799bd90c6ecbbe99f~tplv-k3u1fbpfcp-watermark.image)

### nonzero

“非零规则” -- 计算顺时针逆时针数量，计算结果不是 0，就填充；计算结果是 0，就不填充。

比如我们在下图取一点 B，向任意方向发出一条射线（天蓝色），找到那些和射线发生交叉的路径：

- 发生逆时针的偏转（路径 3），记 -1
- 发生顺时针偏转（路径 2），记 +1

计算结果是 0，所以不填充。

![02.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/6abc803dc6fe4d9ca0478f1f20c4d649~tplv-k3u1fbpfcp-watermark.image)

## 绘制文本

### 绘制

canvas 提供了两种方法来渲染文本：

- `fillText(text, x, y [, maxWidth])`
  在指定的 `(x,y)` 位置填充指定的文本，绘制的最大宽度是可选的。

- `strokeText(text, x, y [, maxWidth])`
  在指定的 `(x,y)` 位置绘制文本边框，绘制的最大宽度是可选的。

### 样式

#### font

描述当前字体样式，取值和 CSS 中的 `font` 属性一样，默认字体是 `10px sans-serif`。

```js
ctx.font = "bold 48px serif";
```

#### textAlign

描述了文本水平方向的对齐方式，默认值是 `start`，取值如下：

```js
ctx.textAlign = "left" || "right" || "center" || "start" || "end";
```

以下 `x` 和 `y`，是在 `fillText`/`strokeText` 的时候所给的。

- `"left"`，文本左对齐。
  ![01.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/82f037865814404393417986f4257db0~tplv-k3u1fbpfcp-watermark.image)

- `"right"`，文本右对齐。
  ![01.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/74fa84d7fd9641ae910da102220aceba~tplv-k3u1fbpfcp-watermark.image)
- `"center"`，文本居中对齐。
  ![01.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/41c24b76e18644b58b7d33a546a91c43~tplv-k3u1fbpfcp-watermark.image)
- `"start"`，文本对齐界线开始的地方。
- `"end"`，文本对齐界线结束的地方。

**`direction` 属性会对此属性产生影响：**

- 如果 `direction = ltr`，则 left 和 start 的效果相同，right 和 end 的效果相同；
- 如果 `direction = rtl`，则 left 和 end 的效果相同，right 和 start 的效果相同

#### textBaseline

描述了当前文本基线，即文字垂直方向的对齐方式，默认值是 `alphabetic`，取值如下：

- `top`，文本基线在文本块的顶部。
- `hanging`，文本基线是悬挂基线。
- `middle`，文本基线在文本块的中间。
- `alphabetic`，文本基线是标准的字母基线。
- `ideographic`，文字基线是表意字基线；如果字符本身超出了 alphabetic 基线，那么 ideograhpic 基线位置在字符本身的底部。
- `bottom`，文本基线在文本块的底部。

![01.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/fbf36cafbccd42e5a1b8d49776d9ceb3~tplv-k3u1fbpfcp-watermark.image)

#### direction

描述了当前文本方向。

```js
ctx.direction = "ltr" || "rtl" || "inherit";
```

- `ltr`，文本方向从左向右，如下图第一行。
- `rtl`，文本方向从右向左，如下图第二行。

![01.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/2664a0b53e3444188a81f5ba95b9ee26~tplv-k3u1fbpfcp-watermark.image)

### 预测量文本宽度

`measureText` 方法将返回一个 [TextMetrics](https://developer.mozilla.org/zh-CN/docs/Web/API/TextMetrics) 对象，只包含部分能体现文本特性的属性（宽度、所在像素）：

```
ctx.measureText(text);
```

```js
  var text = ctx.measureText("foo"); // TextMetrics object
}
```

![01.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/7ff558229f214c42af90e4ccde19f208~tplv-k3u1fbpfcp-watermark.image)

## 绘制图像

引入图像到 canvas 的步骤只需要两步：

- 获取图片资源
- 画上去

### 获取图片资源

canvas 支持的图片源类型：

- **HTMLImageElement**：

  - **`new Image()`**：
    ```js
    let img = new Image();
    img.onload = function () {
      // drawImage to canvas
    };
    img.src = "XXX.png";
    ```
  - **`<img>`**：
    ```html
    <img id="img" src="XXX.png" />
    <script>
      let img = document.getElementById("img");
      img.onload = function () {
        // drawImage to canvas
      };
    </script>
    ```

- **HTMLVideoElement**：`<video>`
  canvas 会绘制当前视频帧，我们可以搭配 video 事件使用，比如下面的例子：

  ```js
  let video = document.getElementById("video");

  // 媒体的第一帧加载完成时，渲染第一帧
  video.onloadeddata = function () {};

  // 播放暂停时，渲染当前帧
  video.onpause = function () {};
  ```

  ![01 (1).gif](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/3c4a9596fe7d409a8eda8159993d824e~tplv-k3u1fbpfcp-watermark.image?)

- **HTMLCanvasElement**：`<canvas>`
  一个常用的应用就是将第二个 canvas 作为第一个 canvas 的缩略图：

  ```js
  let sourceCanvas = document.getElementById("source");
  let thumbCanvas = document.getElementById("thumb");
  thumbCtx.drawImage(source, 0, 0, 100, 50);
  ```

  ![01.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/92451efbd9644018aa88ea6faf008e5b~tplv-k3u1fbpfcp-watermark.image?)

- **ImageBitmap**：高性能的位图，可以低延迟地绘制，它运用 `createImageBitmap()` 工厂方法模式，它可以从多种源中生成。

### drawImage()

获得图片对象后，我们通过 `drawImage` 方法将它渲染到 canvas 里。`drawImage` 参数有三种情况：三个参数，五个参数，九个参数。

#### 基础用法（三个参数）

```js
drawImage(image, x, y);
```

- `image`：图片对象
- `x,y`：在 canvas 中的起始坐标

#### 缩放（五个参数）

```js
drawImage(image, x, y, width, height);
```

- `width,height`：图片对象在 canvas 中的绘制尺寸

#### 切片（九个参数）

```js
drawImage(image, sx, sy, sWidth, sHeight, dx, dy, dWidth, dHeight);
```

![01.jpeg](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/9115b8783a7045feb72897529ae23fb2~tplv-k3u1fbpfcp-watermark.image?)

## 状态的保存和恢复

`save()` 和 `restore()`，这两个方法是简单绘制和复杂绘制的一条分界线，可以帮我们省下不少的重复劳动。

- `save()`：保存画布的所有状态
- `restore()`：恢复保存的画布状态

画布的状态被保存在一个栈中，一个绘画状态包括：

- 以及下面这些属性：
  - 描边/填充样式：`strokeStyle`, `fillStyle`, `globalAlpha`
  - 线的样式：`lineWidth`, `lineCap`, `lineJoin`, `miterLimit`, `lineDashOffset`
  - 阴影：`shadowOffsetX`, `shadowOffsetY`, `shadowBlur`, `shadowColor`,
  - 字体样式：`font`, `textAlign`, `textBaseline`, `direction`
  - 平滑质量：`imageSmoothingEnabled`
  - 合成属性：`globalCompositeOperation`
- 当前变形
- 当前裁剪路径

## 变形

变形是一种更强大的方法，它可以移动原点、对网格进行旋转和缩放。

### 移动 translate

移动 canvas 和它的原点到一个不同的位置。

```js
translate(x, y);
```

x 是左右偏移量，y 是上下偏移量，如下图所示：

![01.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/5a9d72e0ac7448c8bd884296bfb5c21e~tplv-k3u1fbpfcp-watermark.image?)

### 旋转 rotate

**以原点为中心**旋转 canvas。

```js
rotate(angle);
```

angle 取正时，顺时针旋转；angle 取负时，逆时针旋转。以弧度为单位的值。

![01.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/f24e226bcc394ae28410fb112a1914e9~tplv-k3u1fbpfcp-watermark.image?)

### 缩放 scale

对形状，位图进行缩小或者放大，即增减图形在 canvas 中的像素数目。

```js
scale(x, y);
```

- 默认值为 1， 为实际大小。
- 比 1 小，会缩小图形，
- 比 1 大，会放大图形。
- 如果是负数，相当于以 x 或 y 轴作为对称轴镜像反转

```js
// 被拉伸的矩形
ctx.save();
ctx.scale(10, 3);
ctx.fillRect(1, 10, 10, 10);
ctx.restore();

// 以 y 轴为对称轴做镜像反转
ctx.scale(-1, 1);
ctx.font = "48px serif";
ctx.fillText("荷包蛋卷", -193, 120);
```

![01.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/1052b4e6aa2148aba6d1ba96e1264637~tplv-k3u1fbpfcp-watermark.image?)

### 变形矩阵 transform

对变形矩阵直接修改。

- **基础用法**

  ```js
  transform(a, b, c, d, e, f);
  ```

  - `a`：水平方向的缩放
  - `b`：竖直方向的倾斜偏移
  - `c`：水平方向的倾斜偏移
  - `d`：竖直方向的缩放
  - `e`：水平方向的移动
  - `f`：竖直方向的移动

- **重置矩阵**
  重置当前变形为单位矩阵，等同于 `ctx.setTransform(1, 0, 0, 1, 0, 0); `
  ```js
  resetTransform();
  ```
- **重置并修改矩阵**
  将当前的变形矩阵重置为单位矩阵，然后用相同的参数调用 transform 方法。
  ```js
  setTransform(a, b, c, d, e, f);
  ```

## 组合

`globalCompositeOperation` 提供了 12 种合成图形的方式：

| 名称               | 描述                                                                        | 展示                                                                                                                           |
| ------------------ | --------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------ |
| `source-over`      | 默认设置，直接在原有内容上绘制新内容                                        | ![01.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/e7e61f3c77ca4f18b7db40aff28db09a~tplv-k3u1fbpfcp-watermark.image?) |
| `source-in`        | 新内容只在新内容和原有内容**重叠**的区域绘制，**其他都是透明的**。          | ![02.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/6c88fe95f9f9452a90c118f03a0a9617~tplv-k3u1fbpfcp-watermark.image?) |
| `source-out`       | 新内容在新内容和原有内容**不重叠**的区域绘制，其他都是透明的。              | ![03.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/64b07d8099ca4779a3da9b93a05eb3b2~tplv-k3u1fbpfcp-watermark.image?) |
| `source-atop`      | 新内容只在新内容和原有内容**重叠**的区域绘制， **其他都是保持原有内容的**。 | ![04.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/f85cfb9e54d640ee9be2672df75628d7~tplv-k3u1fbpfcp-watermark.image?) |
| `destination-over` | 在原有内容下面绘制性新内容                                                  | ![05.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/556de1cbac3843dba7e952bf362b7ac3~tplv-k3u1fbpfcp-watermark.image?) |
| `destination-in`   | 原有内容只保留新内容和原有内容**重叠**的区域，新内容不显示                  | ![06.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/4453f0e53807495e8088997cfd088950~tplv-k3u1fbpfcp-watermark.image?) |
| `destination-out`  | 原有内容只保留新内容和原有内容**不重叠**的区域，新内容不显示                | ![01.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/7b000c1bd86e4929905b9027671b3984~tplv-k3u1fbpfcp-watermark.image?) |
| `destination-atop` | 原有内容只保留新内容和原有内容**重叠**的区域绘制，**新内容全部保留**        | ![02.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/d6579be09555408186d88e359e342728~tplv-k3u1fbpfcp-watermark.image?) |
| `lighter`          | 颜色值相加                                                                  | ![03.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/3703d761cbf84d10b6b465927ed08559~tplv-k3u1fbpfcp-watermark.image?) |
| `copy`             | 只显示新图形。                                                              | ![04.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/667aaedef3244064a5d78d0c4245cf20~tplv-k3u1fbpfcp-watermark.image?) |
| `xor`              | 重叠区域是透明的                                                            | ![05.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/6a09e149719f4787ab12fd37c9423ae2~tplv-k3u1fbpfcp-watermark.image?) |
| `multiply`         | 像素相乘（更黑暗）                                                          | ![06.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/903a849c81fc4f4ab8b0c5650395464b~tplv-k3u1fbpfcp-watermark.image?) |
| `screen`           | 像素被倒转，相乘，再倒转（更明亮）                                          | ![07.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/a72ce0fdf5ff456dbf9be99c2643b39e~tplv-k3u1fbpfcp-watermark.image?) |
| `overlay`          | multiply 和 screen 的结合，原本暗的地方更暗，原本亮的地方更亮。             | ![08.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/599164996ca8430a988051f780e791ef~tplv-k3u1fbpfcp-watermark.image?) |
| `darken`           | 保留两个图层中最暗的像素。                                                  | ![09.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/066790c765bd46b6ac502f7ac188929d~tplv-k3u1fbpfcp-watermark.image?) |
| `lighten`          | 保留两个图层中最亮的像素。                                                  | ![10.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/f28da99ee1944700afa7c33a69a6699c~tplv-k3u1fbpfcp-watermark.image?) |
| `color-dodge`      | 将底层除以顶层的反置。                                                      | ![01.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/ddbef559e743473d931521a1833a426a~tplv-k3u1fbpfcp-watermark.image?) |
| `color-burn`       | 将反置的底层除以顶层，然后将结果反过来。                                    | ![02.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/850bfbceb4eb47178af9bfb50c32552c~tplv-k3u1fbpfcp-watermark.image?) |
| `hard-light`       | multiply 和 screen 的结合，但上下图层互换了。                               | ![03.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/e2abdf545e0d46c3b76948d6d89f6270~tplv-k3u1fbpfcp-watermark.image?) |
| `soft-light`       | 用顶层减去底层或者相反来得到一个正值。                                      | ![05.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/331e0de4ddd1419c830e1bdfbbec5512~tplv-k3u1fbpfcp-watermark.image?) |
| `difference`       | 一个柔和版本的 hard-light。纯黑或纯白不会导致纯黑或纯白。                   | ![06.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/be8cb91d191d4e9583e62cf734a99d12~tplv-k3u1fbpfcp-watermark.image?) |
| `exclusion`        | 和 difference 相似，但对比度较低。                                          | ![07.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/b5c16e8120134e339179d37655b3b72e~tplv-k3u1fbpfcp-watermark.image?) |
| `hue`              | 保留了底层的亮度（luma）和色度（chroma），同时采用了顶层的色调（hue）。     | ![08.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/2b44717e6ac24519bd20a4807dfc8909~tplv-k3u1fbpfcp-watermark.image?) |
| `saturation`       | 保留底层的亮度（luma）和色调（hue），同时采用顶层的色度（chroma）。         | ![09.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/9a78ee615199421884d41062d2a65622~tplv-k3u1fbpfcp-watermark.image?) |
| `color`            | 保留了底层的亮度（luma），同时采用了顶层的色调(hue)和色度(chroma)。         | ![10.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/804d411115e1417e8fa53961ba93bb55~tplv-k3u1fbpfcp-watermark.image?) |
| `luminosity`       | 保持底层的色调（hue）和色度（chroma），同时采用顶层的亮度（luma）。         | ![11.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/81ee86afddcc4336bcf95f67ab787eda~tplv-k3u1fbpfcp-watermark.image?) |

其中，`destination-in`的遮盖方式，可以用于实现抠图效果（只显示涂抹区域）。

## 裁剪

`clip()` 方法其实是绘制图形（`stroke()`，`fill()`）的第三个进阶方法，`clip()` 会将当前正在构建的路径转换为当前的裁剪路径，所有在路径以外的部分都会被隐藏。

默认情况下，canvas 有一个与它自身一样大的裁切路径（也就是没有裁切效果）。

我们来画一个星空，用一个圆形的裁切路径来限制随机星星的绘制区域：

```js
function draw() {
  var ctx = document.getElementById("canvas").getContext("2d");

  // 黑色背景
  ctx.fillRect(0, 0, 600, 600);
  ctx.translate(300, 300);

  // 圆形裁剪区域
  ctx.beginPath();
  ctx.arc(0, 0, 250, 0, Math.PI * 2, true);
  ctx.clip();

  // 渐变蓝色背景
  var lingrad = ctx.createLinearGradient(0, 0, 600, 600);
  lingrad.addColorStop(0, "#232256");
  lingrad.addColorStop(1, "#143778");

  ctx.fillStyle = lingrad;
  ctx.fillRect(-300, -300, 600, 600);

  // 画星星✨
  for (var j = 1; j < 100; j++) {
    ctx.save();
    ctx.fillStyle = "#fff";
    ctx.translate(
      250 - Math.floor(Math.random() * 500),
      250 - Math.floor(Math.random() * 500)
    );
    drawStar(ctx, Math.floor(Math.random() * 4) + 2);
    ctx.restore();
  }
}
function drawStar(ctx, r) {
  ctx.save();
  ctx.beginPath();
  ctx.moveTo(r, 0);
  for (var i = 0; i < 9; i++) {
    ctx.rotate(Math.PI / 5);
    if (i % 2 == 0) {
      ctx.lineTo((r / 0.525731) * 0.200811, 0);
    } else {
      ctx.lineTo(r, 0);
    }
  }
  ctx.closePath();
  ctx.fill();
  ctx.restore();
}
```

![01.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/9cb357fb38df43c2910eaa2b08ae8886~tplv-k3u1fbpfcp-watermark.image?)
