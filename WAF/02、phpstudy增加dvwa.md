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
#### 防护日志
![image](https://github.com/498946975/Security/blob/master/images/waf_12.png)
### 5、使用sqlmap进行注入
```shell script
Oswald:sqlmap-dev oliver$ 
python3 sqlmap.py -u "http://172.16.120.165/dvwa/vulnerabilities/sqli/?id=1&Submit=Submit#" -p id --cookie="security=low; PHPSESSID=8bjfl230kl2femeqr4q0e195j2" --batch
```
#### 返回数据，显示受到waf或者ips保护
```shell script
[*] starting @ 11:01:54 /2022-03-07/

[11:01:54] [INFO] testing connection to the target URL
[11:01:55] [INFO] checking if the target is protected by some kind of WAF/IPS
[11:01:55] [INFO] testing if the target URL content is stable
[11:01:55] [INFO] target URL content is stable
[11:01:55] [WARNING] heuristic (basic) test shows that GET parameter 'id' might not be injectable
[11:01:55] [INFO] testing for SQL injection on GET parameter 'id'
[11:01:55] [INFO] testing 'AND boolean-based blind - WHERE or HAVING clause'
[11:01:55] [INFO] testing 'Boolean-based blind - Parameter replace (original value)'
[11:01:55] [INFO] testing 'MySQL >= 5.1 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (EXTRACTVALUE)'
[11:01:55] [INFO] testing 'PostgreSQL AND error-based - WHERE or HAVING clause'
[11:01:56] [INFO] testing 'Microsoft SQL Server/Sybase AND error-based - WHERE or HAVING clause (IN)'
[11:01:56] [INFO] testing 'Oracle AND error-based - WHERE or HAVING clause (XMLType)'
[11:01:56] [INFO] testing 'Generic inline queries'
[11:01:56] [INFO] testing 'PostgreSQL > 8.1 stacked queries (comment)'
[11:01:56] [WARNING] time-based comparison requires larger statistical model, please wait. (done)
[11:01:56] [INFO] testing 'Microsoft SQL Server/Sybase stacked queries (comment)'
[11:01:56] [INFO] testing 'Oracle stacked queries (DBMS_PIPE.RECEIVE_MESSAGE - comment)'
[11:01:56] [INFO] testing 'MySQL >= 5.0.12 AND time-based blind (query SLEEP)'
[11:01:56] [INFO] testing 'PostgreSQL > 8.1 AND time-based blind'
[11:01:56] [INFO] testing 'Microsoft SQL Server/Sybase time-based blind (IF)'
[11:01:56] [INFO] testing 'Oracle AND time-based blind'
it is recommended to perform only basic UNION tests if there is not at least one other (potential) technique found. Do you want to reduce the number of requests? [Y/n] Y
[11:01:56] [INFO] testing 'Generic UNION query (NULL) - 1 to 10 columns'
[11:01:56] [WARNING] GET parameter 'id' does not seem to be injectable
[11:01:56] [CRITICAL] all tested parameters do not appear to be injectable. Try to increase values for '--level'/'--risk' options if you wish to perform more tests. If you suspect that there is some kind of protection mechanism involved (e.g. WAF) maybe you could try to use option '--tamper' (e.g. '--tamper=space2comment') and/or switch '--random-agent'

[*] ending @ 11:01:56 /2022-03-07/
```