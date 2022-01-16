### 1、先连接数据库+异常处理
```HTML
 <?php
$servername = "172.16.120.44:3306"; //mysql的地址 
$username = "root";
$password = "123";
$dbname = "test_fastapi";
try {
    $conn = new PDO("mysql:host=$servername;dbname=$dbname", $username, $password); 
    echo "连接成功\r";
} catch (PDOException $e) { 
    echo $e->getMessage();
}
```

### 2、写一个用户输入的form

```HTML
<form action="nocompile.php" method="POST">
    <div style="width:100%;text-align:center">
        <label for="username" class="sr-only">请输入需要查询的用户信息</label>
        <input type="text" id="username" class="form-control" placeholder="Input Username" required="" name="username" />
        <br />
        <button value="查询">查询</button>
    </div>
</form>
```

### 3、写上面form应用的nocompile.php

```HTML
<?php
$servername = "172.16.120.44:3306"; //mysql的地址 
$username = "root";
$password = "123";
$dbname = "test_fastapi";
try {
    $conn = new PDO("mysql:host=$servername;dbname=$dbname", $username, $password); 
    echo "连接成功\r\n";
    $user_input = $_REQUEST['username'];    //获取前端传入的数据
    $sql_base = "select * from user where username='" . $user_input . "'";
    echo $sql_base;?><br \>
<?php
    foreach ($conn->query($sql_base) as $row) {
        print $row['username'] . "\t";
        print $row['create_time'] . "\t"; 
?>
<br \>
<?php
    }
} catch (PDOException $e) { 
    echo $e->getMessage();
}
?>
```

### 4、常规查询方法
![image](https://github.com/498946975/Security/blob/master/images/image-20211219112259052.png)
![image](https://github.com/498946975/Security/blob/master/images/image-20211219112311589.png)

### 5、sql注入的查询方法
![image](https://github.com/498946975/Security/blob/master/images/image-20211219112433181.png)
![image](https://github.com/498946975/Security/blob/master/images/image-20220105091942403.png)
### 6、注入的方法，sql语句如下：
```sql
select * from user where username='1' or '1'='1';
```

### 7、解决方式：sql预编译，implant.php

```HTML
<form action="compile.php" method="POST">
    <div style="width:100%;text-align:center">
        <label for="username" class="sr-only">请输入需要查询的用户信息</label>
        <input type="text" id="username" class="form-control" placeholder="Input Username" required="" name="username" />
        <br />
        <button value="查询">查询</button>
    </div>
</form>
```

```HTML
<?php
$servername = "172.16.120.44:3306"; //mysql的地址 
$username = "root";
$password = "123";
$dbname = "test_fastapi";
try {
    $conn = new PDO("mysql:host=$servername;dbname=$dbname", $username, $password); 
    echo "连接成功\r\n";?><br /><?php
    $user_input = $_REQUEST['username'];    //获取前端传入的数据
    $sql_base = $conn->prepare("select * from user where username=?"); 
    $sql_base->bindParam(1,$user_input);
    $sql_base->execute();
    $res = $sql_base->fetchAll(); 
    foreach ($res as $row) {
        print $row['username'] . "\t";
        print $row['create_time'] . "\t"; 
    }
} catch (PDOException $e) { 
    echo $e->getMessage();
}
?>
```
### 8、面试的关键字：
```
1、用户输入
2、拼接
3、预编译
```
### 9、预编译的原理
```
预编译的功能是mysql提供的，提前编译sql语句，将所有的用户输入当作数据，而非语法。
在sql语句执行之前就确定sql语义，保证用户输入的内容只当作变量执行，而不是sql语句的执行。
```