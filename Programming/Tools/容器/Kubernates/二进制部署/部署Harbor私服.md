---
tags: Kubernates
---
# 部署Harbor私服

### 下载Harbor

```bash
[[harbor要1]].7.6以上
[[https]]://github.com/goharbor/harbor/releases 下载offline版本

cd /opt
mkdir src
cd src

#将源软件包放在这里,仅在运维主机安装
tar xf harbor-offline-installer-v1.9.4.tgz -C /opt
cd ..
mv harbor/ harbor-v1.9.4

ln -s /opt/harbor-v1.9.4/ /opt/harbor  #建立软连接
cd harbor [[进入harbor]]文件夹
vi harbor.yml #编辑配置文件
hostname: harbor.od.com
http端口改成180
harbor_admin_password应该更改，默认为Harbor12345
log location改成/data/harbor/logs
data_volume: /data/harbor
[[EOF]]

mkdir -p /data/harbor/logs

[[安装nginx]]
yum install nginx

#一定要关闭防火墙!!!
vi 安装目录/conf/nginx.conf
server {
        listen  80;
        server_name     harbor.od.com;
        client_max_body_size    1000m;
        location / {
                proxy_pass http://127.0.0.1:180;
        }
}

去nginx安装目录下进入sbin启动nginx

在/var/named/od.com.zone
配置harbor	A	10.4.7.200
主义serial前滚一个序号
systemctl restart named
dig -t A harbor.od.com +short
应该返回10.4.7.200
浏览器打开http://harbor.od.com
```

### 安装docker-compose

```bash
curl -L https://get.daocloud.io/docker/compose/releases/download/1.25.0/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
docker-compose --version 
```

### 启动Harbor

```bash
sh /opt/harbor/install.
```

### 安装nginx

```bash
yum install nginx
```

### 配置Nginx

```bash
#一定要关闭防火墙!!!
vi 安装目录/conf/nginx.conf
server {
        listen  80;
        server_name     harbor.od.com;
        client_max_body_size    1000m;
        location / {
                proxy_pass http://127.0.0.1:180;
        }
}
```

### 配置域名

```bash

[[在/var/named/od]].com.zone
[[配置harbor]]	A	10.4.7.200
[[注意serial]]前滚一个序号

systemctl restart named
dig -t A harbor.od.com +short
#检查域名是否配置正确
[[应该返回域名主机IP]]
[[浏览器打开http]]://harbor.od.com
```

### 使用Harbor

```bash
docker pull nginx:1.7.9
docker ps -a
docker tag containerId harbor.od.com/public/nginx
```