##  2020-3-23
- suricata 中文化 
- 配合天奇把工作接过来

 
```
cd /opt; 

```
    docker run -itd  \
        --name=suri \
        --net=host \
        --cap-add=net_admin --cap-add=sys_nice \
        --privileged=true \
        --restart=always \
         -v /etc/localtime:/etc/localtime:ro \
         -v $(pwd)/suricata.yaml:/etc/suricata/suricata.yaml \
         -v $(pwd)/rules/:/var/lib/suricata/rules/ \
         -v /data/suricata/:/var/log/suricata \
         -v /var/run/:/var/run/ \
        registry.cn-hangzhou.aliyuncs.com/rapid7/suricata:alpine \
         /usr/bin/suricata -c /etc/suricata/suricata.yaml  --pid /var/run/suricata.pid \
          -i eth0  -v

          ## kill -HUP $(ps -ef | grep suricata | grep -v grep | grep '/usr/bin/suricata' | awk '{print $2}')
```


#### 2019
## 　./usr/bin/suricata -c ./etc/suricata/suricata_zxx.yaml -i 

```
rdesktop 10.169.108.111
-f 全屏
-u 用户名
-p 密码
-x lan

-r disk:USB=/mnt/sdb1
-0 附加在控制台
-r sound:[local|off|remote] 声音重定向，将声音重定向服务器到客户端之间。前面设置了-0时无效。

## 开启voice 



## centos7 
```
yum install -y yum-utils device-mapper-persistent-data lvm2 &&  yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo && yum makecache fast && yum -y install docker-ce
```

## ubuntu 
```
#!/bin/bash 


echo >> /etc/apt/sources.list <<- EOF
deb http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse
EOF

apt update && \
apt install apt-transport-https ca-certificates curl software-properties-common && \
curl -fsSL https://mirrors.aliyun.com/docker-ce/linux/ubuntu/gpg | sudo apt-key add - && \
 add-apt-repository "deb [arch=amd64] https://mirrors.aliyun.com/docker-ce/linux/ubuntu $(lsb_release -cs) stable" && \
 apt-get update && apt install -y docker-ce 
```


##  日志轮转

> 肯定会丢失部分日志，但是可以接受
```shell 
#!/bin/bash 

d# apt-get install p7zip-full -y

mv /data/suricata/eve.json /tmp && \
   kill -HUP $(ps -ef | grep suricata | grep -v grep | grep '/usr/bin/suricata' | awk  '{print $2}') && \
    /usr/bin/7z a /data/suricata/old/eve-$(date '+%Y%m%d%H%M').7z  /tmp/eve.json && \
	rm -rf /tmp/eve.json 
```

##  conf.d/supervisor_nat.conf 
```
[program:nat22]
command=/usr/bin/ssh -fNR 17722:localhost:22 -p 22 root@182.61.58.134
user=root
autostart = true
#autorestart=unexpected
autorestart=true
redirect_stderr=true
stdout_logfile=/tmp/supervisor_rnat.log
loglevel=info
logfile_maxbytes=100MB
logfile_backups=3
stdout_logfile_maxbytes=20MB
stdout_logfile_backups=10
stopasgroup=true
killasgroup=true
```