---
tags: Kubernates
---
# 部署kube-controller-manager

### 配置启动文件

```bash
vim /opt/kubernetes/server/bin/kube-controller-manager.sh

#!/bin/sh
./kube-controller-manager \
--cluster-cidr 172.7.0.0/16 \
--leader-elect true \
--log-dir /data/logs/kubernetes/kube-controller-manager \
--master http://127.0.0.1:8080 \
--service-account-private-key-file ./cert/ca-key.pem \
--service-cluster-ip-range 192.168.0.0/16 \
--root-ca-file ./cert/ca.pem \
--v 2

chmod +x /opt/kubernetes/server/bin/kube-controller-manager.sh
mkdir /data/logs/kubernetes/kube-controller-manager
```

```bash
vi /opt/kubernetes/server/bin/kube-scheduler.sh

#!/bin/sh
./kube-scheduler \
--leader-elect true \
--log-dir /data/logs/kubernetes/kube-scheduler \
--master http://127.0.0.1:8080 \
--v 2

chmod +x /opt/kubernetes/server/bin/kube-scheduler.sh
mkdir -p /data/logs/kubernetes/kube-scheduler
```

### 配置supervisor

```bash
vim /etc/supervisord.d/kube-controller-manager.ini

[program:kube-controller-manager-7-21]
command=/opt/kubernetes/server/bin/kube-controller-manager.sh
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
stdout_logfile=/data/logs/kubernetes/kube-controller-manager/controller.stdout.log  
stdout_logfile_maxbytes=64MB                                                      
stdout_logfile_backups=4                                                          
stdout_capture_maxbytes=1MB                                                       
stdout_events_enabled=false

supervisorctl update
```

```bash
vi /etc/supervisord.d/kube-scheduler.ini
[[/etc/supervisord]].d/kube-scheduler.ini
[program:kube-scheduler-7-21]
command=/opt/kubernetes/server/bin/kube-scheduler.sh                     
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
stdout_logfile=/data/logs/kubernetes/kube-scheduler/scheduler.stdout.log 
stdout_logfile_maxbytes=64MB                                             
stdout_logfile_backups=4                                                 
stdout_capture_maxbytes=1MB                                              
stdout_events_enabled=false

supervisorctl update
```

### 建立kubectl软链接

```bash
ln -s /opt/kubernetes/server/bin/kubectl /usr/bin/kubectl
```

### 检查是否配置成功

```bash
kubectl get cs
```