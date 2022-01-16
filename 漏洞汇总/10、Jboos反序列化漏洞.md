###1、Java序列化:把Java对象转换为字节序列的过程。

Java反序列化:指把字节序列恢复为Java对象的过程。

### 2、漏洞：转化时候，没有做安全校验，执行了反序列化的内容。

### 3、java程序运行的时候，动态加载其他代码执行

### 4、影响版本：5.X～6.X

### 5、运行漏洞环境
docker-compose.yaml
```yaml
version: '2'
services:
  jboss:
    image: vulhub/jboss:as-6.1.0
    ports:
      - "9990:9990"
      - "8083:8080"
```

6、反序列化工具jboss反序列化_CVE-2017-12149.jar
![files](https://github.com/498946975/Security/blob/master/files/jboss%E5%8F%8D%E5%BA%8F%E5%88%97%E5%8C%96_CVE-2017-12149.jar)
![image](https://github.com/498946975/Security/blob/master/images/image-20211229123707309.png)


