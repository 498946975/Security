## 应用场景
### 1、分享
```yaml
通过url 地址分享文章，例如如下地址: http://share.magedu.com/index.php?url=http://127.0.0.1
通过url参数的获取来实现点击链接的时候跳到指定的分享文章。如果在此功能中没有对目标地址的范围做过滤与限制则就存在着SSRF漏洞。
```
![image](https://github.com/498946975/Security/blob/master/images/ssrf_04.png)
### 2、图片加载与下载
```yaml
通过URL地址加载或下载图片：http://image.magedu.com/image.php?image=http://127.0.0.1
图片加载存在于很多的编辑器中，编辑器上传图片处，有的是加载远程图片到服务器内。
还有一些采用了加载远程图片的形式，本地文章加载了设定好的远程图片服务器上的图片地址，如果没对加载的参数做限制可能造成SSRF。
```
### 3、图片、文章收藏功能
```yaml
http://title.magedu.com/title?title=http://title.magedu.com/as52ps63de
例如title参数是文章的标题地址，代表了一个文章的地址链接，请求后返回文章是否保存，收藏的返回信息。
如果保存，收藏功能采用了此种形式保存文章，则在没有限制参数的形式下可能存在SSRF。
```
### 4、漏洞绕过
#### 4.1 "@"绕过
```yaml
可以尝试采用http基本身份认证的方式绕过，http://www.xxx.com@www.xxc.com。 
在对@解析域名中，不同的 处理函数存在处理差异，例如: 
http://www.aaa.com@www.bbb.com@www.ccc.com，
在PHP的parse_url中会 识别www.ccc.com，
而libcurl则识别为www.bbb.com。
```
#### 4.2 短网址绕过
```shell script
https://www.985.so/
```
![image](https://github.com/498946975/Security/blob/master/images/ssrf_02.png)
#### 4.3 进制绕过
```shell script
https://www.sojson.com/hexconvert.html
```
![image](https://github.com/498946975/Security/blob/master/images/ssrf_03.png)
```yaml
16进制：0x开头
8进制：0开头
```
#### 4.4 利用句号绕过
```yaml
127。0。0。1 >>> 127.0.0.1
```
