## 无缓存

`nocache`中间件旨在通过设置若干 HTTP 头部来禁用浏览器缓存。

### 攻击

此模块并不是用来防止特定类型的攻击。它是用来阻止用户访问缓存版本的文件（比如旧的 JavaScript 文件）。

例如，假如你有一个前端 Web APP 用来提供一些 JavaScript 文件。有一天，你发现你的 JavaScript 库有一个缺陷，于是你要升级并更新你的网站。不幸的是，有些用户可能依然能够访问旧的、缓存的代码，这些代码依然是有缺陷的。

缓存有很多优点，但它可能会使用户获得过时的版本文件。

### HTTP 头部

此模块可以控制 HTTP 头部的四个字段：

- `Cache-Control` 字段具有很多指令，比如`Cache-Control:max-age=86400`将会告诉浏览器缓存的有效期是 10 天，在这 10 天内，浏览器将会从缓存中拉取信息。将此头部字段设置为`Cache-Control: no-store, no-cache, must-revalidate, proxy-revalidate`将会清除缓存。
- `Surrogate-Control`字段是 CDN 所需要的 HTTP 头部信息，你可以用它来告诉中间缓存避开缓存。
- `Pragma`是一个旧的 HTTP 头部字段。设置`Pragma: no-cache`将会告诉支持该字段的浏览器停止使用缓存进行响应。相对`Cache-Control`而言，它可能会有一些更好的特性，但旧浏览器对它的支持比较好。
- `Expire`字段用来指定内容何时过期，如果将此字段的值设置为 0，则会高速浏览器内容立即过期。换句话说，就是不对内容进行缓存。

### 代码

可以使用`Helmet`提供的`noCache`模块来设置上面提到的四个 HTTP 头部字段。

你可以将`noCache`模块作为`Helmet`的一部分来使用：

```javascript
const helmet = require('helmet');
app.use(helmet.noCache());
```

你也可以单独使用此模块：

```javascript
const noCache = require('nocache');
app.use(noCache());
```
