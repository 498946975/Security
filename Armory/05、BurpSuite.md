# BurpSuite

## 一、安装jdk环境

```
https://www.oracle.com/java/technologies/downloads/#java8-mac
```
![image](https://github.com/498946975/Security/blob/master/images/image-20220108193118152.png)

## 二、在mac上安装BurpSuite

 [BrupsuiteV1.7.31.zip](1月8日/BrupsuiteV1.7.31.zip) 

### 1、解压缩该zip
![image](https://github.com/498946975/Security/blob/master/images/image-20220108192344962.png)
![image-20220108192344962](/Users/oliver/Library/Application Support/typora-user-images/image-20220108192344962.png)

```shell script
Oswald:BrupsuiteV1.7.31 root# pwd
/Users/oliver/Downloads/【马哥教育】网络安全C1课件/1月8日/BrupsuiteV1.7.31
```

### 2、运行BurpKeygen.jar，激活BurpSuite

```shell script
Oswald:BrupsuiteV1.7.31 root# java -jar BurpKeygen.jar 
```
![image](https://github.com/498946975/Security/blob/master/images/image-20220108193202786.png)

点上面的“执行”，用上面的命令，运行burpsuite_pro_v1.7.31.jar
![image](https://github.com/498946975/Security/blob/master/images/image-20220108193809402.png)

注意：复制粘贴的时候，尝试使用Command+C Command+V Control+C Control+V
![image](https://github.com/498946975/Security/blob/master/images/image-20220108193851579.png)

### 3、激活成功之后，如下显示
![image](https://github.com/498946975/Security/blob/master/images/image-20220108191620596.png)
![image](https://github.com/498946975/Security/blob/master/images/image-20220108191645579.png)
![image](https://github.com/498946975/Security/blob/master/images/image-20220108191709667.png)

## 三、以后每次启动BurpSuite的方法

```shell script
Oswald:BrupsuiteV1.7.31 root# java -jar BurpLoader.jar 
```

## 四、配置ca证书，可以扫描https的流量

### 1、在BurpSuite设置代理
![image](https://github.com/498946975/Security/blob/master/images/image-20220109113100672.png)

### 2、在浏览器访问127.0.0.1:8888，并进行下载cacert.cer
![image](https://github.com/498946975/Security/blob/master/images/image-20220109113205515.png)

### 3、Chrome导入证书
![image](https://github.com/498946975/Security/blob/master/images/image-20220109114605255.png)
![image](https://github.com/498946975/Security/blob/master/images/image-20220109114616786.png)

新增刚刚下载的证书
![image](https://github.com/498946975/Security/blob/master/images/image-20220109114646497.png)

修改信任选项和等级即可
![image](https://github.com/498946975/Security/blob/master/images/image-20220109114705131.png)

## 五、对流量进行拦截
![image](https://github.com/498946975/Security/blob/master/images/image-20220109162532843.png)
![image](https://github.com/498946975/Security/blob/master/images/image-20220109162600480.png)

### 1、修改拦截到的http发出的请求内容

勾选默认的optinons
![image](https://github.com/498946975/Security/blob/master/images/image-20220109170720476.png)
![image](https://github.com/498946975/Security/blob/master/images/image-20220109162644981.png)

```yaml
Raw：这是视图主要显示web请求的raw格式
Params：自动识别参数，这个视图主要显示客户端请求的参数信息、包括GET或者POST请求的参数、Cookie参数
Headers：这个视图显示的信息和Raw的信息类似
Hex：这个视图显示Raw的16进制内容
```
![image](https://github.com/498946975/Security/blob/master/images/image-20220109162742853.png)

### 2、修改拦截到的http请求的返回值的信息

勾选默认的optinons
![image](https://github.com/498946975/Security/blob/master/images/image-20220109171028995.png)
![image](https://github.com/498946975/Security/blob/master/images/image-20220109171131939.png)

修改拦截到的信息
![image](https://github.com/498946975/Security/blob/master/images/image-20220109171241117.png)
![image](https://github.com/498946975/Security/blob/master/images/image-20220109171309840.png)

## 六、对手机浏览器流量进行拦截修改

### 1、首先，手机和电脑必须在同一个Wi-Fi下

### 2、设置BurpSuite
![image](https://github.com/498946975/Security/blob/master/images/image-20220109174235822.png)

### 3、设置手机代理
![image](https://github.com/498946975/Security/blob/master/images/image-20220109174342868.png)
![image](https://github.com/498946975/Security/blob/master/images/image-20220109174404912.png)

### 4、修改http请求的返回包的信息
![image](https://github.com/498946975/Security/blob/master/images/image-20220109174604049.png)
![image](https://github.com/498946975/Security/blob/master/images/image-20220109175555420.png)



## 七、 拦截高亮

### 1、准备访问一个网站，如“百度”

### 2、拦截请求，给“百度一下”这几个字高亮显示
![image](https://github.com/498946975/Security/blob/master/images/image-20220110203403905.png)

### 3、在访问的HTTP history中，高亮显示
![image](https://github.com/498946975/Security/blob/master/images/image-20220110203527277.png)

## 8、客户端请求消息拦截
![image](https://github.com/498946975/Security/blob/master/images/image-20220110204135555.png)

```
表示当前的规则与其他规则是与的方式(And)还是或的方式(Or)共存
```
![image](https://github.com/498946975/Security/blob/master/images/image-20220110204209138.png)

```
域名、IP地址、协议、请求方法、 URL、文件类型、参数, cookies, 头部或者内容, 状态码, MIME类型, HTML⻚面的title
```
![image](https://github.com/498946975/Security/blob/master/images/image-20220110204259370.png)

```
Matches：匹配
Does not matches：不匹配
```
![image](https://github.com/498946975/Security/blob/master/images/image-20220110204434734.png)


## 9、通过正则表达式匹配，并修改拦截的信息
![image](https://github.com/498946975/Security/blob/master/images/image-20220111201812073.png)
![image](https://github.com/498946975/Security/blob/master/images/image-20220111201901672.png)
![image](https://github.com/498946975/Security/blob/master/images/image-20220111201920635.png)

## 10、http历史记录过滤
![image](https://github.com/498946975/Security/blob/master/images/image-20220111202936177.png)
![image](https://github.com/498946975/Security/blob/master/images/image-20220111203012853.png)

## 11、设置探索站点过滤

### 只看哪些网站的信息
![image](https://github.com/498946975/Security/blob/master/images/image-20220111212242235.png)

### 设置目标地图，和代理的历史过滤配合使用
![image](https://github.com/498946975/Security/blob/master/images/image-20220111212522900.png)
![image](https://github.com/498946975/Security/blob/master/images/image-20220111212545123.png)
![image](https://github.com/498946975/Security/blob/master/images/image-20220111212603846.png)

## 12、网站地图
![image](https://github.com/498946975/Security/blob/master/images/image-20220111213738205.png)

## 13、查看网站分析报告
![image](https://github.com/498946975/Security/blob/master/images/image-20220111215840002.png)
![image](https://github.com/498946975/Security/blob/master/images/image-20220111215856711.png)

## 14、主动“爬”网站信息
![image](https://github.com/498946975/Security/blob/master/images/image-20220111222621483.png)

也可以通过详细的设置来搜索
![image](https://github.com/498946975/Security/blob/master/images/image-20220111222954582.png)
![image](https://github.com/498946975/Security/blob/master/images/image-20220111223044538.png)

## 15、Scanner扫描漏洞

### 1、注意，要先有“站点地图”才能进行扫描

### 2、在站点地图上进行分支扫描也可以
![image](https://github.com/498946975/Security/blob/master/images/image-20220112212307435.png)

### 3、扫描的进度和结果，可以在Scanner中进行查看
![image](https://github.com/498946975/Security/blob/master/images/image-20220112212357512.png)

### 4、扫描子目录的时候，可以排除一些图片，css，js文件等
![image](https://github.com/498946975/Security/blob/master/images/image-20220112212607243.png)

### 5、导出漏洞报告
![image](https://github.com/498946975/Security/blob/master/images/image-20220112213046079.png)
![image](https://github.com/498946975/Security/blob/master/images/image-20220112213125465.png)

## 16、暴力破解

### 1、选择暴力破解的url加入
![image](https://github.com/498946975/Security/blob/master/images/image-20220112230645163.png)

### 2、选择需要破解的参数
![image](https://github.com/498946975/Security/blob/master/images/image-20220112230900908.png)

### 3、确认需要破解上面的password的参数，进行枚举破解
![image](https://github.com/498946975/Security/blob/master/images/image-20220112231210964.png)

### 2、通过长度来判断是否破解成功
![image](https://github.com/498946975/Security/blob/master/images/image-20220112232007414.png)

### 4、对请求结果进行过滤
![image](https://github.com/498946975/Security/blob/master/images/image-20220112232730206.png)

### 5、waf绕过对方法
![image](https://github.com/498946975/Security/blob/master/images/image-20220113093433212.png)
![image](https://github.com/498946975/Security/blob/master/images/image-20220113093451679.png)

### 6、爆破字典

可以自动生成密码字典，爆破验证码
![image](https://github.com/498946975/Security/blob/master/images/image-20220113094038277.png)

### 7、笛卡尔乘积爆破
![image](https://github.com/498946975/Security/blob/master/images/image-20220113095719988.png)

对每个位置，进行单独的赋值，进行爆破
![image](https://github.com/498946975/Security/blob/master/images/image-20220113095804214.png)
![image](https://github.com/498946975/Security/blob/master/images/image-20220113095818856.png)