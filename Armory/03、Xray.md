### 1、正向扫描和反向扫描
```yaml
1、正向: 主动触发扫描
2、反向: 被动接受流量，并扫描
```
### 2、正向脚本扫描
#### 不建议生产中使用，因为类似于爬虫，会被封堵
```shell script
./xray webscan --basic-crawler http://172.16.120.252:8081 --html-output vuln.html
```
![image](https://github.com/498946975/Security/blob/master/images/image-20211228100841452.png)
### 3、反向被动扫描，使用http代理和ca证书
#### 3.1 生成xray证书
```shell script
┌──(rootli)-[~/xray]
└─# ./xray genca


____  ___.________.    ____.   _____.___.
\   \/  /\_   __   \  /  _  \  \__  |   |
 \     /  |    _  _/ /  /_\  \  /   |   |
 /     \  |    |   \/    |    \ \____   |
\___/\  \ |____|   /\____|_   / / _____/
      \_/       \_/        \_/  \/

Version: 1.8.2/79e7dd56/COMMUNITY

CA certificate ca.crt and key ca.key generated
                                                                                                                                                                                                                                                                                                                                                                                                     
┌──(rootli)-[~/xray]
└─# ls
ca.crt  ca.key  config.yaml  xray  xray_linux_amd64.zip
```
#### 3.2 导入证书
![image](https://github.com/498946975/Security/blob/master/images/xray_1.png)
![image](https://github.com/498946975/Security/blob/master/images/xray_2.png)
#### 3.3 修改xray代理，扫描目标地址（172.16.120.252是靶机）
```shell script
┌──(rootli)-[~/xray]
└─# vim config.yaml
```
```shell script
###修改以下内容
hostname_allowed: [172.16.120.252]
```
#### 3.4 在浏览器上设置代理服务器和端口
FireFox用FoxyProxy作为代理插件，如下图
![image](https://github.com/498946975/Security/blob/master/images/image-20211228105930761.png)
Chrome用Proxy SwitchyOmega作为代理插件，如下图
![image](https://github.com/498946975/Security/blob/master/images/xray_3.png)
#### 3.5 在xray安装的主机上，监听7777端口
```shell script
./xray webscan --listen 0.0.0.0:7777 --html-output test.html	
```
#### 3.6 然后就是在第3.3步骤监听的网站上，点点点了
https://github.com/498946975/Security/blob/master/files/test.html