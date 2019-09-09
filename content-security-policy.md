## 内容安全策略

内容安全策略模块用于设置响应头的`Content-Security-Policy`字段，这有助于防止恶意注入 JavaScript、CSS 和插件等。

### 攻击

如果黑客能够在你的网页上植入一些内容，那么他们就能实施很多恶意的行为。

最有名的跨站脚本攻击（XSS）就是黑客通过将一些恶意的 JavaScript 代码植入到你的网页上实现的。如果我能在你的网页上运行 JavaScript 代码，那么我就能够做很多对你的网站或用户不利的事情，比如盗取权限 Cookie、记录用户访问记录等。

即使黑客不能在你的网页上执行 JavaScript 代码，他们依然可以做一些其他的事情。比如，如果能够在你的网页上放置一个很小而又透明的图片，那么就能够知道你的网站有多少流量。如果能够在你的网站上运行一些易受攻击的浏览器插件，比如 Flash，那就能够利用这些插件的缺陷做一些对你的网站不利的事情。

CSP 模块不会指定去阻止哪种类型的攻击。它要做的事情是帮你达到一个目的：你不希望任何人在你的网页上放你不期望的东西。

### 头部

关于注入攻击的一个比较尴尬的问题是浏览器并不知道哪些是好的，哪些是不好的。浏览器如何知道哪些是合法的 JavaScript 文件哪些是恶意的 JavaScript 文件呢？在大多数情况下，浏览器是不知道的，除非你定义了内容安全策略。

大多数现代浏览器支持一个叫做`Content-Security-Policy`的头部字段，它实际上是允许出现在页面上的内容的白名单。你可以在此字段上列出 JavaScript、CSS、图片、插件等，被列出的内容都是被允许的。

假如有一个网页，该网页没有链接任何外部资源，那么你可以设置如下的内容安全策略头：

```
Content-Security-Policy: default-src 'self'
```

上面的内容告诉浏览器“只允许加载自己域名下的资源”。比如你运营着域名为 example.com 的网站，用户在访问 `http://example.com/my-javascript.js`时，可以正常工作，但是用户在此网站上访问`http://evil.com/evil.js`时，就不允许加载了。

现在假如你允许通过 CDN 加载 CSS 库 Bootstrap 的内容，那么你可以设置如下的内容安全策略：

```
Content-Security-Policy: default-src 'self'; style-src 'self' maxcdn.bootstrapcdn.com
```

现在我们将`self`和`maxcdn.bootstrapcdn.com`加入了白名单。那么用户可以通过这两个地方获取 CSS 资源，而不能通过其他地方获取。甚至用户都不能在页面上加载 URL `maxcdn.bootstrapcdn.com` 下的 JavaScript 或图片资源，只能加载样式文件。

### 指令参考

响应头的`Content-Security-Policy`字段的值是由一个或多个指令（见下表）构成的，多个指令之间使用分号“;”隔开。

|      指令       |          值举例           | 描述                                                                                                                                                          |
| :-------------: | :-----------------------: | :------------------------------------------------------------------------------------------------------------------------------------------------------------ |
|   default-src   |  'self' cdn.example.com   | `default-src`是加载 JavaScript、图片、CSS、字体文件、AJAX 请求、Frames、H5 媒体文件等的默认策略                                                               |
|   script-src    |   'self' js.exmaple.com   | 定义有效的 JavaScript 文件资源                                                                                                                                |
|    style-src    |  'self' css.example.com   | 定义有效的样式文件资源                                                                                                                                        |
|     img-src     |  'self' img.example.com   | 定义有效的图片资源                                                                                                                                            |
|   connect-src   |          'self'           | 定义可用的 XMLHttpRequest（AJAX）， WebSocket 或 EventSource，如果不在白名单内，浏览器会返回 400 状态码                                                       |
|    font-src     |     font.example.com      | 定义有效的字体资源                                                                                                                                            |
|   object-src    |          'self'           | 定义有效的插件资源，比如&lt;object&gt;, &lt;embed&gt;或&lt;applet&gt;                                                                                         |
|    media-src    |     media.example.com     | 定义有效的音视频文件，比如 H5 的&lt;audio&gt;, &lt;video&gt;元素                                                                                              |
|    child-src    |          'self'           | 定义允许通过&lt;frame&gt;和&lt;iframe&gt;加载的有效的资源                                                                                                     |
|     sandbox     | allow-forms allow-scripts | 对请求的资源激活沙箱。沙箱适用同源策略，禁止弹出菜单、插件和脚本的执行。                                                                                      |
|   report-uri    |     /some-report-uri      | 指示浏览器将策略失败的报告发布到此 URI。你还可以在响应头添加`-Report-Only`字段来指示浏览器只发送报告（不阻塞任何东西）                                        |
|   form-action   |          'self'           | 定义可用于 HTML 中&lt;form&gt;元素的 action 属性的有效资源                                                                                                    |
| frame-ancestors |          'none'           | 为&lt;frame&gt;、&lt;iframe&gt;、&lt;object&gt;、&lt;embed&gt;和&lt;applet&gt;等嵌入式元素定义有效的资源，将此指令设置为 none 基本上等于 X-Frame-Options:DENY |
|  plugin-types   |      application/pdf      | 定义插件通过&lt;object&gt;、&lt;embed&gt;可调用的 MIME 类型的资源。要加载&lt;applet&gt;，必须指定该指令的值包含`application/x-java-applet`                    |

