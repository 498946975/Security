### 1、测试代码如下
#### 前端
```html
<form action="nocompile.php" method="POST">
    <div style="width:100%;text-align:center">
        <label for="id" class="sr-only">请输入需要查询的用户id</label>
        <input type="text" id="id" class="form-control" placeholder="Input Username" required="" name="id" />
        <br />
        <button value="查询">查询</button>
    </div>
</form>
```
#### 后端nocompile.php
```html
<?php
$servername = "172.16.120.44:3306"; //mysql的地址 
$username = "root";
$password = "123";
$dbname = "test_fastapi";
try {
    $conn = new PDO("mysql:host=$servername;dbname=$dbname", $username, $password); 
    echo "连接成功\r\n";
    $id = $_REQUEST['id'];    //获取前端传入的数据
    $sql_base = "select * from user where id=" . $id . "";
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
### 2、测试数字型的sql语句
![image](https://github.com/498946975/Security/blob/master/images/sql_1.png)
![image](https://github.com/498946975/Security/blob/master/images/sql_2.png)
### 3、查看user表列的信息，id是数字类型的
![image](https://github.com/498946975/Security/blob/master/images/sql_3.png)
### 4、如果这时输入string的"1"，会自动转换成为int类型1，这个功能叫"隐示转换"
![image](https://github.com/498946975/Security/blob/master/images/sql_5.png)
![image](https://github.com/498946975/Security/blob/master/images/sql_4.png)

