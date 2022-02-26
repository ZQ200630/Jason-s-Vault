---
tags: Kubernates
---
# 部署apiserver

### 创建client使用文件

```python
#运维主机

vim /opt/certs/client-csr.json

{
    "CN": "k8s-node",
    "hosts": [
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

### 生成client证书

```python
cfssl gencert -ca=ca.pem -ca-key=ca-key.pem -config=ca-config.json -profile=client client-csr.json |cfssl-json -bare client
```

### 创建apiserver请求文件

```bash
vim /opt/certs/apiserver-csr.json

{
    "CN": "k8s-apiserver",
    "hosts": [
    	"127.0.0.1",
    	"192.168.0.1",
    	"kubernetes.default",
    	"kubernetes.default.svc",
    	"kubernetes.default.svc.cluster",
    	"kubernetes.default.svc.cluster.local",
    	"10.4.7.10",
    	"10.4.7.21",
    	"10.4.7.22",
    	"10.4.7.23"
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

### 生成apiserver证书

```bash
cfssl gencert -ca=ca.pem -ca-key=ca-key.pem -config=ca-config.json -profile=server apiserver-csr.json |cfssl-json -bare apiserver
```

### 部署apiserver

```bash
cd /opt/src

[[将kubernetes-v1]].16.2-server-linux-amd64.tar.gz放到此处

tar xf kubernetes-v1.16.2-server-linux-amd64.tar.gz -C /opt
cd /opt
mv kubernetes/ kubernetes-v1.16.2
ln -s /opt/kubernetes-v1.16.2/ /opt/kubernetes
cd kubernetes
rm -rf kubernetes-src.tar.gz #删除原代码
cd server
cd bin
rm -f *.tar
rm -f *_tag

cd /opt/kubernetes/server/bin
mkdir cert
cd cert
[[将运维主机/opt/certs中apiserver-key]].pem  apiserver.pem  ca-key.pem  ca.pem  client-key.pem  client.pem拷贝到此文件夹中
cd /opt/kubernetes/server/bin
mkdir conf
cd conf
```

### 编写日志审计文件

```bash
vim /opt/kubernetes/server/bin/conf/audit.yaml

apiVersion: audit.k8s.io/v1beta1 # This is required.
kind: Policy
# Don't generate audit events for all requests in RequestReceived stage.
omitStages:
  - "RequestReceived"
rules:
  # Log pod changes at RequestResponse level
  - level: RequestResponse
    resources:
    - group: ""
      # Resource "pods" doesn't match requests to any subresource of pods,
      # which is consistent with the RBAC policy.
      resources: ["pods"]
  # Log "pods/log", "pods/status" at Metadata level
  - level: Metadata
    resources:
    - group: ""
      resources: ["pods/log", "pods/status"]

  # Don't log requests to a configmap called "controller-leader"
  - level: None
    resources:
    - group: ""
      resources: ["configmaps"]
      resourceNames: ["controller-leader"]

  # Don't log watch requests by the "system:kube-proxy" on endpoints or services
  - level: None
    users: ["system:kube-proxy"]
    verbs: ["watch"]
    resources:
    - group: "" # core API group
      resources: ["endpoints", "services"]

  # Don't log authenticated requests to certain non-resource URL paths.
  - level: None
    userGroups: ["system:authenticated"]
    nonResourceURLs:
    - "/api*" # Wildcard matching.
    - "/version"

  # Log the request body of configmap changes in kube-system.
  - level: Request
    resources:
    - group: "" # core API group
      resources: ["configmaps"]
    # This rule only applies to resources in the "kube-system" namespace.
    # The empty string "" can be used to select non-namespaced resources.
    namespaces: ["kube-system"]

  # Log configmap and secret changes in all other namespaces at the Metadata level.
  - level: Metadata
    resources:
    - group: "" # core API group
      resources: ["secrets", "configmaps"]

  # Log all other resources in core and extensions at the Request level.
  - level: Request
    resources:
    - group: "" # core API group
    - group: "extensions" # Version of group should NOT be included.

  # A catch-all rule to log all other requests at the Metadata level.
  - level: Metadata
    # Long-running requests like watches that fall under this rule will not
    # generate an audit event in RequestReceived.
    omitStages:
      - "RequestReceived"
```

### 配置启动文件

```bash
vim /opt/kubernetes/server/bin/kube-apiserver.sh

#!/bin/sh
./kube-apiserver \
--apiserver-count 2 \
--audit-log-path /data/logs/kubernetes/kube-apiserver/audit-log \
--audit-policy-file ./conf/audit.yaml \
--authorization-mode RBAC \
--client-ca-file ./cert/ca.pem \
--requestheader-client-ca-file ./cert/ca.pem \
--enable-admission-plugins=NamespaceLifecycle,LimitRanger,ServiceAccount,DefaultStorageClass,ResourceQuota,MutatingAdmissionWebhook,ValidatingAdmissionWebhook,ResourceQuota \
--etcd-cafile ./cert/ca.pem \
--etcd-certfile ./cert/client.pem \
--etcd-keyfile ./cert/client-key.pem \
--etcd-servers https://10.4.7.12:2379, https://10.4.7.21:2379, https://10.4.7.22:2379 \
--service-account-key-file ./cert/ca-key.pem \
--service-cluster-ip-range 192.168.0.0/16 \
--service-node-port-range 3000-29999 \
--target-ram-mb=1024 \
--kubelet-client-certificate ./cert/client.pem \
--kubelet-client-key ./cert/client-key.pem \
--log-dir /data/logs/kubernetes/kube-apiserver \
--tls-cert-file ./cert/apiserver.pem \
--tls-private-key-file ./cert/apiserver-key.pem \
--v 2

chmod +x /opt/kubernetes/server/bin/kube-apiserver.sh
```

### 配置supervisor

```bash
vi /etc/supervisord.d/kube-apiserver.ini

[program:kube-apiserver-7-21] 
command=/opt/kubernetes/server/bin/kube-apiserver.sh
numprocs=1                 
directory=/opt/kubernetes/server/bin
autorestart=true
autostart=true
startsecs=30
startretries=3
exitcodes=0,2
stopsignal=QUIT
stopwaitsecs=10      
user=etcd
redirect_stderr=true
stdout_logfile = /data/logs/kubernetes/kube-apiserver/apiserver.stdout.log
stdout_logfile_maxbytes=64MB
stdout_logfile_backups=4
stdout_capture_maxbytes=1MB
stdout_events_enabled=false

mkdir -p /data/logs/kubernetes/kube-apiserver/
supervisorctl update
```