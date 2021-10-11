## 什么是 WebSocket

WebSocket 是 HTML5 开始提供的一种在单个 TCP 连接上进行全双工通讯的协议。

WebSocket 使得客户端和服务器之间的数据交换变得更加简单，允许服务端主动向客户端推送数据。

在 WebSocket API 中，浏览器和服务器只需要完成一次握手，两者之间就直接可以创建持久性的连接，浏览器和服务器之间就形成了一条快速通道，用于双向数据的传输。

现在，很多网站为了实现推送技术，所用的技术都是 Ajax 轮询。轮询是在特定的的时间间隔（如每 1 秒），由浏览器对服务器发出 HTTP 请求，然后由服务器返回最新的数据给客户端的浏览器。这种传统的模式带来很明显的缺点，即浏览器需要不断的向服务器发出请求，然而 HTTP 请求可能包含较长的头部，其中真正有效的数据可能只是很小的一部分，显然这样会浪费很多的带宽等资源。

如果选择 WebSocket 协议，能更好的节省服务器资源和带宽，并且能够更实时地进行通讯。

![](https://www.runoob.com/wp-content/uploads/2016/03/ws.png)

## 创建 WebSocket 对象

```js
var Socket = new WebSocket(url, [protocol]);
```

- `url`，指定连接的 URL。
- `protocol`，是可选的，指定了可接受的子协议。

返回一个 WebSocket 对象。

**WebSocket 对象的属性：**

- `Socket.readyState`：只读属性，表示连接状态

  - 0（CONNECTING）- 表示连接尚未建立。

  - 1（OPEN）- 表示连接已建立，可以进行通信。

  - 2（CLOSING）- 表示连接正在进行关闭。

  - 3（CLOSED）- 表示连接已经关闭或者连接不能打开。

- `Socket.bufferedAmount`：只读属性，已被 `send()` 放入正在队列中等待传输，但是还没有发出的 UTF-8 文本字节数。

- `Socket.binaryType`：连接所传输二进制数据的类型。

- `Socket.protocol`：只读属性，服务器选择的子协议。

- `Socket.url`：只读属性，创建 WebSocket 实例对象时 URL 的绝对路径。

**WebSocket 对象的相关事件：**

| 事件    | 事件处理程序     | 描述                       |
| ------- | ---------------- | -------------------------- |
| open    | Socket.onopen    | 连接建立时触发             |
| message | Socket.onmessage | 客户端接收服务端数据时触发 |
| error   | Socket.onerror   | 通信发生错误时触发         |
| close   | Socket.onclose   | 连接关闭时触发             |

**WebSocket 对象的相关方法：**
|方法| 描述|
|--|--|
|Socket.send() |使用连接发送数据|
|Socket.close()|关闭连接|

## WebSocket 实例

WebSocket 协议本质上是一个基于 TCP 的协议。 WebSocket 连接的建立流程：

- 首先，浏览器向服务器发起一个 HTTP 请求，和通常的 HTTP 请求不同的是，包含了一些附加头信息，其中附加头信息 "Upgrade: WebSocket" 表明这是一个申请协议升级的 HTTP 请求。
- 服务器端解析这些附加的头信息然后产生应答信息返回给客户端。

客户端和服务器端的 WebSocket 连接就建立起来了，双方就可以通过这个连接通道自由的传递信息，并且这个连接会持续存在直到客户端或者服务器端的某一方主动的关闭连接。

### 客户端的代码

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Web Socket</title>
  </head>
  <body>
    <script type="text/javascript">
      function WebSocketTest() {
        if ("WebSocket" in window) {
          alert("您的浏览器支持 WebSocket!");

          // 打开一个 web socket
          var ws = new WebSocket("ws://localhost:9998/echo");

          ws.onopen = function () {
            // Web Socket 已连接上，使用 send() 方法发送数据
            ws.send("发送数据");
            alert("数据发送中...");
          };

          ws.onmessage = function (evt) {
            var received_msg = evt.data;
            alert("数据已接收...");
          };

          ws.onclose = function () {
            // 关闭 websocket
            alert("连接已关闭...");
          };
        } else {
          // 浏览器不支持 WebSocket
          alert("您的浏览器不支持 WebSocket!");
        }
      }
      WebSocketTest();
    </script>
  </body>
</html>
```
