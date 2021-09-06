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

## 渲染上下文
