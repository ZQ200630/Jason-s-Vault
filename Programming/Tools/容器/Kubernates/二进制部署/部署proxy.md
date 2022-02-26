---
tags: Kubernates
---
# 部署proxy

### 创建proxy请求文件

```bash
vim /opt/certs/kube-proxy-csr.json

{
    "CN": "system:kube-proxy",
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

### 生成证书

```bash
cfssl gencert -ca=ca.pem -ca-key=ca-key.pem -config=ca-config.json -profile=client kube-proxy-csr.json |cfssl-json -bare kube-proxy-client
```

### 拷贝证书

```bash
cd /opt/kubernetes/server/bin/cert/
scp hdss7-200:/opt/certs/kube-proxy-client-key.pem ./
scp hdss7-200:/opt/certs/kube-proxy-client.pem ./
```

### 配置proxy

```bash
cd /opt/kubernetes/server/bin/conf

kubectl config set-cluster myk8s \
  --certificate-authority=/opt/kubernetes/server/bin/cert/ca.pem \
  --embed-certs=true \
  --server=https://10.4.7.10:7443 \
  --kubeconfig=kube-proxy.kubeconfig
  
  kubectl config set-credentials kube-proxy \
  --client-certificate=/opt/kubernetes/server/bin/cert/kube-proxy-client.pem \
  --client-key=/opt/kubernetes/server/bin/cert/kube-proxy-client-key.pem \
  --embed-certs=true \
  --kubeconfig=kube-proxy.kubeconfig
  
  kubectl config set-context myk8s-context \
  --cluster=myk8s \
  --user=kube-proxy \
  --kubeconfig=kube-proxy.kubeconfig
  
  kubectl config use-context myk8s-context --kubeconfig=kube-proxy.kubeconfig
```

### 拷贝配置文件

```bash
[[把kube-proxy]].kubeconfig拷贝到其它主机对应文件夹下
```

### 配置ipvs

```bash
vi /root/ipvs.sh

#!/bin/bash
ipvs_mods_dir="/usr/lib/modules/$(uname -r)/kernel/net/netfilter/ipvs"
for i in $(ls $ipvs_mods_dir|grep -o "^[^.]*")
do
  /sbin/modinfo -F filename $i &>/dev/null
  if [ $? -eq 0 ];then
    /sbin/modprobe $i
  fi
done

chmod +x /root/ipvs.sh
sh /root/ipvs.sh
lsmod |grep ip_vs
```

### 创建启动文件

```bash
vi /opt/kubernetes/server/bin/kube-proxy.sh

#!/bin/sh
./kube-proxy \
  --cluster-cidr 172.7.0.0/16 \
  --hostname-override hdss7-21.host.com \
  --proxy-mode=ipvs \
  --ipvs-scheduler=nq \
  --kubeconfig ./conf/kube-proxy.kubeconfig
  
  
  chmod +x /opt/kubernetes/server/bin/kube-proxy.sh
  mkdir -p /data/logs/kubernetes/kube-proxy
```

### 配置supervisor

```bash
vi /etc/supervisord.d/kube-proxy.ini

[program:kube-proxy-7-21]
command=/opt/kubernetes/server/bin/kube-proxy.sh                     
numprocs=1                                                           
directory=/opt/kubernetes/server/bin                                 
autostart=true                                                       
autorestart=true                                                     
startsecs=30                                                         
startretries=3                                                       
exitcodes=0,2                                                        
stopsignal=QUIT                                                      
stopwaitsecs=10                                                      
user=root                                                            
redirect_stderr=true                                                 
stdout_logfile=/data/logs/kubernetes/kube-proxy/proxy.stdout.log     
stdout_logfile_maxbytes=64MB                                         
stdout_logfile_backups=4                                             
stdout_capture_maxbytes=1MB                                          
stdout_events_enabled=false

supervisorctl update
```

### 安装ipvsadm

```bash
yum install ipvsadm -y
```

### 检查是否配置成功

```bash
tail -fn 200 /data/logs/kubernetes/kube-proxy/proxy.stdout.log
ipvsadm -Ln
kubectl get svc
```