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
#### 4.1 如果是int开头的字符串，则会隐式转换第一个int，后面的都忽略
![image](https://github.com/498946975/Security/blob/master/images/sql_26.png)
### 5、测试是不是数字型的sql注入的三个步骤
#### 5.1 加单引号，语句报错，证明不是string类型的
```sql
select * from user where id=1'
```
#### 5.2 加 "and 1=1"，语句执行正常
```sql
select * from user where id=1 and 1=1
```
```shell script
superadmin 2021-12-11 14:51:53
```
#### 5.3 没有输出的结果，和上面的输出结果不一致，但是也能正常执行
```sql
select * from user where id=1 and 1=2
```