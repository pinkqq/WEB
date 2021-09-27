**地理位置 API** 允许用户向 Web 应用程序提供他们的位置。出于隐私考虑，报告地理位置前会先请求用户许可。

![01.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/d2da221f3aca4ecfa62af37e2e444184~tplv-k3u1fbpfcp-watermark.image?)

## geolocation 对象

需要先确定 `navigator.geolocation` 是否可用：

```js
if ("geolocation" in navigator) {
  /* 地理位置服务可用 */
} else {
  /* 地理位置服务不可用 */
}
```

### 获取当前定位

通过 `getCurrentPosition()` 来获取用户当前定位位置。

```js
navigator.geolocation.getCurrentPosition(success, error, options);
```

- `success`：成功得到位置信息时的回调函数，使用 [Position 对象](https://developer.mozilla.org/zh-CN/docs/Web/API/GeolocationPosition)作为唯一的参数。

  ```js
  function success(pos) {
    // 地理状态
    var crd = pos.coords;

    console.log("Your current position is:");
    // 纬度
    console.log("Latitude : " + crd.latitude);
    // 经度
    console.log("Longitude: " + crd.longitude);
    // 准确性
    console.log("More or less " + crd.accuracy + " meters.");
  }
  ```

  默认情况下，会尽快返回一个低精度结果

- `error`：可选，获取位置信息失败时的回调函数，使用 [PositionError 对象](https://developer.mozilla.org/zh-CN/docs/Web/API/GeolocationPositionError)作为唯一的参数。
  ```js
  function error(err) {
    console.warn("ERROR(" + err.code + "): " + err.message);
    switch (error.code) {
      case error.PERMISSION_DENIED:
        x.innerHTML = "用户拒绝对获取地理位置的请求。";
        break;
      case error.POSITION_UNAVAILABLE:
        x.innerHTML = "位置信息是不可用的。";
        break;
      case error.TIMEOUT:
        x.innerHTML = "请求用户地理位置超时。";
        break;
      case error.UNKNOWN_ERROR:
        x.innerHTML = "未知错误。";
        break;
    }
  }
  ```
- `options`：可选，一个 `PositionOptions` 对象。
  - `enableHighAccuracy`：是否使用其最高精度
    - `false`，默认值，设备会通过更快响应、更少的电量等方法来尽可能的节约资源
    - `true`，这会导致较慢的响应时间或者增加电量消耗（比如对于支持 gps 的移动设备来说）
  - `timeout`：限制返回时间
    - `Infinity`，默认值，一直等待到获取位置为止。
  - `maximumAge`：可以返回多长时间（单位毫秒）内的缓存位置。

### 监视定位

通过 `watchPosition()` 来监听定位数据的变更（设备发生了移动，或获取到了更高精度的地理位置信息），它与 `getCurrentPosition()` 接受相同的参数。

```js
id = navigator.geolocation.watchPosition(success[, error[, options]])
```

该方法会返回一个 ID，如要取消监听可以通过 `clearWatch()` 传入该 ID 实现取消的目的。

```js
navigator.geolocation.clearWatch(id);
```

示例：

```js
var id, target, options;

function success(pos) {
  var crd = pos.coords;

  // 到达目的地
  if (target.latitude === crd.latitude && target.longitude === crd.longitude) {
    console.log("Congratulations, you reached the target");
    navigator.geolocation.clearWatch(id);
  }
}

function error(err) {
  console.warn("ERROR(" + err.code + "): " + err.message);
}

// 目的地
target = {
  latitude: 0,
  longitude: 0,
};

options = {
  enableHighAccuracy: false,
  timeout: 5000,
  maximumAge: 0,
};

id = navigator.geolocation.watchPosition(success, error, options);
```

## Position 对象

| 属性                    | 描述                   |
| ----------------------- | ---------------------- |
| coords.latitude         | 十进制数的纬度         |
| coords.longitude        | 十进制数的经度         |
| coords.accuracy         | 位置精度               |
| coords.altitude         | 海拔，海平面以上以米计 |
| coords.altitudeAccuracy | 位置的海拔精度         |
| coords.heading          | 方向，从正北开始以度计 |
| coords.speed            | 速度，以米/每秒计      |
| timestamp               | 响应的日期/时间        |
