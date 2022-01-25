### 1、下载Nessus镜像
```shell script
docker pull registry.cn-hangzhou.aliyuncs.com/876500/nessus:v1
```
### 2、编写docker-compose文件
```yaml
version: '3'
services:
  centos7:
    restart: always
    image: registry.cn-hangzhou.aliyuncs.com/876500/nessus:v1
    container_name: nessus
    stdin_open: true
    privileged: true
    command: /usr/sbin/init
    ports:
      - 8834:8834
```
### 3、启动
```shell script
docker-compose up -d 
```
### 4、无限循环使用
```shell script
账号root
密码123
```
![image](https://github.com/498946975/Security/blob/master/images/nessus_1.png)