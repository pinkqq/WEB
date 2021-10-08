## 什么是 Web Storage

Web Storage 提供了 Web 存储机制，浏览器可以安全地存储键值对，比使用 cookie 更加直观。它可以存储大量的数据，而不影响网站的性能.

以键值对的形式存储，类似于对象。我们可以通过多种方式访问到这些值：

```js
localStorage.colorSetting = "#a4509b";
localStorage["colorSetting"] = "#a4509b";
localStorage.getItem("colorSetting"); // '#a4509b'
localStorage.setItem("colorSetting", "#a4509b");
```

Web Storage 有两种机制：

- **sessionStorage**，用于临时保存同一窗口(或标签页)的数据，该存储区域只在页面会话期间可用（即只要浏览器处于打开状态，包括页面重新加载和恢复）。
- **localStorage**，用于长久保存整个网站的数据，保存的数据没有过期时间，直到手动去除（在浏览器关闭，然后重新打开后数据仍然存在）。

![](https://www.runoob.com/wp-content/uploads/2019/04/3793073884-56950753e65db_articlex.png)

## 是否支持 Storage

```js
if (window.Storage) {
  // 是的! 支持 localStorage  sessionStorage 对象!
  // 一些代码.....
}
```

## API

不管是 localStorage，还是 sessionStorage，可使用的 API 都相同，常用的有如下几个（以 localStorage 为例）：

- 保存数据：`localStorage.setItem(key,value)`;
- 读取数据：`localStorage.getItem(key)`;
- 删除单个数据：`localStorage.removeItem(key)`;
- 删除所有数据：`localStorage.clear()`;
- 得到某个索引的 key：`localStorage.key(index)`;

## 一个示例

一个可以自定义颜色、字体和装饰图片的页面，当选择了不同的选项后，页面会立即更新并将数据存储到 localStorage 中去：

### 填充 localStorage

如果 localStorage 还未被填充，则初始化数据；如果数据已填充，则根据数据更新页面：

```js
if (!localStorage.bgcolor) {
  populateStorage();
} else {
  setStyles();
}
```

### 更新 localStorage

```js
function populateStorage() {
  localStorage.setItem("bgcolor", document.getElementById("bgcolor").value);
  localStorage.setItem("font", document.getElementById("font").value);
  localStorage.setItem("image", document.getElementById("image").value);

  setStyles();
}
```

监听页面数据变化，触发 `populateStorage`：

```js
bgcolorForm.onchange = populateStorage;
fontForm.onchange = populateStorage;
imageForm.onchange = populateStorage;
```

### 更新页面

```js
function setStyles() {
  var currentColor = localStorage.getItem("bgcolor");
  var currentFont = localStorage.getItem("font");
  var currentImage = localStorage.getItem("image");

  document.getElementById("bgcolor").value = currentColor;
  document.getElementById("font").value = currentFont;
  document.getElementById("image").value = currentImage;

  htmlElem.style.backgroundColor = "#" + currentColor;
  pElem.style.fontFamily = currentFont;
  imgElem.setAttribute("src", currentImage);
}
```

### StorageEvent 响应存储的变化

无论何时，`Storage` 对象发生变化时（即创建/更新/删除数据项时，重复设置相同的键值不会触发该事件，`Storage.clear()` 方法至多触发一次该事件），`StorageEvent` 事件会触发。

需要注意的是，下面三种情况中，只有一种情况会触发：

- 在同一个页面内发生的改变不会起作用
- **在相同域名下的其他页面（如一个新标签或 iframe）发生的改变才会起作用**
- 在其他域名下的页面不能访问相同的 Storage 对象。

```js
window.addEventListener("storage", function (e) {});
```

返回的事件对象包含了很多有用的信息，比如 _改变的数据项的键_，_改变前的旧值_，_改变后的新值_，_改变的存储对象所在的文档的 URL_，以及 _存储对象本身_。
