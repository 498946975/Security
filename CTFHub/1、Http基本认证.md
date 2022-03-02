## 1、http基本认证
```yaml
在HTTP中，基本认证（英语：Basic access authentication）
是允许http用户代理（如：网页浏览器）在请求时，提供 用户名 和 密码 的一种方式。
在进行基本认证的过程里，请求的HTTP头字段会包含Authorization字段，形式如下： 
Authorization: Basic <凭证>，
该凭证是用户和密码的组和的base64编码。
```
![image](https://github.com/498946975/Security/blob/master/CTFHub/images/001.png)
## 2、靶机环境
![image](https://github.com/498946975/Security/blob/master/CTFHub/images/003.png)
### 2.1 密码字典
![image](https://github.com/498946975/Security/blob/master/CTFHub/images/004.png)
### 2.2 提交用户名密码
![image](https://github.com/498946975/Security/blob/master/CTFHub/images/005.png)
## 2、base64解码
![image](https://github.com/498946975/Security/blob/master/CTFHub/images/002.png)
## 3、暴力破解密码
### 3.1 发送到intruder
![image](https://github.com/498946975/Security/blob/master/CTFHub/images/006.png)
### 3.2 加载payload
![image](https://github.com/498946975/Security/blob/master/CTFHub/images/007.png)
### 3.3 增加payload前缀，用户名是admin
![image](https://github.com/498946975/Security/blob/master/CTFHub/images/008.png)
### 3.4 增加编码特征，取消url编码
![image](https://github.com/498946975/Security/blob/master/CTFHub/images/009.png)
## 4、攻击即可