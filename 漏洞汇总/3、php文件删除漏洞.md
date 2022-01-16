### 1、代码如下
```php
<?php
// 从用户目录中删除指定的文件
$username = $_POST['user_submitted_name']; 
$userfile = $_POST['user_submitted_filename']; 
$homedir = "/home/$username";   
unlink ("$homedir/$userfile"); // 删除文件
echo "The file has been deleted!";
?>
```
### 2、漏洞分析
```yaml
1、homedir：在上面的代码中，安全的情况下，指的是某个用户的家目录。
		如：/home/oliver
2、既然$username和$userfile是用户输入的，在不安全的情况下，这两个变量可以是下面的情况
```
![image](https://github.com/498946975/Security/blob/master/images/image-20211218195134797.png)
### 3、漏洞防控和治理
```yaml
1、不可以使用root用户来启动应用；
2、不可以直接用普通用户来删除文件，如果要删除，需要做用户权限控制；
	a）敏感文件只有管理员有权限
	b）用户只能操作自身的文件
	c）检查所有提交的变量，如：变量不能有“../”，
```