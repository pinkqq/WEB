通过上一篇，我们了解到 canvas 上的所有绘制动作都是通过 Javascript 操控完成的，也正是这个特性，让 canvas 获得了实现简单交互动画的能力。

用 canvas 做动画的最大的限制可能就是图像一旦绘制出来，想要移动它，就不得不对所有东西（包括之前的）进行重绘。重绘是相当费时的，而且性能很依赖于电脑的速度。

## 动画的基本步骤

绘制每一帧的步骤：

- 清空 canvas：比如 `clearRect` 方法
- 保存 canvas 状态：`save()`
- 绘制动画帧
- 恢复 canvas 状态：`restore()`

## 操控动画

为了实现动画，我们需要一些可以定时执行重绘的方法。

- `setInterval(function, delay)`：适合不需要交互的场景
- `setTimeout(function, delay)`：通过键盘或者鼠标事件来捕捉用户的交互，再用 `setTimeout` 执行相应的动作
- `requestAnimationFrame(callback)`：这个方法更加平缓并更加有效率，当系统准备好了重绘条件的时候，才调用绘制动画帧。

## 长尾效果

在清除画布时，用一个透明度的填充矩形覆盖全屏，就可以实现长尾效果，比如下面这个小球：

```js
ctx.fillStyle = "rgba(255,255,255,0.3)";
ctx.fillRect(0, 0, canvas.width, canvas.height);
```

![01.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/7832d4ea40cb42878c54ba85b853d385~tplv-k3u1fbpfcp-watermark.image?)

## ImageData 对象

我们通过 ImageData 对象，可以达到像素级别的操作。

ImageData 对象中存储着 canvas 对象真实的像素数据，它包含以下几个只读属性：

- `width`：图片宽度，单位是像素
- `height`：图片高度，单位是像素
- `data`：Uint8ClampedArray 类型的一维数组[^1]，每四个值代表一个像素，按照红，绿，蓝和透明值的顺序。
  [^1]: `Uint8ClampedArray`（8 位无符号整型固定数组） 类型化数组表示一个由值固定在 0-255 区间的 8 位无符号整型组成的数组；如果你指定一个在 [0,255] 区间外的值，它将被替换为 0 或 255；如果你指定一个非整数，那么它将被设置为最接近它的整数。

### createImageData

有两种方式创建一个新的 ImageData 对象：

- 具体特定尺寸，所有像素被预设为透明黑：

```js
var myImageData = ctx.createImageData(width, height);
```

- 基于另一个 ImageData 对象，并非复制，新对象的像素会全部被预设为透明黑：

```js
var myImageData = ctx.createImageData(anotherImageData);
```

### getImageData

获得像素数据：

```js
var myImageData = ctx.getImageData(left, top, width, height);
```

任何在画布以外的元素都会被返回成一个透明黑的 ImageData 对像。

#### 拾色器

借助鼠标时间 mousemove 和 mousedown，我们根据鼠标的位置和 getImageData 获取当前的像素信息。

![01.gif](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/1e537c55d95e4a25ba7e2b5e9719e343~tplv-k3u1fbpfcp-watermark.image?)

```js
// 加载图片
var img = new Image();
img.crossOrigin = "anonymous";
img.src =
  "https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/3747c10c114f491daf72aa2b7e971c42~tplv-k3u1fbpfcp-zoom-crop-mark:1304:1304:1304:734.awebp?";
var canvas = document.getElementById("canvas");
var ctx = canvas.getContext("2d");
img.onload = function () {
  ctx.drawImage(img, 0, 0);
  img.style.display = "none";
};

// 颜色展示区块
var hoveredColor = document.getElementById("hovered-color");
var selectedColor = document.getElementById("selected-color");

// 获取像素数据
function pick(event, target) {
  let pixel = ctx.getImageData(event.offsetX, event.offsetY, 1, 1).data;
  let rgba = `rgba(${pixel[0]},${pixel[1]},${pixel[2]},${pixel[3] / 255})`;
  target.style.background = rgba;
  target.textContent = rgba;
}

// mousemove
canvas.addEventListener("mousemove", function (event) {
  pick(event, hoveredColor);
});

// mousedown
canvas.addEventListener("mousedown", function (event) {
  pick(event, selectedColor);
});
```

### putImageData

改写像素信息：

```js
ctx.putImageData(myImageData, dx, dy);
```

`dx,dy` 代表你希望绘制图像的左上角位置。

#### 反相颜色

我们对上面的图像做一个反相修改：

![01.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/8f95d6a8c6724cdda0bb8e5f88a26c92~tplv-k3u1fbpfcp-watermark.image?)

```js
var invert = function () {
  ctx.drawImage(img, 0, 0);
  const imageData = ctx.getImageData(0, 0, canvas.width, canvas.height);
  const data = imageData.data;
  for (var i = 0; i < data.length; i += 4) {
    data[i] = 255 - data[i]; // red
    data[i + 1] = 255 - data[i + 1]; // green
    data[i + 2] = 255 - data[i + 2]; // blue
  }
  ctx.putImageData(imageData, 0, 0);
};
invert();
```

## 保存图片

```js
canvas.toDataURL("image/png");
```

默认创建一个 PNG 图片。

我们也可以选择创建一个 JPEG 图片，还可以选择从 0 到 1 的品质量，1 表示最好品质，0 基本不被辨析但有比较小的文件大小。

```js
canvas.toDataURL("image/jpeg", quality);
```

## Canvas 的优化

如果你把 canvas 用于游戏及复杂的图像可视化中去，相信这些改善性能的建议会很有帮助：

- **避免浮点数的坐标点，用整数取而代之**
  ```js
  // ❌
  ctx.drawImage(myImage, 0.3, 0.5);
  // ✅
  ctx.drawIamge(myImage, Math.floor(0.3), Math.floor(0.5));
  ```
- **不要在用 drawImage 时缩放图像，** 可以缓存图片的不同尺寸
- **使用多层画布去画一个复杂的场景**，比如静态背景和频繁发生移动的对象
- **用 CSS 设置大的背景图**，避免在每一帧在画布上反复地绘制大图。
- **用 CSS transforms 特性缩放画布**
  CSS transforms 使用 GPU 速度会更快。当然最好是不直接缩放画布，或者让小的画布按比例放大，而不是大的画布按比例缩小。
- **关闭透明度**
  如果不需要透明度时，手动关闭后浏览器会进行内部优化。
  ```js
  var ctx = canvas.getContext("2d", { alpha: false });
  ```
- **将画布的函数调用集合到一起**（例如，画一条折线，而不要画多条分开的直线）
- **避免不必要的画布状态改变**
- **渲染画布中的不同点，而非整个新状态**
- **尽可能避免 `shadowBlur` 特性**
- **尽可能避免 `text rendering`**
- **不同的场景可以选择不同的清除画布的方法**：`clearRect()`、`fillRect()`、调整 canvas 大小
- **使用 `window.requestAnimationFrame()` 而非 `window.setInterval()`**
- **请谨慎使用大型物理库**
