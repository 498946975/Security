### 1、漏洞原理
```
用户输入的特殊字符，如javascript代码，在前端浏览器进行执行
```
### 2、写一个form表单
```html
<form action="action漏洞.php" method="post"> 
    <p>姓名: <input type="text" name="name" /></p> 
    <p>年龄: <input type="text" name="age" /></p> 
    <p><input type="submit" /></p>
</form>
```
### 3、上面form表单的action，action漏洞.php
```HTML
你好，<?php echo ($_REQUEST['name']); ?>。<br>
你 <?php echo (int)$_REQUEST['age']; ?> 岁了。<br>
_REQUEST的数据是<?php echo $_REQUEST ?>

<!-- 如果没有htmlspecialchars，那么在前端则会执行js代码，实现攻击漏洞 -->
<!-- 如果并不关心请求数据的来源，也可以用超全局变量 $_REQUEST，它包含了所有 GET、POST、COOKIE 和 FILE 的数据。 -->
<!-- <script>alert(document.cookie)</script>  在用户输入字符串的时候，拿到cookie-->
<!-- 也可以使用js的事件原理，执行js代码 -->
```

### 4、在html上输入js代码，并执行
![image](https://github.com/498946975/Security/blob/master/images/image-20211219153958813.png)
![image](https://github.com/498946975/Security/blob/master/images/image-20211219154033537.png)

5、将用户输入的内容，必须进行html实体编码

```HTML
你好，<?php echo htmlspecialchars($_POST['name']); ?>。
你 <?php echo (int)$_POST['age']; ?> 岁了。

<!-- htmlspecialchars: html的实体编码，显示特殊的字符
     如果没有htmlspecialchars，则会执行js代码，造成xss漏洞 -->
<!-- 反射性xss漏洞： -->
<!-- xss漏洞原理：就是在页面上，执行了用户输入的js代码 -->
```

6、在进行html实体编码之后，再输入js代码，就不会在前端执行了
![image](https://github.com/498946975/Security/blob/master/images/image-20211219154325121.png)