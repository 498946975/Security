## XSS防御
### 1、xss攻击2大要素
```yaml
1. 攻击者提交恶意代码。 
2. 浏览器执行恶意代码。
```
### 2、httponly
```yaml
许多 XSS 攻击的目的就是为了获取用户的cookie，将重要的 cookie 标记为http only，
这样的话当浏览器向服务端 发起请求时就会带上cookie字段，但是在脚本中却不能访问 cookie，
这样就避免了XSS攻击利用JavaScript的 document.cookie获取cookie。
严格来说，HttpOnly 并非阻止 XSS 攻击，而是能阻止 XSS 攻击后的 Cookie 劫持攻击。
```
### 3、csp内容安全策略
```yaml
1、本质上，就是一个白名单
2、规定了浏览器只能够执行特定来源的代码
```
#### 3.1 可以使用 Content-Security-Policy HTTP头部 来指定你的策略
#### 3.2 分类
```yaml
Content-Security-Policy
配置好并启用后，不符合 CSP 的外部资源就会被阻止加载。
```
```yaml
Content-Security-Policy-Report-Only 表示不执行限制选项，只是记录违反限制的行为。
它必须与`report-uri`选项配合使用
```
#### 3.3 在http头部使用
```shell script
"Content-Security-Policy:" 策略 
"Content-Security-Policy-Report-Only:" 策略
```