### 代码

Helmet 的 `csp` 模块可以用来设置内容安全策略。你可以将此模块作为 Helmet 的一部分来使用：

```javascript
const helmet = require("helmet");

app.use(
 helmet.contentSecurityPolicy({
  directives: {
   defaultSrc: ["'self'"],
   styleSrc: ["'self'", "maxcdn.bootstrapcdn.com"]
  }
 })
);
```

你也可以将其作为一个单独的模块使用：

```javascript
const csp = require("helmet-csp");

app.use(
 csp({
  directives: {
   defaultSrc: ["'self'"],
   styleSrc: ["'self'", "maxcdn.bootstrapcdn.com"]
  }
 })
);
```

### 指令
所有的内容安全策略指令（比如`default-src`, `style-src`）都需要放置在`directives`配置项下，如下所示：
```javascript
app.use(csp({
    directives: {
        defaultSrc: ["'self'", 'default.com'],
        scriptSrc: ["'self'", "'unsafe-inline'"],
        sandbox: ['allow-forms', 'allow-scripts'],
        reportUri: '/report-violation',
        objectSrc: ["'none'"],
        upgradeInsecureRequests: true,
        workerSrc: false
    }
}))
```
指令可以是短横线形式（比如`script-src`）或小驼峰形式（比如`scriptSrc`），它们是等价的。

### 违反内容安全策略
如果你指定了`reportUri`，浏览器将会通过POST请求将任何违反内容安全策略的内容发送至你的服务器，下面给出一个使用Express路由处理这些报告的例子：
```javascript
// 你首先需要使用JSON解析器
app.use(bodyParser.json({
    type: ['json', 'application/csp-report']
}))

app.post('/report-violation', (req, res) => {
    if(req.body) {
        console.log('CSP Violation:', req.body);
    } else {
        console.log('CSP Violation: No data received!')
    }
    res.status(204).end()
})
```
并不是所有的浏览器都以同样的方式发送违反内容安全策略的消息，它们可能需要一些额外的工作。

注：如果你在使用CSRF模块（比如`csurf`），你可能在没有一个有效的CSRF令牌情况下对这些违法内容安全策略的消息处理时会出现一些问题。解决方式是将CSP相关的路由放在csurf中间件之上。

`csp`模块的`reportOnly`配置项可以控制是否在响应头中添加`Content-Security-Policy-Report-Only`字段。此字段会指示浏览器将违反内容安全策略的消息发送到`reportUri`（如果指定了的话），但不会阻塞任何资源的加载。
```javascript
app.use(csp({
    directives: {
        // ...
    },
    reportOnly: true
}))
```
你还可以为`reportOnly`字段设置一个函数，从而可以动态的决定是否使用`reportOnly`模式。此函数以request对象和response对象作为参数，并且返回一个布尔值。

```javascript
app.use(csp({
    directives: {
        // ...
    },
    reportOnly: (req, res) => req.query.cspmode === 'debug'
}))
```

### 浏览器嗅探
默认情况下，`csp`模块会查看请求头的`User-Agent`字段，并且根据探测到的浏览器不同发送不同的响应头。例如，25版本之前的Chrome浏览器会使用`X-Webkit-CSP`字段控制内容安全策略。如果没有检测到浏览器，则`csp`模块会按照2.0规范设置所有的响应头。

要禁用浏览器嗅探并假定为现代浏览器，则设置配置项`browserSniff`的值为`false`。
```javascript
app.use(csp({
    directives: {
        // ...
    },
    browserSniff: false
}))
```
要设置所有的响应头，包括旧的响应头，可以将配置项`setAllHeaders`设置为`true`。注意，这将会根据`User-Agent`的内容改变响应头各字段的值。你可以通过使用`browserSniff: false`来禁用此功能。

```javascript
app.use(csp({
    directives: {
        // ...
    },
    setAllHeaders: true
}))
```
旧版的Android浏览器可能会有很多Bug。默认情况下会设置为`false`。
```javascript
app.use(csp({
    directives: {
        // ...
    },
    disabledAndroid: true
}))
```