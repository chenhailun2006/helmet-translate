## 浏览器功能策略

`Helmet`的`featurePolicy`中间件可以让你去限制允许使用哪些浏览器功能。例如，你可以禁用全屏或振动相关的 API。

### 攻击

限制你的代码的功能范围总是有益处的，因为更少的功能意味着更少的攻击。

Web 浏览器有很多不同的功能，比如振动、全屏、语音访问权限等。尽管这些功能很有用，但是你可能也不希望全部使用，而且你可能也不希望你代码中的任何第三方脚本去使用某些浏览器功能。

### HTTP 头部

HTTP 头部字段`Feature-Policy`告诉浏览器可以使用哪些浏览器功能。比如，如果你想要完全禁用通知功能（notifications），并且允许 **example.com**域名使用支付功能（payments），你可以发送如下的头部字段：

```
Feature-Policy: notifications 'none'; payments example.com
```

阅读更多：

- [一个新的 HTTP 安全头：Feature-Policy](https://scotthelme.co.uk/a-new-security-header-feature-policy/)

- [Feature-Policy](https://developers.google.com/web/updates/2018/06/feature-policy)

### 代码

`Helmet`的`featurePolicy`中间件可以帮你设置 HTTP 的`Feature-Policy`头部字段。

你可以将`featurePolicy`模块作为`Helmet`的一部分来使用：

```javascript
const helmet = require('helmet');

app.use(
 helmet.featurePolicy({
  features: {
   fullscreen: ["'self'"],
   vibrate: ["'none'"],
   payment: ['example.com'],
   syncXhr: ["'none'"],
  },
 })
);
```

你也可以单独使用此模块：

```javascript
const featurePolicy = require('feature-policy');

app.use(
 featurePolicy({
  features: {
   fullscreen: ["'self'"],
   vibrate: ["'none'"],
   payment: ['example.com'],
   syncXhr: ["'none'"],
  },
 })
);
```

目前支持对以下的浏览器功能进行控制：

- **accelerometer**：是否允许使用加速度计功能

- **ambientLightSensor**：是否允许使用环境光传感器

- **autoplay**：是否允许自动播放媒体文件

- **camera**：是否允许使用视频输入设备

- **documentDomain**：是否允许设置 document.domain

- **documentWrite**：是否允许使用 document.write 方法

- **encryptMedia**：是否允许使用 Encrypt Media Extensions API

- **fontDisplayLateSwap**：

- **fullscreen**：是否允许全屏

- **geolocation**：是否允许使用 Geolocation 相关接口

- **gyroscope**：

- **layoutAnimations**：是否允许显示布局动画

- **legacyImageFormats**：是否允许以传统格式显示图片

- **loadingFrameDefaultEager**：

- **magnetometer**：是否允许使用 Magnetometer 相关接口收集设备的方位信息

- **microphone**：是否允许使用音频输入设备

- **midi**：是否允许使用 Web MIDI 相关接口

- **oversizedImages**：是否允许下载和显示大图片

- **payment**：是否允许使用支付相关的 API

- **pictureInPicture**：是否允许使用画中画模式播放视频

- **serial**：

- **speaker**：是否允许通过任意方法播放音频文件

- **syncScript**：是否允许同步脚本执行

- **syncXhr**：是否允许发送同步的 XHR 请求

- **unoptimizedImages**：是否允许下载并显示未优化的图片

- **unoptimizedLosslessImages**：

- **unoptimizedLossyImages**：是否允许下载并显示未优化的丢失对象

- **unsizedMedia**：是否允许在初始布局完成后改变媒体元素的大小

- **usb**：是否允许使用 WebUSB 相关的 API

- **verticalScroll**：是否允许垂直滚动

- **vr**：是否允许使用 WebVR 相关的 API

- **wakeLock**：是否允许使用 WakeLock 相关的 API

- **xr**：是否允许使用 WebVR 相关的 API
