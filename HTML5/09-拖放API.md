拖放是一种常见的特性，即抓取对象以后拖到另一个位置。在 HTML5 中，拖放是标准的一部分，任何元素都能够拖放。

**一个典型的拖拽操作是这样的**：用户选中一个可拖拽的（`draggable`）元素，并将其拖拽（鼠标不放开）到一个可放置的（`droppable`）元素，然后释放鼠标。

## 拖拽事件

在操作期间，会触发一些事件类型，有一些事件类型可能会被多次触发。

| 事件      | On 型事件处理程序 | 触发时刻                                                                |
| --------- | ----------------- | ----------------------------------------------------------------------- |
| drag      | ondrag            | 当拖拽元素或选中的文本时触发。                                          |
| dragend   | ondragend         | 当拖拽操作结束时触发 (比如松开鼠标按键或敲“Esc”键).                     |
| dragenter | ondragenter       | 当拖拽元素或选中的文本到一个可释放目标时触发。                          |
| dragexit  | ondragexit        | 当元素变得不再是拖拽操作的选中目标时触发。                              |
| dragleave | ondragleave       | 当拖拽元素或选中的文本离开一个可释放目标时触发。                        |
| dragover  | ondragover        | 当元素或选中的文本被拖到一个可释放目标上时触发（每 100 毫秒触发一次）。 |
| dragstart | ondragstart       | 当用户开始拖拽一个元素或选中的文本时触发（见开始拖拽操作 。             |
| drop      | ondrop            | 当元素或选中的文本在可释放目标上被释放时触发 。                         |

> 注意：当从操作系统向浏览器中拖拽文件时，不会触发 `dragstart` 和 `dragend` 事件。

**对于这些事件可以做一个简单的分类：**

- 与拖拽元素相关：

  - `dragstart`：拖动开始

  - `drag`：拖动中

  - `dragend`：拖动结束

  - 拖动过程： `dragstart`*1 + `drag`*n + `dragend`\*1

- 与可释放目标相关：

  - `dragenter`：拖动着进入

  - `dragover`：拖动着悬停

  - `dragleave`：拖动着离开

  - `drop`：释放

  - 拖动过程：`dragenter`*1 + `dragover`*n + `dragleave`\*1 || `drop`\*1

## 可拖拽属性

在 HTML 中，除了**图像**、**链接**和**选择的文本**默认的可拖拽行为之外，其他元素在默认情况下是不可拖拽的。

要使其他的 HTML 元素可拖拽，可以把想要拖拽的元素的 `draggable` 属性设置成 `draggable="true"`。

```html
<p draggable="true">This text <strong>may</strong> be dragged.</p>
```

> 注意：当一个元素被设置成可拖拽时， 元素中的文本和其他子元素不能再以正常的方式（通过鼠标点击和拖拽）被选中。

## 拖拽数据

每个拖拽事件都有一个 `dataTransfer` 属性（一个 `DataTransfer` 对象），其中保存着事件的数据。我们往往在拖拽元素时，存储数据（`setData`）；在释放到可放置目标时，读取数据（`getData`）。在拖拽操作中包含任意数量的数据项。每个数据项都是一个 `string` 类型，典型的 `MIME` 类型，如：`text/html`。

- `DataTransfer.dropEffect`
  获取当前选定的拖放操作类型或者设置的为一个新的类型。值必须为 `none`, `copy`, `link` 或 `move`。
- `DataTransfer.effectAllowed`
  提供所有可用的操作类型。取值 `none`, `copy`, `copyLink`, `copyMove`, `link`, `linkMove`, `move`, `all` or `uninitialized` 之一。
- `DataTransfer.files`
  包含数据传输中可用的所有本地文件的列表。如果拖动操作不涉及拖动文件，则此属性为空列表。
- `DataTransfer.items` 只读
  提供一个包含所有拖动数据列表的 DataTransferItemList 对象。
- `DataTransfer.types` 只读
  一个提供 dragstart 事件中设置的格式（ strings 数组）。

当拖拽发生时，数据必须与被拖拽的项目相关联。例如，当在文本框中拖拽选定的文本时，与拖拽数据项相关联的数据就是文本本身。类似地，当在 Web 页面上拖拽链接时，拖拽数据项就是链接的 URL 。

### setData

