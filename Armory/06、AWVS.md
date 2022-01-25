### 1、下载awvs镜像
```shell script
docker pull registry.cn-hangzhou.aliyuncs.com/876500/awvs:v1
```
### 2、执行lesions脚本
```shell script
#!/usr/bin/env bash

set -ex

Echo_c() {
  echo -e "\033[1;33m$1\033[0m"
}

check() {
  Echo_c "Starting cracking"
  curl -s -o awvs_listen.zip https://www.fahai.org/usr/uploads/2021/09/734242510.zip
  docker cp awvs_listen.zip awvs:/awvs/acunetix/
  docker exec -it awvs /bin/bash -c "unzip -o /awvs/acunetix/awvs_listen.zip -d /home/acunetix/.acunetix/data/license/"
  docker exec -it awvs /bin/bash -c "chmod 444 /home/acunetix/.acunetix/data/license/license_info.json"
  docker exec -it awvs /bin/bash -c "chown acunetix:acunetix /home/acunetix/.acunetix/data/license/wa_data.dat"
  docker exec -it awvs /bin/bash -c "rm /awvs/acunetix/awvs_listen.zip"
  docker exec -it awvs /bin/bash -c "echo '#!/usr/bin/env bash' > awvs.sh"
  docker exec -it awvs /bin/bash -c "echo 'clear' >> awvs.sh"
  docker exec -it awvs /bin/bash -c "echo 'cat /awvs/acunetix/.hosts >> /etc/hosts' >> awvs.sh"
  docker exec -it awvs /bin/bash -c "echo 'cat /etc/hosts | grep acunetix' >> awvs.sh"
  docker exec -it awvs /bin/bash -c "echo 'su -l acunetix -c /home/acunetix/.acunetix/start.sh' >> awvs.sh"
  docker exec -it awvs /bin/bash -c "chmod 777 /awvs/awvs.sh"
  docker exec -it awvs /bin/bash -c "echo '127.0.0.1 updates.acunetix.com' > /awvs/acunetix/.hosts"
  docker exec -it awvs /bin/bash -c "echo '127.0.0.1 erp.acunetix.com' >> /awvs/acunetix/.hosts"
  docker restart awvs
  rm awvs_listen.zip
  Echo_c "Chech over!"
}

main() {
  Echo_c "Start Install"
  Echo_c "本操作会删除所有名字包含 awvs 的容器，5秒后将执行";sleep 5
  docker pull registry.cn-hangzhou.aliyuncs.com/876500/awvs:v1
  if [ ! -n "$(docker ps -aq --filter name=awvs)" ]; then
    if [ ! -n "$(docker ps -aq --filter publish=3443)" ]; then
      docker run -it -d --name awvs -p 3443:3443 --restart=always registry.cn-hangzhou.aliyuncs.com/876500/awvs:v1;check
      Echo_c "Please visit https://127.0.0.1:3443"
    else
      docker run -it -d --name awvs -p 3444:3443 --restart=always registry.cn-hangzhou.aliyuncs.com/876500/awvs:v1;check
      Echo_c "Please visit https://127.0.0.1:3444"
    fi
  else
    docker rm -f $(docker ps -aq --filter name=awvs)
    docker run -it -d --name awvs -p 3443:3443 --restart=always registry.cn-hangzhou.aliyuncs.com/876500/awvs:v1
    check
    Echo_c "Please visit https://127.0.0.1:3443"
  fi
}
main 
```
![image](https://github.com/498946975/Security/blob/master/images/awvs_1.png)