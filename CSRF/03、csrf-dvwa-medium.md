## csrf中级别测试，验证referer
### 1、php源码
```shell script
<?php

if( isset( $_GET[ 'Change' ] ) ) {
    // Checks to see where the request came from
    if( stripos( $_SERVER[ 'HTTP_REFERER' ] ,$_SERVER[ 'SERVER_NAME' ])!=false ) { # 判断http中的HTTP_REFERER 是否包含host参数的SERVER_NAME
        // Get input
        $pass_new  = $_GET[ 'password_new' ];
        $pass_conf = $_GET[ 'password_conf' ];

        // Do the passwords match?
        if( $pass_new == $pass_conf ) {
            ...
        }
        else {
            // Issue with passwords matching
            echo "<pre>Passwords did not match.</pre>";
        }
    }
    else {
        // Didn't come from a trusted source
        echo "<pre>That request didn't look correct.</pre>";
    }

    ((is_null($___mysqli_res = mysqli_close($GLOBALS["___mysqli_ston"]))) ? false : $___mysqli_res);
}

?>
```
```yaml
1、HTTP_REFERER：表示发送请求的来源
    如：http://172.16.120.252:8080/vulnerabilities/csrf/
2、SERVER_NAME：表示配置默认的二级域名，不会是当前的域名
    如：172.16.120.252
```
### 2、http_referer
![image](https://github.com/498946975/Security/blob/master/images/csrf_02.png)
### 3、http_referer绕过的方法
#### 3.1 随便申请一个域名
```shell script
liuxiang.com
```
#### 3.2 子域名可以自己定义
假如下面这个连接是dvwa的域名
```shell script
http://dvwa.liuxiang.com/vulnerabilities/csrf/
```
自己的子域名可以是
```shell script
http://dvwa.liuxiang.com.liuxiang.com/vulnerabilities/csrf/
```
#### 3.3 自己的域名，完全包含正常的域名