### 1、字符串一般需要通过单引号来闭合
### 2、前端代码
```html
<form action="nocompile.php" method="POST">
    <div style="width:100%;text-align:center">
        <label for="username" class="sr-only">请输入需要查询的用户信息</label>
        <input type="text" id="username" class="form-control" placeholder="Input Username" required="" name="username" />
        <br />
        <button value="查询">查询</button> 
    </div>
</form>
```
### 3、后端nocompile.php
```html
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
### 4、测试string的sql语句
![image](https://github.com/498946975/Security/blob/master/images/sql_6.png)
![image](https://github.com/498946975/Security/blob/master/images/sql_7.png)

### 5、重点知识，"#"，后面的内容，就相当于sql语句的注释
![image](https://github.com/498946975/Security/blob/master/images/sql_8.png)
```sql
select * from user where username='1' or 1=1 #'
```
![image](https://github.com/498946975/Security/blob/master/images/sql_9.png)
```yaml
相当于把最后面的引号，给注释了
```





