---
layout: post
title: docker部署zabbix

---
### 前言
zabbix的安装，有源码安装，yum安装，docker方式安装这几种方法，一般常采用yum方式安装，很方便。如果你对docker感兴趣，可以尝试docker方式安装。各种安装的方法官网上的文档写的已经非常详细了。我看了下docker安装对应的docker-composer.yml配置文件，的确非常全面，但对于初学者来说，很多配置都用不上，而且缺乏相应的说明，让人有点迷糊。

官网docker安装文档：https://www.zabbix.com/documentation/4.4/manual/installation/containers

本文参考网上的资料，结合自己的实际操作，整理了一份简化版的docker-composer.yml,并对关键的配置项做了说明。

### 环境准备

宿主机：基于Hyper-v创建的centos7虚拟机，主机名为：laptop-centos。已经装好docker环境和docker-composer。

### zabbix镜像

本次安装需要如下三个镜像：
1. zabbix/zabbix-server-mysql:lastest - mysql版的zabbix server
2. zabbix/zabbix-web-nginx-mysql:latest - mysql版的zabbix web包，集成了nginx
3. mysql:5.7 - mysql 5.7数据库
4. zabbix/zabbix-agent:lastet - zabbix agent

如果需要安装proxy，镜像为：zabbix/zabbix-proxy-mysql:latest，这次没有安装。

### docker-compose.yml配置

yaml文件对格式要求比较严格，用空格来缩进，同级别的缩进量要一致。

1. mysql安装：

yaml配置如下：

```
version: '3'
services:
  zabbix-mysql:
    image: mysql:5.7
    # 容器名，相当于主机名，其他容器通过该名字访问mysql服务
    container_name: zabbix-mysql
    # 网络模式：采用默认的网桥模式
    network_mode: bridge
    ports:
        - "3306:3306"
    command: [
        '--character-set-server=utf8',
        '--collation-server=utf8_bin',
    ]
    # mysql启动时会创建zabbix用户和zabbix数据库
    environment:
        - MYSQL_USER=zabbix
        - MYSQL_DATABASE=zabbix
        - MYSQL_PASSWORD=zabbix
        - MYSQL_ROOT_PASSWORD=zabbix
        - LANG=en_US.utf8
        - TZ=Asia/Shanghai
    restart: always
```
2. zabbix-server安装

docker-composer默认的网络模式是bridge，zabbix服务如果采用bridge网络，就有独立的ip和主机名，后面涉及服务器地址的配置都要以容器的主机为准，不能配置宿主机的地址。网上说这种模式无法监控tcp端口，因此我们采用改成host网络模式。host模式表示容器和宿主机共享一个ip。

```
zabbix-server:
    container_name: zabbix-server
    image: zabbix/zabbix-server-mysql:centos-latest
    restart: always
    # 因为zabbix采用的是host模式，和mysql容器不在一个网段，无法直接访问
    # 宿主机配置了3306端口映射，所以这里的db_server_host要写宿主机名。
    # 数据库信息会自动写入到zabbix_server.conf文件中
    environment:
        - DB_SERVER_HOST=laptop-centos
        - MYSQL_DATABASE=zabbix
        - MYSQL_USER=zabbix
        - MYSQL_PASSWORD=zabbix
        - MYSQL_ROOT_PASSWORD=zabbix
        - TZ='Asia/Shanghai'
    ports:
        - "10051:10051"
    # 设置网络模式为host
    network_mode: host
    # 如果MySQL没起，会先启动mysql
    depends_on:
        - "zabbix-mysql"
```

3. zabbix web端安装

```
zabbix-web:
    image: zabbix/zabbix-web-nginx-mysql:latest
    container_name: zabbix-web
    restart: always
    network_mode: bridge
    ports:
        - "8080:80"
    environment:
        - DB_SERVER_HOST=laptop-centos
        - MYSQL_DATABASE=zabbix
        - MYSQL_USER=zabbix
        - MYSQL_PASSWORD=zabbix
        - MYSQL_ROOT_PASSWORD=zabbix
        - PHP_TZ="Asia/Shanghai"
        - TZ='Asia/Shanghai'
    depends_on:
        - "zabbix-server"
        - "zabbix-mysql"
```

4. zabbix-agent 安装

```
zabbix-agent:
    image: zabbix/zabbix-agent:latest
    container_name: zabbix-agent
    #zabbix服务器名要写宿主机名，如果采用bridge模式，就写容器名。
    environment:
        - ZBX_HOSTNAME=laptop-centos
        - ZBX_SERVER_HOST=laptop-centos
        - ZBX_SERVER_PORT=10051
        - TZ='Asia/Shanghai'
    # 网络模式也是host
    network_mode: host
    ports:
        - "10050:10050"
    depends_on:
        - "zabbix-server"
    restart: always
    privileged: true
```

### 启动

进入docker-compose.yml所在目录，敲入命令：docker-compose up -d

-d 表示后台运行，调试期间不用加-d，方便看日志。

