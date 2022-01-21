### 1、这个数据库利用好，利于渗透
```yaml
这个是mysql自带的库；
存储整个mysql数据库里面的，所有的
库信息（SCHEMATA），
表信息（TABLES），
列信息（COLUMNS）
```
### 2、查询某个数据库的所有表名称
```sql
SELECT
	TABLE_NAME 
FROM
	information_schema.`TABLES` 
WHERE
	TABLE_SCHEMA = "test_fastapi";
```
![image](https://github.com/498946975/Security/blob/master/images/sql_22.png)