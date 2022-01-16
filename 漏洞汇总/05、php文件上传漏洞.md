### 1、php代码如下
#### 1.1 提交文件的form表单
```html
<form enctype="multipart/form-data" action="loadfile.php" method="POST">
    <input type="hidden" name="MAX_FILE_SIZE" value="30000" />
    <!--前端限制文件大小，可以绕过，可以不用前端上传，直接访问接口上传也行 -->
    Send this file: <input type="file" name="userfile" />
    <input type="submit" value="Send File">
</form>
```
#### 1.2 上面form表单，应用的action，loadfile.php
```yaml
<?php
$uploaddir = '/Users/oliver/Documents/Php/study/12月18日练习/';   //文件上传的目录
$uploadfile = $uploaddir . basename($_FILES['userfile']['name']);     // 获取上传的文件名称
echo $_FILES['userfile']['name']; //全局变量，文件名成功，usefile=form表单提交的name名称
echo '<pre>';
if (move_uploaded_file($_FILES['userfile']['tmp_name'], $uploadfile)) { //上传文件到临时目录，然后移动到uploadfile的路径下
    echo "File is valid, and was successfully uploaded.\n"; 
} else {
    echo "Possible file upload attack!\n"; 
}
echo 'Here is some more debugging info:'; 
print_r($_FILES);//打印文件到内容
print "</pre>"; 
?>
```
### 2、使用上面的代码，上传一个一句话木马文件
![image](https://github.com/498946975/Security/blob/master/images/image-20211218215737717.png)
![image](https://github.com/498946975/Security/blob/master/images/image-20211218215809612.png)

### 3、执行在postman执行shell.php
跳转到存储shell.php的目录，执行shell.php，使用post请求，给a进行赋值，即可执行一句话木马
![image](https://github.com/498946975/Security/blob/master/images/image-20211218220327913.png)
shell.php一句话木马，还可以操作服务器，可以执行系统命令
![image](https://github.com/498946975/Security/blob/master/images/image-20211218220601582.png)

### 4、利用一句话木马的条件
```yaml
1、可以上传php文件
2、服务器有php环境，可以执行php代码
3、上传的php文件可以访问（大部分可以做到），重要的是知道你上传的php文件的路径
```
### 5、防御措施
```yaml
1、黑名单：限制上传类型，禁止上传php文件；
2、白名单：文件后缀名限制，一般用白名单限制：jpg,png,gif等类型；
3、文件上传目录下的所有文件，都没有可执行权限，可以在后端做（上传成功之后，直接在后端修改权限）；
4、不返回上传文件路径。
5、修改上传文件的文件名，随机文件名，可以在后端操作
```