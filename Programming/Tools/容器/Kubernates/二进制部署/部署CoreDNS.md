
---
tags: Kubernates
---# 部署CoreDNS

```bash
vi /etc/nginx/conf.d/k8s-yaml.od.com.conf
# /etc/nginx/conf.d/k8s-yaml.od.com.conf
server {
	listen	80;
	server_name	k8s-yaml.od.com;
	
	location / {
		autoindex on;
		default_type text/plain;
		root /data/k8s-yaml;
	}
}

mkdir /data/k8s-yaml

[[HDSS7-11]]
vi /var/named/od.com.zone
前滚序列号
添加k8s-yaml A 10.4.7.200

[[HDSS7-200]]
cd /data/k8s-yaml
mkdir coredns

docker pull coredns/coredns:1.6.1
docker tag containerid docker harbor.od.com/public/coredns:v1.6.1
docker push harbor.od.com/public/coredns:v1.6.1

cd /data/k8s-yaml/coredns

[[rbac]].yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: coredns
  namespace: kube-system
  labels:
      kubernetes.io/cluster-service: "true"
      addonmanager.kubernetes.io/mode: Reconcile
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  labels:
    kubernetes.io/bootstrapping: rbac-defaults
    addonmanager.kubernetes.io/mode: Reconcile
  name: system:coredns
rules:
- apiGroups:
  - ""
  resources:
  - endpoints
  - services
  - pods
  - namespaces
  verbs:
  - list
  - watch
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  annotations:
    rbac.authorization.kubernetes.io/autoupdate: "true"
  labels:
    kubernetes.io/bootstrapping: rbac-defaults
    addonmanager.kubernetes.io/mode: EnsureExists
  name: system:coredns
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: system:coredns
subjects:
- kind: ServiceAccount
  name: coredns
  namespace: kube-system
  
  
[[cm]].yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: coredns
  namespace: kube-system
data:
  Corefile: |
    .:53 {
        errors
        log
        health
        ready
        kubernetes cluster.local 192.168.0.0/16  [[service资源cluster]]地址
        forward . 10.4.7.11   [[上级DNS]]地址
        cache 30
        loop
        reload
        loadbalance
       }

[[dp]].yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: coredns
  namespace: kube-system
  labels:
    k8s-app: coredns
    kubernetes.io/name: "CoreDNS"
spec:
  replicas: 1
  selector:
    matchLabels:
      k8s-app: coredns
  template:
    metadata:
      labels:
        k8s-app: coredns
    spec:
      priorityClassName: system-cluster-critical
      serviceAccountName: coredns
      containers:
      - name: coredns
        image: harbor.od.com/public/coredns:v1.6.1
        args:
        - -conf
        - /etc/coredns/Corefile
        volumeMounts:
        - name: config-volume
          mountPath: /etc/coredns
        ports:
        - containerPort: 53
          name: dns
          protocol: UDP
        - containerPort: 53
          name: dns-tcp
          protocol: TCP
        - containerPort: 9153
          name: metrics
          protocol: TCP
        livenessProbe:
          httpGet:
            path: /health
            port: 8080
            scheme: HTTP
          initialDelaySeconds: 60
          timeoutSeconds: 5
          successThreshold: 1
          failureThreshold: 5
      dnsPolicy: Default
      volumes:
        - name: config-volume
          configMap:
            name: coredns
            items:
            - key: Corefile
              path: Corefile
             
# svc.yaml
apiVersion: v1
kind: Service
metadata:
  name: coredns
  namespace: kube-system
  labels:
    k8s-app: coredns
    kubernetes.io/cluster-service: "true"
    kubernetes.io/name: "CoreDNS"
spec:
  selector:
    k8s-app: coredns
  clusterIP: 192.168.0.2
  ports:
  - name: dns
    port: 53
    protocol: UDP
  - name: dns-tcp
    port: 53
  - name: metrics
    port: 9153
    protocol: TCP

kubectl create -f http://k8s-yaml.od.com/coredns/rbac.yaml
kubectl create -f http://k8s-yaml.od.com/coredns/cm.yaml
kubectl create -f http://k8s-yaml.od.com/coredns/dp.yaml
kubectl create -f http://k8s-yaml.od.com/coredns/svc.yaml

kubectl get all -n kube-system -o wide

kubectl get svc -n kube-system
#域名获取方式
[[servicename]].namespace.svc.cluster.local.
```