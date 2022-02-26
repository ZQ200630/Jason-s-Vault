---
tags: Kubernates
---
# 部署Flanneld

### 部署Flanneld

```bash
# /opt/src 把flannel压缩包放里面
mkdir /opt/flannel-v0.11.0
tar xf flannel-v0.11.0-linux-amd64.tar.gz -C /opt/flannel-v0.11.0/
ln -s /opt/flannel-v0.11.0/ /opt/flannel
cd /opt/flannel
mkdir cert/
#  把ca证书， client证书考过去
```

### 配置

```bash
#第二行主义更改
vim /opt/flannel/subnet.env

FLANNEL_NETWORK=172.7.0.0/16
FLANNEL_SUBNET=172.7.21.1/24
FLANNEL_MTU=1500
FLANNEL_IPMASQ=false
```

### 启动配置

```bash
vim /opt/flannel/flanneld.sh

#!/bin/sh
./flanneld \
  --public-ip=10.4.7.21 \
  --etcd-endpoints=https://10.4.7.12:2379,https://10.4.7.21:2379,https://10.4.7.22:2379 \
  --etcd-keyfile=./cert/client-key.pem \
  --etcd-certfile=./cert/client.pem \
  --etcd-cafile=./cert/ca.pem \
  --iface=ens33 \
  --subnet-file=./subnet.env \
  --healthz-port=2401
  
chmod +x /opt/flannel/flanneld.sh
mkdir -p /data/logs/flanneld

[[etcd]]网络配置
[[只需要一台etcd]]搞好即可
cd /opt/etcd
./etcdctl set /coreos.com/network/config '{"Network": "172.7.0.0/16", "Backend": {"Type": "host-gw"}}'
```

### 配置supervisor

```bash
vi /etc/supervisord.d/flannel.ini

[program:flanneld-7-21]
command=/opt/flannel/flanneld.sh                   
numprocs=1                                                           
directory=/opt/flannel                       
autostart=true                                                       
autorestart=true                                                     
startsecs=30                                                     
startretries=3                                                       
exitcodes=0,2                                                        
stopsignal=QUIT                                                      
stopwaitsecs=10                                                      
user=root                                                            
redirect_stderr=true                                               
stdout_logfile=/data/logs/flanneld/flanneld.stderr.log
stdout_logfile_maxbytes=64MB                                         
stdout_logfile_backups=4                                             
stdout_capture_maxbytes=1MB                                          
stdout_events_enabled=false

supervisorctl update
```

### 优化

```bash
### 更改模式
### ./etcdctl rm /coreos.com/network/config
### ./etcdctl set /coreos.com/network/config '{"Network": "172.7.0.0/16", "Backend": {"Type": "VxLAN"}}'
### ./etcdctl set /coreos.com/network/config '{"Network": "172.7.0.0/16", "Backend": {"Type": "VxLAN", "Directrouting":true}}'

优化：
yum install iptables-services -y
systemctl start iptables
systemctl enable iptables
iptables-save |grep -i postrouting
iptables -t nat -D POSTROUTING -s 172.7.21.0/24 ! -o docker0 -j MASQUERADE
iptables -t nat -I POSTROUTING -s 172.7.21.0/24 ! -o docker0 ! -d 172.7.0.0/16 -j MASQUERADE
iptables-save | grep -i reject
iptables -t filter -D INPUT -j REJECT --reject-with icmp-host-prohibited
iptables -t filter -D FORWARD -j REJECT --reject-with icmp-host-prohibited
iptables-save | grep -i reject
iptables-save > /etc/sysconfig/iptables

kubectl logs -f podname
[[查看出网ip]]
```