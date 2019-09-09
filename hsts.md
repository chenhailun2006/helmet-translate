## HSTS

`hsts`模块通过设置 HTTP 头部字段`Strict-Transport-Security`来保证用户始终使用 HTTPS 协议。

### 攻击

`HTTPS`协议是一个安全协议，相对而言，`HTTP`协议的安全性可能就要差一些了。使用`HTTP`协议发送的报文是没有加密的，这样 HTTP 用户就比较容易受到“中间人攻击”。

如果我想要黑你，就更希望你使用的是`HTTP`协议，而不是`HTTPS`协议。

阅读更多：

- [维基百科对 HTTPS 的定义](https://en.wikipedia.org/wiki/HTTPS)
- [网络安全：为什么你应该使用 HTTPS](https://mashable.com/2011/05/31/https-web-security/)

### HTTP 头部

`HTTP`头部字段`Strict-Transport-Security`告诉浏览器始终使用`HTTPS`协议发送请求，永远不要使用不安全的`HTTP`协议。当浏览器发现如下的头部字段，则会在未来的 60 天内只使用`HTTPS`协议访问当前网站服务器：

```
Strict-Transport-Security:max-age=5184000
```

需要注意的是，此字段不会告诉用户将`HTTP`协议转换为`HTTPS`协议，而是告诉用户始终使用`HTTPS`协议。你可以使用`express-enforces-ssl`模块来强制使用`HTTPS`协议。

阅读更多：

- [规范](https://tools.ietf.org/html/rfc6797)

### 代码

`HELMET`的`HSTS`模块是一个比较简单的中间件，可以用它来设置`HTTP`头部的`Strict-Transport-Security`字段。

你可以将其作为`Helmet`的一部分使用：

```javascript
const helmet = require('helmet');

const sixtyDaysInSeconds = 5184000;

app.use(
  helmet.hsts({
    maxAge: sixtyDaysInSeconds
  })
);
```

你也可以将其作为一个单独的模块使用：

```javascript
const hsts = require('hsts');

const sixtyDaysInSeconds = 5184000;
app.use(
  hsts({
    maxAge: sixtyDaysInSeconds
  })
);
```

### 包含子域

默认情况下会包含`includeSubDomains`指令，如果不想包含子域，可以将`includeSubDomains`属性设置为**false**：

```javascript
app.use(
  hsts({
    maxAge: sixtyDaysInSeconds,
    includeSubDomains: false
  })
);
```

### 根据条件设置此字段

`Strict-Tranport-Security`字段总是会被设置，只不过在`HTTP`协议下此字段会被忽略。你可能想要根据某些条件来决定是否要设置此字段：

```javascript
const hstsMiddleware = helmet.hsts({
  /*....*/
});
app.use((req, res, next) => {
  if ((req, secure)) {
    hstsMiddleware(req, res, next);
  } else {
    next();
  }
});
```

### 在谷歌浏览器中提前加载`HSTS`

有些浏览器允许您提交站点的`HSTS`到浏览器中。你可以在`HTTP`头部中按照如下的方式加入`preload`字段：

```javascript
app.use(
  helmet.hsts({
    maxAge: 31536000, // 必须至少是一年
    includeSubDomains: true, // 必须包含子域
    preload: true
  })
);
```