除了 `DataTransfer` 的原始属性，我们还使用 `setData()` 方法，设置自定义拖拽数据项，方法接受两个参数，数据类型和数据值。

```js
const dt = event.dataTransfer;
dt.setData("application/x.bookmark", bookmarkString);
dt.setData("text/uri-list", "http://www.mozilla.org");
dt.setData("text/plain", "http://www.mozilla.org");
```

使用 `getData()` 方法，可以获得设置的数据项。

```js
event.dataTransfer.getData("text/plain"); // "http://www.mozilla.org"
```

### clearData

使用 `clearData(type)` 方法清除这些数据，该方法接收一个参数，即要删除的数据类型。

```js
event.dataTransfer.clearData("text/uri-list");
```

如果没有声明 type，则所有类型的数据都会被删除。。如果拖拽不包含拖拽数据项，或者所有的数据项都被清除，那么就不会出现拖拽行为。

## 拖拽图像

当拖拽发生时，会生成拖拽目标的一个半透明图像，并在拖拽过程中跟踪鼠标指针。这个图像是自动创建的，但是，你可以使用 `setDragImage()` 方法来自定义拖拽反馈图像。

```js
event.dataTransfer.setDragImage(image, xOffset, yOffset);
```

- `image`：图像的引用。通常是一个 `<img>` 元素，但也可以是 `<canvas>` 或任何其他元素。
- `xOffset / yOffset`：鼠标位置相对图像左上角的偏移量。

**一个使用 img 元素绘制自定义的拖拽反馈图像的例子：**

```html
<img src="../asset/03.png" alt="" id="img" />
<p id="drag" draggable="true">This text may be dragged.</p>
<script>
  document.getElementById("drag").addEventListener("dragstart", function (ev) {
    console.log("drag-start");
    let img = document.getElementById("img");
    ev.dataTransfer.setDragImage(img, 10, 10);
  });
</script>
```

## 拖拽效果

在 `dragstart` 事件监听程序中设置 `effectAllowed` 属性以指定允许拖拽源头执行哪几种。

```js
ev.dataTransfer.effectAllowed = "all";
```

- **基础的三种类型**
  - **copy**：操作用来指示被拖拽的数据将从当前位置复制到放置位置，
  - **move**：操作指示被拖拽的数据会被移动，
  - **link**：操作表示在源和放置位置之间将会创建某种形式的关系或连接。
- **组合类型**
  - **copyMove**：复制或移动
  - **copyLink**：复制或链接
  - **linkMove**：链接或移动
  - **all**：默认值，复制、移动或链接

`dropEffect` 属性用来控制拖放操作中用户给予的反馈，它会影响到拖拽过程中浏览器显示的鼠标样式。在 `dragenter` 或 `dragover` 事件期间修改 `dropEffect` 属性，取值 `none`、`copy`、`move` 或 `link`，不使用上述的组合值。

```js
ev.dataTransfer.dropEffect = "copy";
```

在 `drop` 和 `dragend` 事件中，你可以检查 `dropEffect` 属性，以确定最终选择了哪种效果。如果所选的效果是 `move`，那么应该在 `dragend` 事件中从拖拽源头删除拖拽数据。

## 放置对象

当拖拽一个项目到 HTML 元素中时，浏览器默认不会有任何响应。想要让一个元素变成可释放区域，该元素必须在 `ondragover` 和 `ondrop` 中阻止默认的处理并设置事件处理：

```html
<script>
  function dragover_handler(ev) {
    // 阻止默认的处理，表明在该位置允许放置
    ev.preventDefault();
    ev.dataTransfer.dropEffect = "move";
  }
  function drop_handler(ev) {
    // 阻止默认的处理，表明在该位置允许放置
    ev.preventDefault();
    var data = ev.dataTransfer.getData("text/plain");
    ev.target.appendChild(document.getElementById(data));
  }
</script>

<p
  id="target"
  ondrop="drop_handler(event)"
  ondragover="dragover_handler(event)"
>
  Drop Zone
</p>
```

## 拖拽结束

当用户放开鼠标，拖放操作就会结束。

不管拖拽是完成还是被取消，在源元素（开始拖拽时的目标元素）上都会触发 `dragend` 这个事件。

`dragend` 事件处理程序可以检查 `dropEffect` 属性的值来确认拖拽成功与否。如果 `dropEffect` 属性值为 `none`，则拖拽会被取消。
