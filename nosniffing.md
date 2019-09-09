## 不要嗅探 MIME 类型

`noSniff`中间件可以阻止浏览器嗅探 MIME 类型，嗅探 MIME 类型可能会引起安全性问题。`noSniff`中间件是通过设置 HTTP 头部的`X-Content-Type-Options`字段来阻止浏览器嗅探 MIME 类型的。

### 攻击

MIME 类型是一种用来确定你查看的是什么类型文件的方法。PNG 图片的 MIME 类型为`image/png`;JSON 文件的 MIME 类型为`application/json`;JavaScript 文件的 MIME 类型为`text/javascript`。当浏览器加载文件时，它会读取 HTTP 响应头部的`Content-Type`字段来确定加载的是什么类型的文件。

假如你的浏览器要加载如下的文件：

```html
<script src="https://example.com/my-javascript"></script>
```

那么浏览器会从`example.com`服务器加载`my-javascript`文件。如果`example.com`服务器在发送给浏览器的 HTTP 响应头中加入了`Content-Type: text/javascript`，那么你的浏览器就会将`my-javascript`文件作为 JavaScript 执行。

如果`my-javascript`文件实际上是一个`Content-Type`为`text/html`的 HTML 页面呢？如果你的浏览器做了如`MIME嗅探`之类的事情，它就可以去查看文件的内容到底是不是 JavaScript，从而决定是否需要以 JavaScript 的方式执行此文件内容。这也就意味着服务器在返回 HTTP 响应时可以设置错误的`Content-Type`响应头，但是 JavaScript 依然可以被执行。

MIME 嗅探可能会成为攻击的一个方向。用户可能会上传一个`.jpg`格式的文件，但此文件实际上是一段 HTML 内容。当其他用户访问此文件时，可能会导致浏览器运行并渲染此 HTML 页面，此页面可能会包含一些恶意的 JavaScript 脚本。可能目前为止遭受此种类型攻击最严重的就是`Rosetta Flash`了，它允许加载一些恶意的 Flash 插件而不是真正的数据。

阅读更多：

- [维基百科对 MIME 类型的定义](https://en.wikipedia.org/wiki/Media_type)

- [MIME 嗅探：功能还是漏洞？](https://blog.fox-it.com/2012/05/08/mime-sniffing-feature-or-vulnerability/)

- [IE 浏览器中的 MIME 嗅探会引起跨站脚本攻击](http://www.h-online.com/security/features/Risky-MIME-sniffing-in-Internet-Explorer-746229.html)

- [使用 Rosetta Flash 滥用 JSONP](https://blog.miki.it/2014/7/8/abusing-jsonp-with-rosetta-flash/)

- [谷歌浏览器中的跨源资源阻塞](https://developers.google.com/web/updates/2018/07/site-isolation)

### HTTP 头部

HTTP 头部的`X-Content-Type-Options`字段可以告诉浏览器不要去嗅探 MIME 类型。当此字段被设置为`nosniff`时，浏览器就不会去嗅探 MIME 类型，浏览器会严格按照服务器在`Content-Type`字段中所指定的类型来执行文件，如果资源与服务器所指定的类型不同，则会将其阻塞，不去执行。

### 代码

`noSniff`中间件会将对每一个 HTTP 请求的响应设置`X-Content-Type-Options:nosniff`。

你可以将此中间件作为`Helmet`的一部分使用：

```javascript
const helmet = require('helmet');

app.use(helmet.noSniff());
```

你也可以将其作为一个单独的模块使用：

```javascript
const noSniff = require('dont-sniff-mimetype');

app.use(noSniff());
```

默认情况下，`Helmet`已经将此头部字段包含在 HTTP 响应头中了。
