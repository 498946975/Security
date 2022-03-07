## phpstudy安装dvwa
### 1、dvwa官网
```shell script
https://github.com/digininja/DVWA
```
### 2、下载源码，加入到phpstudy的www目录下
![image](https://github.com/498946975/Security/blob/master/images/waf_07.png)
### 3、修改dvwa的配置文件，将数据库的用户名和密码，都改为root
```yaml
C:\phpstudy_pro\WWW\dvwa\config
```
![image](https://github.com/498946975/Security/blob/master/images/waf_08.png)
### 4、修改phpstudy的php配置文件
```yaml
C:\phpstudy_pro\Extensions\php\php5.4.45nts
```
![image](https://github.com/498946975/Security/blob/master/images/waf_09.png)
### 5、登陆dvwa，初始化，然后测试sql注入
#### 设置为low模式
![image](https://github.com/498946975/Security/blob/master/images/waf_10.png)
![image](https://github.com/498946975/Security/blob/master/images/waf_11.png)
