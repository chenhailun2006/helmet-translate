## 隐藏 X-Powered-By

`Hide Powered-By`的中间件会移除 HTTP 头部字段中的`X-Powered-By`字段，这样攻击者就更难发现你的网站使用了哪些易受攻击的技术。

### 攻击

如果黑客知道你在使用`Express`和`Node`，那么他们就可以利用`Express`和`Node`中的已知漏洞对你的应用进行攻击。`Express`（以及其他的一些 web 技术，比如 PHP）会为每次请求设置 HTTP 头部中`X-Powered-By`字段，用来只是服务器使用的什么技术。比如`Express`会将`X-Powered-By`字段的值设置为 **Express**，用来表示你的服务器是使用的`Express`。

### 修复

只需要移除 HTTP 头部中的`X-Powered-By`字段就可以了。

不过，对于一个有毅力的黑客而言，即便在 HTTP 头部中没有发现`X-Powered-By`字段，他也不会立即放弃对你的网站的攻击。他们会寻找其他线索来查明你是否是使用的`Node`，或者干脆给你来个一连串的攻击，看是否能攻击成功。简单的移除`X-Powered-By`字段并不意味着没有可以利用的漏洞，它只是会稍微降低攻击速度或阻止一些比较懒散的黑客的攻击。

移除`X-Powered-By`字段还会略微有一点性能的提升，因为发送的数据报更小了 😄

阅读更多：

- [移除头部对提升安全性没有太大帮助](https://github.com/expressjs/express/pull/2813#issuecomment-159270428)

### 代码

```javascript
const helmet = require('helmet');

app.use(helmet());
```

如果你是单独使用`Helmet`的头部，这里有一个更简单的使用方式：

```javascript
app.disable('x-powered-by');
```

如果你仍然想要使用此模块，你可以将其作为`Helmet`的一部分来使用：

```javascript
app.use(helmet.hidePoweredBy());
```

或者单独使用：

```javascript
const hidePoweredBy = require('hide-powered-by');

app.use(hidePoweredBy());
```

你还可以设置一个假的值来迷惑黑客。比如，你的服务器实际上使用的是`Express`，但是你将其设置为`PHP`来迷惑黑客：

```javascript
app.use(helmet.hidePoweredBy({ setTo: 'PHP 4.2.0' }));
```
