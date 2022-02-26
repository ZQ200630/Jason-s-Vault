---
tags: Kubernates
---
# 部署vip

### 配置nginx

```bash
yum install nginx -y

vim /etc/nginx/nginx.conf

#在最底下添加

stream {
upstream kube-apiserver { 
      server  10.4.7.21:6443; 
      server  10.4.7.22:6443; 
} 
  
server{ 
    listen 7443; 
    proxy_connect_timeout 2s;
    proxy_timeout 900s;
    proxy_pass kube-apiserver;
        }
}
```

### 检查并启动nginx

```bash
nginx -t #检查配置
systemctl start nginx
systemctl enable nginx
```

### 安装keepalive

```bash
yum install keepalived -y

vim /etc/keepalived/check_port.sh

#!/bin/bash
CHK_PORT=$1
if [ -n "$CHK_PORT" ];then
	PORT_PROCESS=`ss -lnt|grep $CHK_PORT|wc -l`
	if [ $PORT_PROCESS -eq 0 ];then
		echo "Port $CHK_PORT Is Not Used, End."
		exit 1
	fi
else
	echo "Check Porrt Cant Be Empty!"
fi

chmod +x /etc/keepalived/check_port.sh
```

### 配置keepalive

```bash
#主

vim /etc/keepalived/keepalived.conf

! Configuration File for keepalived

global_defs {#全局配置
   router_id 10.4.7.11 #可以使用主机名
}

vrrp_script chk_nginx {
	script "/etc/keepalived/check_port.sh 7443"
	interval 2
	weight -20
}

vrrp_instance VI_1 { #定义一个虚拟路由器，第一个
    state MASTER  #当前状态，主的
    interface ens33  [[当前的vrp]]应用，绑定到那个网卡设备上
    virtual_router_id 251#这个虚拟ip，与其他主机要保持一至
    priority 100 #优先级，高于其他主机
    advert_int 1
    mcast_src_ip 10.4.7.11
    nopreempt
    authentication {
        auth_type PASS  #密码认证
        auth_pass 11111111   [[密码为8]]位,不能用默认的密码
    }
    track_script {
    	chk_nginx
	}
    virtual_ipaddress {[[虚拟ip]]地址
    	10.4.7.10
    }
}
```

```bash
#从

! Configuration File for keepalived

global_defs {
   router_id 10.4.7.12
}

vrrp_script chk_nginx {
	script "/etc/keepalived/check_port.sh 7443"
	interval 2
	weight -20
}

vrrp_instance VI_1 {
    state BACKUP
    interface ens33
    virtual_router_id 251
    priority 90
    advert_int 1
    mcast_src_ip 10.4.7.12
    authentication {
        auth_type PASS
        auth_pass 11111111
    }
    track_script {
    	chk_nginx
	}
    virtual_ipaddress {
    	10.4.7.10
    }
}
```

### 启动keepalive

```bash
# 一定要关闭防火墙

systemctl start keepalived
systemctl enable keepalived
```