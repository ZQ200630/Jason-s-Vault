---
tags: Kubernates
---
# 部署kubelet

### 创建kubelet请求文件

```bash
vim /opt/certs/kubelet-csr.json

{
    "CN": "k8s-kubelet",
    "hosts": [
    "127.0.0.1",
    "10.4.7.10",
    "10.4.7.21",
    "10.4.7.22",
    "10.4.7.23",
    "10.4.7.24",
    "10.4.7.25",
    "10.4.7.26",
    "10.4.7.27",
    "10.4.7.28"
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
cfssl gencert -ca=ca.pem -ca-key=ca-key.pem -config=ca-config.json -profile=server kubelet-csr.json | cfssl-json -bare kubelet
```

### 拷贝证书

```bash
[[将运维主机上的证书复制到/opt/kubernetes-v1]].16.2/server/bin/cert中
```

### 配置kubectl

```bash
[[HDSS7-21]]
mkdir /opt/kubernetes/server/conf
cd /opt/kubernetes/server/conf

#以下命令均直接运行

kubectl config set-cluster myk8s \
  --certificate-authority=/opt/kubernetes/server/bin/cert/ca.pem \
  --embed-certs=true \
  --server=https://10.4.7.10:7443 \
  --kubeconfig=kubelet.kubeconfig
  
  
kubectl config set-credentials k8s-node \
  --client-certificate=/opt/kubernetes/server/bin/cert/client.pem \
  --client-key=/opt/kubernetes/server/bin/cert/client-key.pem \
  --embed-certs=true \
  --kubeconfig=kubelet.kubeconfig 
  
  
kubectl config set-context myk8s-context \
  --cluster=myk8s \
  --user=k8s-node \
  --kubeconfig=kubelet.kubeconfig
  
  
kubectl config use-context myk8s-context --kubeconfig=kubelet.kubeconfig

vim k8s-node.yaml

apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: k8s-node
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: system:node
subjects:
- apiGroup: rbac.authorization.k8s.io
  kind: User
  name: k8s-node

kubectl create -f k8s-node.yaml
#检查是否绑定完成
kubectl get clusterrolebinding k8s-node -o yaml
```

### 拷贝配置文件

```bash
[[将/opt/kubernetes/server/conf/kubelet]].kubeconfig 拷到其它节点对应目录下
```

### 拷贝镜像

```bash
docker pull kubernetes/pause
docker tag imgId harbor.od.com/public/pause:latest
docker push harbor.od.com/public/pause:latest
```

### 制作启动脚本

```bash
vi /opt/kubernetes/server/bin/kubelet.sh
[[cgroup-driver]] 要与docker 的 daemon保持一致
[[hostname-overide]]需要根据情况更改
  
#!/bin/sh
./kubelet \
  --anonymous-auth=false \
  --cgroup-driver systemd \
  --cluster-dns 192.168.0.2 \
  --cluster-domain cluster.local \
  --runtime-cgroups=/systemd/system.slice \
  --kubelet-cgroups=/systemd/system.slice \
  --fail-swap-on="false" \
  --client-ca-file ./cert/ca.pem \
  --tls-cert-file ./cert/kubelet.pem \
  --tls-private-key-file ./cert/kubelet-key.pem \
  --hostname-override hdss7-21.host.com \
  --image-gc-high-threshold 20 \
  --image-gc-low-threshold 10 \
  --kubeconfig ./conf/kubelet.kubeconfig \
  --log-dir /data/logs/kubernetes/kube-kubelet \
  --pod-infra-container-image harbor.od.com/public/pause:latest \
  --root-dir /data/kubelet
  
  chmod +x /opt/kubernetes/server/bin/kubelet.sh
  mkdir -p /data/logs/kubernetes/kube-kubelet /data/kubelet
```

### 配置supervisor

```bash
vim /etc/supervisord.d/kube-kubelet.ini

[program:kube-kubelet-7-21]
command=/opt/kubernetes/server/bin/kubelet.sh     
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
stdout_logfile=/data/logs/kubernetes/kube-kubelet/kubelet.stdout.log   
stdout_logfile_maxbytes=64MB                      
stdout_logfile_backups=4                          
stdout_capture_maxbytes=1MB                      
stdout_events_enabled=false

supervisorctl update
```

### 检查是否配置成功

```bash
#启动日志
tail -fn 200 /data/logs/kubernetes/kube-kubelet/kubelet.stdout.log
[[注意：apiserver启动条件是所有的etcd]]服务器全部启动

kubectl get nodes

kubectl label node hdss7-21.host.com node-role.kubernetes.io/master=
kubectl label node hdss7-21.host.com node-role.kubernetes.io/node=
kubectl label node hdss7-22.host.com node-role.kubernetes.io/master=
kubectl label node hdss7-22.host.com node-role.kubernetes.io/node=
```