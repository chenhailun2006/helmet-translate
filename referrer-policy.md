## 访问来源策略

`Referrer Policy`模块可以通过设置 HTTP 头部的`Referrer-Policy`字段来控制 HTTP 头部的`Referer`字段的行为。

### 攻击

HTTP 头部的`Referer`字段通常是由 web 浏览器设置的，用来告诉服务器请求的来源。例如，如果你点击`example.com/index.html`链接会将你带到`wikipedia.org`页面，那么维基百科的服务器将会在请求头中看到`Referer:example.com`的信息。

这可能会产生隐私泄露隐患，因为网站服务器可以得知用户来源。

### HTTP 头部

新的 HTTP 头部字段`Referrer-Policy`可以让作者控制浏览器如何设置`Referrer`首部。

比如，当支持`Referrer-Policy`首部的浏览器看到下面的首部信息时，在发送请求时就不会携带任何关于`Referer`的信息：

```
Referrer-Policy: no-referrer
```

`Referrer-Policy`首部还可以有其他的指令，比如`same-origin`，只会对同一页面来源的请求发送`Referer`信息。

```
Referrer-Policy: same-origin
```

### 指令

| 指令                               | 描述                                                                                                                                                        |
| ---------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------- |
| no-referrer                        | 整个`Referer`首部会被移除。访问来源信息不随着请求一起发送。                                                                                                 |
| no-referrer-when-downgrade(默认值) | 在没有指定任何策略的情况下用户代理的默认行为。在同等安全级别的情况下，引用页面的地址会被发送（HTTPS->HTTPS），但是在降级的情况下不会被发送（HTTPS->HTTP）。 |
| origin                             | 在任何情况下，仅发送文件的源作为引用地址。                                                                                                                  |
| origin-when-cross-origin           | 对于同源的请求，会发送完整的 URL 作为引用地址，但是对于非同源请求仅发送文件的源                                                                             |
| same-origin                        | 对于同源的请求会发送引用地址，但是对于非同源的请求则不发送引用地址信息                                                                                      |
| strict-origin                      | 在同等安全级别的情况下，发送文件的源作为引用地址（HTTPS->HTTPS），但是在降级的情况下不会发送（HTTPS->HTTP）。                                               |
| strict-origin-when-cross-origin    | 对于同源的请求，会发送完整的 URL 作为引用地址；在同等安全级别的情况下，发送文件的源作为引用地址(HTTPS->HTTPS)；在降级的情况下不发送此首部 (HTTPS->HTTP)。   |
| unsafe-url                         | 无论是同源请求还是非同源请求，都发送完整的 URL（移除参数信息之后）作为引用地址                                                                              |

### 代码

`Helmet`的`Referrer-Policy`模块是一个很简单的中间件，用来设置 HTTP 头部的`Referrer-Policy`字段。

你可以将此模块作为`Helmet`的一部分来使用：

```javascript
cosnt helmet = require('helmet');
app.use(helmet.referrerPolicy({
    policy: 'same-origin'
}));
```

你也可以将`Referrer-Policy`模块作为一个单独的模块来使用：

```javascript
const referrerPolicy = require('referrer-policy');

app.use(
 referrerPolicy({
  policy: 'no-referrer',
 })
);
```
