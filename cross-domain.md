## 允许跨域政策（X-Permitted-Cross-Domain-Policies）

`Helmet`的`crossdomain`中间件可以阻止`Adobe Flash`和`Adobe Acrobat`在你的网站上加载内容。

### 攻击

`Adobe Flash`和`Adobe Acrobat`可以在你的网站上加载其他网站的内容（换句话说就是跨域）。这可能会导致隐私数据泄露或者是额外的带宽使用。

### HTTP 头部

HTTP 的头部字段`X-Permitted-Cross-Domain-Policies`可以告诉客户端像`Flash`和`Acrobat`插件可以使用什么样的跨域策略。如果你比希望他们从你的服务器加载说句，可以将此字段的值设置为`none`：

```
X-Permitted-Cross-Domain-Policies: none
```

如果`Flash`从你的服务器加载一些内容，并且发现有上面所示的头部字段信息，它就知道它不应该从你的服务器加载数据了。

此头部字段还可以有其他值，这需要你创建一个`crossdomain.xml`文件定义你的跨域策略。关于此，你可以阅读下面列出的内容。

阅读更多：

- [X-Permitted-Cross-Domain-Policies](https://www.owasp.org/index.php/OWASP_Secure_Headers_Project#xpcdp)

- [Adobe 跨域配置指南](https://www.adobe.com/devnet-docs/acrobatetk/tools/AppSec/xdomain.html)

### 代码

`Helmet`的`crossdomain`中间件可以帮助你设置`X-Permitted-Cross-Domain-Policies`字段。

你可以将`crossdomain`模块作为`Helmet`的一部分使用：

```javascript
const helmet = require('helmet');

app.use(helmet.permittedCrossDomainPolicies());
```

你也可以将其作为一个单独的模块使用：

```javascript
const permittedCrossDomainPolicies = require('helmet-crossdomain');

app.use(permittedCrossDomainPolices());
```

默认此字段的值为`none`。你也可以为此字段设置其他值：

```javascript
app.use(permittedCrossDomainPolicies({ permittedPolices: 'master-only' }));

app.use(permittedCrossDomainPolicies({ permittedPolicies: 'by-content-type' }));

app.use(permittedCrossDomainPolicies({ permittedPolicies: 'all' }));
```
