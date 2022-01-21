### 1、官网地址
```shell script
https://cn.gobies.org/
```
### 2、非常方便，集成了很多工具，如subDomainBrute，FOFA，Xray，Shodan，MSF等
![image](https://github.com/498946975/Security/blob/master/images/goby_1.png)

### 3、在Goby中使用MSF
#### 3.1 在本机系统上安装msf，安装教程如下
```shell script
https://github.com/rapid7/metasploit-framework/wiki/Nightly-Installers
```
```shell script
curl https://raw.githubusercontent.com/rapid7/metasploit-omnibus/master/config/templates/metasploit-framework-wrappers/msfupdate.erb > msfinstall && \
  chmod 755 msfinstall && \
  ./msfinstall
```
#### 3.2 在Goby中，设置msf的启动路径
```shell script
/opt/metasploit-framework/bin/msfconsole
```
![image](https://github.com/498946975/Security/blob/master/images/goby_2.png)
#### 3.3 扫描一台主机的漏洞，如有有msf可以渗透的（如：永恒之蓝），显示如下
![image](https://github.com/498946975/Security/blob/master/images/goby_3.png)

#### 3.4 点击MSF按钮，进行攻击即可
![image](https://github.com/498946975/Security/blob/master/images/goby_4.png)

