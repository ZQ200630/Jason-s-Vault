
---
tags: Kubernates
---
# 部署Docker

### 下载Docker

```bash
[[在所有需要部署k8s的服务器上都装上Docker]]
curl -fsSL https://get.docker.com | bash -s docker --mirror Aliyun

```

### 配置Docker

```bash
mkdir /etc/docker
vi /etc/docker/daemon.json

[[daemon]].json
[[下述bip中ip]]第三位要和主机的第四位对应
{
	"graph": "/data/docker",
	"storage-driver": "overlay2",
	"insecure-registries": ["http://harbor.od.com"],
	"registry-mirrors": ["https://kzflb.mirror.aliyuncs.com"],
	"bip": "172.7.21.1/24",
	"exec-opts": ["native.cgroupdriver=systemd"],
	"live-restore": true
}
[[bip指定网桥IP]]
[[insecure-registries指定docker]]私服域名

mkdir -p /data/docker
[[添加Docker]]的数据文件位置
```

### 启动Docker

```bash

systemctl daemon-reload
systemctl start docker
docker version
```