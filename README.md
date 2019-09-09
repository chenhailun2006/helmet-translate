## Helmet

`Helmet`是一个通过设置各种 HTTP 头部来保护你的`Express`应用的插件。它不是银弹，但是很有用！

### 快速起步

首先，通过`npm install helmet --save`在你的项目中安装`Helmet`。然后：

```javascript
const express = require('express');
const helmet = require('helmet');

const app = express();

app.use(helmet());

// ...
```

你应尽早在你的中间件栈中使用`Helmet`来保证 HTTP 头部被正确的设置。

你也可以使用它的一部分内容，如下所示：

```javascript
app.use(helmet.noCache());
app.use(helmet.frameguard());
```

你可以禁用一个默认被激活的中间件，如下代码会将 **frameguard** 中间件禁用，并且保持其他的中间件处于默认方式：

```javascript
app.use(
 helmet({
  frameguard: false,
 })
);
```

你也可以对一些中间件设置一些配置项，通过如下方式设置配置项将会“总是”包含中间件，无论其默认方式如何：

```javascript
app.use(
 helmet({
  frameguard: {
   action: 'deny',
  },
 })
);
```

如果你使用的是 Express 3, 请确保这些中间件位于 **app.router** 中间件之前。

### Helmet 是如何工作的

`Helmet`是 14 个小的设置 HTTP 响应头部的中间件函数的集合。默认情况下，运行 **app.use(helmet())**不会包含这 14 个中间件函数中的任何一个。

| 模块                                                                        | 默认激活？ |
| --------------------------------------------------------------------------- | :--------: |
| 用于设置内容安全策略的[contentSecurityPolicy](./content-security-policy.md) |
| 用于处理 Adobe 产品跨域请求的[crossdomain](./corss-domain.md)               |
| 用于控制浏览器 DNS 预解析的[dnsPrefetchControl](./dns-prefetch-control.md)  |     √      |
| 用于处理证书透明度的[expectCt](./expect-ct.md)                              |
| 用于限制网站特征的[featurePolicy](./feature-policy.md)                      |
| 用于阻止点击劫持的[frameguard](./frameguard.md)                             |     √      |
| 用于移除 X-Powered-By 头部的[hidePoweredBy](./hide-powered-by.md)           |     √      |
| 用于保持使用 Https 协议的[hsts](./hsts.md)                                  |     √      |
| 为 IE8+设置 X-Download-Options 的[ieNoOpen](./ie-no-open.md)                |     √      |
| 禁用客户端缓存的[noCache](./no-cache.md)                                    |
| 用于阻止客户端爬取 MIME 类型数据的[noSniff](./nosniffing.md)                |     √      |
| 用于隐藏 Referer 头部的[referrerPolicy](./referrer-policy.md)               |
| 添加一些小的 XSS 保护机制的[xssFilter](./xss-filter.md)                     |     √      |
