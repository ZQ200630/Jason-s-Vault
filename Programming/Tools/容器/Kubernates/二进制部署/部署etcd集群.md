---
tags: Kubernates
---
# 部署etcd集群

### 生成证书配置文件

```bash
#运维主机

vim /opt/certs/ca-config.json

{
    "signing": {
        "default": {
            "expiry": "438000h"
        },
        "profiles": {
            "server": {
                "expiry": "438000h",
                "usages": [
                    "signing",
                    "key encipherment",
                    "server auth"
                ]
            },
            "client": {
                "expiry": "438000h",
                "usages": [
                    "signing",
                    "key encipherment",
                    "client auth"
                ]
            },
            "peer": {
                "expiry": "438000h",
                "usages": [
                    "signing",
                    "key encipherment",
                    "server auth",
                    "client auth"
                ]
            }
        }
    }
}
```

### 创建etcd请求文件

```bash
#运维主机

vi etcd-peer-csr.json

{
    "CN": "k8s-etcd",
    "hosts": [
        "10.4.7.11",
        "10.4.7.12",
        "10.4.7.21",
        "10.4.7.22"
    ],
    "key": {
        "algo": "rsa",
        "size": 2048
    },
    "names": [
        {
            "C": "CN",
            "ST": "beijing",
            "L": "beijing",
            "O": "od",
            "OU": "ops"
        }
    ]
}
```

### 签发证书

```bash
#运维主机

cfssl gencert -ca=ca.pem -ca-key=ca-key.pem -config=ca-config.json -profile=peer etcd-peer-csr.json |cfssl-json -bare etcd-peer
```

### 部署etcd

```bash
#结点主机

cd /opt
mkdir src
cd src

useradd -s /sbin/nologin -M etcd
id etcd
下载etcd-v3.3.12-linux-amd64.tar.gz，放在src包下
tar xfv etcd-v3.3.12-linux-amd64.tar.gz -C /opt
cd /opt
ll
mv etcd-v3.3.12-linux-amd64 etcd-v3.3.12
ln -s /opt/etcd-v3.3.12/ /opt/etcd

mkdir -p /opt/etcd/certs /data/etcd/etcd-server /data/logs/etcd-server
```

### 拷贝证书

```bash
cd /opt/etcd/certs
scp hdss7-200.host.com:/opt/certs/ca.pem .
scp hdss7-200.host.com:/opt/certs/etcd-peer.pem .
scp hdss7-200.host.com:/opt/certs/etcd-peer-key.pem .
```

### 配置启动文件

```bash
#第一行，第三行，第四行，第六行需要更改

vim /opt/etcd/etcd-server-startup.sh

#!/bin/sh
/opt/etcd/etcd --name etcd-server-7-12 \
       --data-dir /data/etcd/etcd-server \
       --listen-peer-urls https://10.4.7.12:2380 \
       --listen-client-urls https://10.4.7.12:2379,http://127.0.0.1:2379 \
       --quota-backend-bytes 8000000000 \
       --initial-advertise-peer-urls https://10.4.7.12:2380 \
       --advertise-client-urls https://10.4.7.12:2379,http://127.0.0.1:2379 \
       --initial-cluster  etcd-server-7-12=https://10.4.7.12:2380,etcd-server-7-21=https://10.4.7.21:2380,etcd-server-7-22=https://10.4.7.22:2380 \
       --ca-file /opt/etcd/certs/ca.pem \
       --cert-file /opt/etcd/certs/etcd-peer.pem \
       --key-file /opt/etcd/certs/etcd-peer-key.pem \
       --client-cert-auth  \
       --trusted-ca-file /opt/etcd/certs/ca.pem \
       --peer-ca-file /opt/etcd/certs/ca.pem \
       --peer-cert-file /opt/etcd/certs/etcd-peer.pem \
       --peer-key-file /opt/etcd/certs/etcd-peer-key.pem \
       --peer-client-cert-auth \
       --peer-trusted-ca-file /opt/etcd/certs/ca.pem \
       --log-output stdout

cd /opt/etcd
chmod +x etcd-server-startup.sh 
chown -R etcd:etcd /opt/etcd-v3.3.12/
ll  [[查看user和group是否改为etcd]] etcd
chown -R etcd:etcd /data/etcd/
chown -R etcd:etcd /data/logs/etcd-server/
```

### 安装supervisor

```bash
yum install supervisor
systemctl start supervisord
systemctl enable supervisord
```

### 配置supervisor

```bash
mkdir /etc/supervisord.d

# 第一行需要更改

vim /etc/supervisord.d/etcd-server.ini # 这里的文件名称自定义

[program:etcd-server-7-12] 
command=/opt/etcd/etcd-server-startup.sh
numprocs=1                 
directory=/opt/etcd
autorestart=true 
startsecs=30
startretries=3
exitcodes=0,2
stopsignal=QUIT
stopwaitsecs=10      
user=etcd
redirect_stderr=true
stdout_logfile = /data/logs/etcd-server/etcd.stdout.log
stdout_logfile_maxbytes=64MB
stdout_logfile_backups=4
stdout_capture_maxbytes=1MB
stdout_events_enabled=false
```

### supervisor命令

```bash
[[echo_supervisord_conf]] > /etc/supervisord.conf #生成配置文件

supervisord : 启动supervisor
supervisorctl reload :修改完配置文件后重新启动supervisor
supervisorctl status :查看supervisor监管的进程状态
supervisorctl start all | 进程名 ：启动全部或某进程
supervisorctl stop all | 进程名 ：停止全部或某进程
supervisorctl stop all：停止进程，注：start、restart、stop都不会载入最新的配置文件。
supervisorctl update：根据最新的配置文件，启动新配置或有改动的进程，配置没有改动的进程不会受影响而重启
supervisorctl status
```

### 启动

```bash
systemctl start supervisord
systemctl enable supervisord
supervisorctl update
supervisorctl reload

[[一定要保证所有的文件的所有者都是etcd]]!!!!
```

### 检查结点状态

```bash
cd /opt/etcd
./etcdctl cluster-health
./etcdctl member list
```