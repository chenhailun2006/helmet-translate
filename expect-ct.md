## Expect-CT

HTTP 头部字段`Expect-CT`告诉浏览器期望证书透明。更多关于证书透明和此头部字段的信息请参阅[这篇博客](https://scotthelme.co.uk/a-new-security-header-expect-ct/)。

### 用法

```javascript
const expectCt = require('expect-ct');
//设置Expect-CT: max-age=123
app.use(expectCt({ maxAge: 123 }));

// 设置Expect-CT:enforce;max-age=123
app.use(
 expectCt({
  enforce: true,
  maxAge: 123,
 })
);

// 设置Expect-CT: enforce;max-age=30;report-uri="http://example.com/report"
app.use(
 expectCt({
  enforce: true,
  maxAge: 30,
  reportUri: 'http://example.com/report',
 })
);
```
