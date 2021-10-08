## 什么是 Web Worker

**Web Worker** 为在后台线程中运行 JavaScript 脚本提供了一种简单的方法，线程可以执行任务而不干扰用户界面（可以继续做任何愿意做的事情：点击、选取内容等等），不会影响页面的性能。

在 worker 线程中可以运行任何代码，不过有一些例外情况。因为 workers 运行在另一个全局上下文中，所以

- 不能直接操作 DOM 节点
- 无法访问 window 对象
- 无法访问 document 对象
- 无法访问 parent 对象

worker 可访问到的内容：[Functions and classes available to Web Workers](https://developer.mozilla.org/en-US/docs/Web/API/Web_Workers_API/Functions_and_classes_available_to_workers)

## 消息机制

workers 和主线程间的数据传递

- `postMessage()`：发送各自的消息
- `onmessage`：响应消息（`event.data`）

这个过程中数据并不是被共享而是被复制。

## subworker

如果需要的话 worker 能够生成更多的 worker。这就是所谓的 subworker，它们必须托管在同源的父页面内。**而且，subworker 解析 URI 时会相对于父 worker 的地址而不是自身页面的地址。** 这使得 worker 更容易记录它们之间的依赖关系。

## 专用 Web Workers 实例

### 检测浏览器是否支持

```js
if (window.Worker) {
  // ...
}
```

### 创建 Worker 对象

创建一个 Worker 对象很简单：

- 调用 `Worker()` 的构造器，
- 指定一个脚本的 URI

```js
var myWorker = new Worker("worker.js");
```

### 消息的接收和发送

input（first、second）的值发生改变时，将数据传递给 worker：

```js
// main.js
first.onchange = function () {
  // Message posted to worker
  myWorker.postMessage([first.value, second.value]);
};

second.onchange = function () {
  // Message posted to worker
  myWorker.postMessage([first.value, second.value]);
};
```

在 worker 中接收数据后，做乘法处理后，将结果抛回给主线程：

```js
// worker.js
onmessage = function (e) {
  var workerResult = "Result: " + e.data[0] * e.data[1];
  postMessage(workerResult);
};
```

主线程也使用 onmessage 接受计算结果：

```js
// main.js
myWorker.onmessage = function (e) {
  // Message received from worker
  result.textContent = e.data;
};
```

> **注意：** 在主线程中使用时，`onmessage` 和 `postMessage()` 必须挂在 `worker` 对象上，而在 `worker` 中使用时不用这样做。原因是，在 `worker` 内部，`worker` 是有效的全局作用域。

### 终止 Worker

从主线程中立刻终止一个运行中的 worker：

```js
myWorker.terminate();
```

而在 worker 线程中，workers 也可以关闭自己：

```js
close();
```

## 共享 Worker

> **注意：** 如果共享 worker 可以被多个浏览上下文调用，所有这些浏览上下文必须属于同源（相同的协议，主机和端口号）。

一个共享 worker 可以被多个脚本使用——即使这些脚本正在被不同的 window、iframe 或者 worker 访问。

### 生成一个共享 worker

```js
var myWorker = new SharedWorker("worker.js");
```

与专用 Worker 一个最大的区别是：共享 worker 通信必须通过端口对象（一个确切的打开的端口），在专用 worker 中这一部分是隐式进行的。

在传递消息之前，端口连接必须被显式的打开，打开方式是使用 `onmessage` 事件处理函数或者 `start()` 方法。如果消息事件被 `addEventListener()` 方法使用，则选择 `start()` 打开端口：

```js
myWorker.port.start(); // 父级线程中的调用
```

```js
port.start(); // worker线程中的调用, 假设port变量代表一个端口
```

### 消息的接收和发送

`postMessage()` 方法必须被端口对象调用。

首先，在主线程中，向 worker 发送消息：

```js
// main.js
myWorker.port.postMessage();
```

在 worker 中，接受消息的流程会比较复杂：

- 当一个端口连接被创建时（例如：在父级线程中，设置 `onmessage` 事件处理函数，或者显式调用 `start()` 方法时），使用 `onconnect` 事件来执行函数
- 获取端口（`event.ports[0]`）
- 为端口添加一个消息处理函数 `onmessage`（隐式的打开了与主线程的端口连接）

```js
// worker.js
onconnect = function (e) {
  var port = e.ports[0];

  port.onmessage = function (e) {
    var workerResult = "Result: " + e.data[0] * e.data[1];
    port.postMessage(workerResult);
  };
};
```

最后，在主线程中接收消息：

```js
// main.js
myWorker.port.onmessage = function (e) {
  result2.textContent = e.data;
  console.log("Message received from worker");
};
```

## Chrome：不允许加载本地文件

```
Uncaught SecurityError: Failed to create a worker: script at '(path)/worker.js' cannot be accessed from origin 'null'.
```

当我们的脚本是本地文件时，Chrome 不允许加载 Web Worker。一种解决方式是通过 `--allow-file-access-from-files` 标记启动 Chrome，启动前保证 Chrome 的所有窗口都关闭了。

MacOsX 的命令：

```
/Applications/Google\ Chrome.app/Contents/MacOS/Google\ Chrome --allow-file-access-from-files
```
