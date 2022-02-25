## csrf高级别测试，验证token
### 1、php源码
```shell script
<?php

if( isset( $_GET[ 'Change' ] ) ) {
    // Check Anti-CSRF token
    checkToken( $_REQUEST[ 'user_token' ], $_SESSION[ 'session_token' ], 'index.php' ); # 验证token，对比2个token

    // Get input
    $pass_new  = $_GET[ 'password_new' ];
    $pass_conf = $_GET[ 'password_conf' ];

    // Do the passwords match?
    if( $pass_new == $pass_conf ) {
        // They do!
        $pass_new = ((isset($GLOBALS["___mysqli_ston"]) && is_object($GLOBALS["___mysqli_ston"])) ? mysqli_real_escape_string($GLOBALS["___mysqli_ston"],  $pass_new ) : ((trigger_error("[MySQLConverterToo] Fix the mysql_escape_string() call! This code does not work.", E_USER_ERROR)) ? "" : ""));
        $pass_new = md5( $pass_new );

        // Update the database
        $insert = "UPDATE `users` SET password = '$pass_new' WHERE user = '" . dvwaCurrentUser() . "';";
        $result = mysqli_query($GLOBALS["___mysqli_ston"],  $insert ) or die( '<pre>' . ((is_object($GLOBALS["___mysqli_ston"])) ? mysqli_error($GLOBALS["___mysqli_ston"]) : (($___mysqli_res = mysqli_connect_error()) ? $___mysqli_res : false)) . '</pre>' );

        // Feedback for the user
        echo "<pre>Password Changed.</pre>";
    }
    else {
        // Issue with passwords matching
        echo "<pre>Passwords did not match.</pre>";
    }

    ((is_null($___mysqli_res = mysqli_close($GLOBALS["___mysqli_ston"]))) ? false : $___mysqli_res);
}

// Generate Anti-CSRF token
generateSessionToken();

?>
```
### 2、token的原理，不可预测参数
```yaml
High级别的代码增加了Anti-CSRF token机制，
用户每次访问改密⻚面时，服务器会返回一个随机的token，
向服务器发起请求时，需要参提交token数，
而服务器在收到请求时，会优先检查token，
只有token正确，才会处理客户端请求

每次刷新页面，token都会改变
```
### 3、演示
```shell script
http://172.16.120.252:8080/vulnerabilities/csrf/?password_new=password&password_conf=password&Change=Change&user_token=30469906be0ec4e5f405e8b031e814fe#
```
![image](https://github.com/498946975/Security/blob/master/images/csrf_03.png)
### 4、结合xss漏洞
```yaml
1、获取cookie，把high改成low即可。。。
```