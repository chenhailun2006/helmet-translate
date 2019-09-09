## IE No Open

`Helmet`的`ieNoOpen`中间件通过设置 HTTP 头部字段`X-Download-Options`来阻止 IE 浏览器在你的网站上下文环境中执行下载操作。

### 攻击

此攻击只会对老版本的 IE 浏览器有影响。

一些 web 应用会支持不可信 HTML 的下载服务。例如，你可以允许用户上传和下载 HTML 文件。

默认情况下，老版本的 IE 浏览器允许在你网站的上下文环境中打开这些 HTML 文件，这也就意味着不可信的 HTML 页面可以在你的网站上下文环境中执行恶意的操作。更多信息请参阅[这篇博客](https://blogs.msdn.microsoft.com/ie/2008/07/02/ie8-security-part-v-comprehensive-protection/)。

### 头部字段

HTTP 头部字段`X-Download-Options`可以设置为`noopen`。这可以阻止老版本的 IE 浏览器在你的网站上下文环境中去执行下载的恶意 HTML 文件。

### 代码

`Helmet`的`ieNoOpen`中间件可以帮助你将 HTTP 头部字段`X-Download-Options`的值设置为`noopen`。

你可以将`ieNoOpen`模块当作`Helmet`的一部分使用：

```javascript
const helmet = require('helmet');

// 设置"X-Download-Options: noopen"
app.use(helmet.ieNoOpen());
```

你也可以将其作为一个单独的模块使用：

```javascript
const ieNoOpen = require('ienoopen');

// 设置"X-Download-Options: noopen"
app.use(ieNoopen());
```

默认情况下，`Helmet`已经将此头部字段包含在 HTTP 响应头中了。
