## DNS 预读取控制

`Helmet`的中间件`dnsPrefetchControl`可以通过设置 HTTP 头部的`X-DNS-Prefetch-Controll`字段来禁用浏览器的 DNS 预读取功能。

### 攻击

当你访问一个 URL 时，浏览器需要查找域名所对应的 IP 地址。比如，将`example.com`解析到`93.184.216.34`。

浏览器可以在用户点击链接之前或者加载资源之前就发起 DNS 请求。这可以提升用户点击链接时的性能，但是对用户有隐私影响。

阅读更多：

- [控制 DNS 预读取](https://developer.mozilla.org/zh-CN/docs/Controlling_DNS_prefetching)

- [Chromium 文档：DNS 预读取](https://dev.chromium.org/developers/design-documents/dns-prefetching)

- [预读取](https://www.keycdn.com/support/prefetching)

### HTTP 头部

HTTP 头部字段`X-DNS-Prefetch-Control`告诉浏览器是否应该做 DNS 预读取操作。即便开启此功能，它可能也不会正常工作——不是所有的浏览器在所有情况下都支持此功能——但是关闭它，所有支持的浏览器都会禁用此功能。

要想禁用 DNS 预读取，可以将字段`X-DNS-Prefetch-Control`的值设置为`off`。要想尝试激活它，可以将值设置为`on`。

大多数浏览器都不会做 DNS 预读取操作，因此对于大多数浏览器来说可以忽略此头部字段。

### 代码

`Helmet`的 DNS 预读取控制中间件可以帮助你设置 HTTP 的头部字段`X-DNS-Prefetch-Control`。

你可以将其作为`Helmet`的一部分使用：

```javascript
const helmet = require('helmet');

// 设置"X-DNS-Prefetch-Control: off"
app.use(helmet.dnsPrefetchControl());
```

你也可以将其作为一个单独的模块使用：

```javascript
const dnsPrefetchControl = require('dns-prefetch-control');

// 设置"X-DNS-Prefetch-Control: off"
app.use(dsnPrefetchControl());

// 设置"X-DNS-Prefetch-Control: off"
app.use(dnsPrefetchControl({ allow: false }));

// 设置"X-DNS-Prefetch-Control: on"
app.use(dnsPrefetchControl({ allow: true }));
```

默认情况下，`Helmet`已经将此头部字段包含在 HTTP 响应头中了。
