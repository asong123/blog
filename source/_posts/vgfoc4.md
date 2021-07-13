---
title: Docker网络
urlname: vgfoc4
date: '2021-07-12 14:05:38 +0800'
tags: []
categories: []
abbrlink: 59415
---

# 1. 单机

## 1. network namespace

> linux 网络虚拟化：network namespace
> `network namespace` 是实现网络虚拟化的重要功能，它能创建多个`隔离的网络空间`，它们有独自的网络栈信息。

[https://blog.csdn.net/u012707739/article/details/78163354](https://blog.csdn.net/u012707739/article/details/78163354)

> 操作系统为 centos7

## 2. 单机如何实现两个隔离的网络空间互相连接

![image-20191109104544069.png](https://cdn.nlark.com/yuque/0/2019/png/656687/1575966124874-75dee21d-626b-4918-a540-d3cb3bb1ddd7.png#height=539&id=ldh5M&name=image-20191109104544069.png&originHeight=539&originWidth=1078&originalType=binary∶=1&size=76994&status=done&style=none&width=1078)

### 创建两个 network namespace

```shell
[vagrant@docker-node1 ~]$ sudo ip netns add test1
[vagrant@docker-node1 ~]$ sudo ip netns add test2
[vagrant@docker-node1 ~]$ sudo ip netns list
test2
test1
#删除network namespace $ sudo ip netns delete test2
```

### 宿主机上创建一对 ip link

```shell
[vagrant@docker-node1 ~]$ sudo ip link add veth-test1 type veth peer name veth-test2
[vagrant@docker-node1 ~]$ ip link
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN mode DEFAULT group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP mode DEFAULT group default qlen 1000
    link/ether 52:54:00:8a:fe:e6 brd ff:ff:ff:ff:ff:ff
3: eth1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP mode DEFAULT group default qlen 1000
    link/ether 08:00:27:f7:58:d2 brd ff:ff:ff:ff:ff:ff
4: docker0: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc noqueue state DOWN mode DEFAULT group default
    link/ether 02:42:43:05:82:45 brd ff:ff:ff:ff:ff:ff
5: veth-test2@veth-test1: <BROADCAST,MULTICAST,M-DOWN> mtu 1500 qdisc noop state DOWN mode DEFAULT group default qlen 1000
    link/ether 62:50:61:f6:af:3e brd ff:ff:ff:ff:ff:ff
6: veth-test1@veth-test2: <BROADCAST,MULTICAST,M-DOWN> mtu 1500 qdisc noop state DOWN mode DEFAULT group default qlen 1000
    link/ether ae:26:4d:7c:bb:e4 brd ff:ff:ff:ff:ff:ff
```

> 5，6 都是新增的 ip-link

### 将 ip-link 加入到 network namespace 中

```shell
[vagrant@docker-node1 ~]$ sudo ip link set veth-test1 netns test1
[vagrant@docker-node1 ~]$ sudo ip netns exec test1 ip link
1: lo: <LOOPBACK> mtu 65536 qdisc noop state DOWN mode DEFAULT group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
6: veth-test1@if5: <BROADCAST,MULTICAST> mtu 1500 qdisc noop state DOWN mode DEFAULT group default qlen 1000
    link/ether ae:26:4d:7c:bb:e4 brd ff:ff:ff:ff:ff:ff link-netnsid 0
[vagrant@docker-node1 ~]$ ip link
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN mode DEFAULT group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP mode DEFAULT group default qlen 1000
    link/ether 52:54:00:8a:fe:e6 brd ff:ff:ff:ff:ff:ff
3: eth1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP mode DEFAULT group default qlen 1000
    link/ether 08:00:27:f7:58:d2 brd ff:ff:ff:ff:ff:ff
4: docker0: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc noqueue state DOWN mode DEFAULT group default
    link/ether 02:42:43:05:82:45 brd ff:ff:ff:ff:ff:ff
5: veth-test2@if6: <BROADCAST,MULTICAST> mtu 1500 qdisc noop state DOWN mode DEFAULT group default qlen 1000
    link/ether 62:50:61:f6:af:3e brd ff:ff:ff:ff:ff:ff link-netnsid 0
```

> 6 ip-link 就新增到了 test1 中，同理操作 test2 与 veth-test2

### 给两个 ip-link 添加 ip 地址并启动端口

```shell
#添加
[vagrant@docker-node1 ~]$ sudo ip netns exec test1 ip addr add 192.168.1.1/24 dev veth-test1
[vagrant@docker-node1 ~]$ sudo ip netns exec test2 ip addr add 192.168.1.2/24 dev veth-test2
[vagrant@docker-node1 ~]$ sudo ip netns exec test1 ip link
1: lo: <LOOPBACK> mtu 65536 qdisc noop state DOWN mode DEFAULT group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
6: veth-test1@if7: <BROADCAST,MULTICAST> mtu 1500 qdisc noop state DOWN mode DEFAULT group default qlen 1000
    link/ether 2e:ff:e2:3b:d5:79 brd ff:ff:ff:ff:ff:ff link-netnsid 1
[vagrant@docker-node1 ~]$ sudo ip netns exec test2 ip link
1: lo: <LOOPBACK> mtu 65536 qdisc noop state DOWN mode DEFAULT group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
5: veth-test2@if8: <BROADCAST,MULTICAST> mtu 1500 qdisc noop state DOWN mode DEFAULT group default qlen 1000
    link/ether 32:cd:aa:1c:50:be brd ff:ff:ff:ff:ff:ff link-netnsid 0
#启动
[vagrant@docker-node1 ~]$ sudo ip netns exec test1 ip link set dev veth-test1 up
[vagrant@docker-node1 ~]$ sudo ip netns exec test2 ip link set dev veth-test2 up
[vagrant@docker-node1 ~]$ sudo ip netns exec test1 ip link
1: lo: <LOOPBACK> mtu 65536 qdisc noop state DOWN mode DEFAULT group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
6: veth-test1@if7: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP mode DEFAULT group default qlen 1000
    link/ether 2e:ff:e2:3b:d5:79 brd ff:ff:ff:ff:ff:ff link-netnsid 1
[vagrant@docker-node1 ~]$ sudo ip netns exec test2 ip link
1: lo: <LOOPBACK> mtu 65536 qdisc noop state DOWN mode DEFAULT group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
5: veth-test2@if8: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP mode DEFAULT group default qlen 1000
    link/ether 32:cd:aa:1c:50:be brd ff:ff:ff:ff:ff:ff link-netnsid 0
#查看ip
[vagrant@docker-node1 ~]$ sudo ip netns exec test1 ip a
1: lo: <LOOPBACK> mtu 65536 qdisc noop state DOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
6: veth-test1@if7: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default qlen 1000
    link/ether 2e:ff:e2:3b:d5:79 brd ff:ff:ff:ff:ff:ff link-netnsid 1
    inet 192.168.1.1/24 scope global veth-test1
       valid_lft forever preferred_lft forever
    inet6 fe80::2cff:e2ff:fe3b:d579/64 scope link
       valid_lft forever preferred_lft forever
[vagrant@docker-node1 ~]$ sudo ip netns exec test2 ip a
1: lo: <LOOPBACK> mtu 65536 qdisc noop state DOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
5: veth-test2@if8: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default qlen 1000
    link/ether 32:cd:aa:1c:50:be brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 192.168.1.2/24 scope global veth-test2
       valid_lft forever preferred_lft forever
    inet6 fe80::30cd:aaff:fe1c:50be/64 scope link
       valid_lft forever preferred_lft forever
```

### 测试网络连接

```shell
[vagrant@docker-node1 ~]$ sudo ip netns exec test1 ping 192.168.1.2
PING 192.168.1.2 (192.168.1.2) 56(84) bytes of data.
64 bytes from 192.168.1.2: icmp_seq=1 ttl=64 time=0.045 ms
64 bytes from 192.168.1.2: icmp_seq=2 ttl=64 time=0.076 ms
64 bytes from 192.168.1.2: icmp_seq=3 ttl=64 time=0.064 ms
64 bytes from 192.168.1.2: icmp_seq=4 ttl=64 time=0.048 ms
[vagrant@docker-node1 ~]$ sudo ip netns exec test2 ping 192.168.1.1
PING 192.168.1.1 (192.168.1.1) 56(84) bytes of data.
64 bytes from 192.168.1.1: icmp_seq=1 ttl=64 time=0.033 ms
64 bytes from 192.168.1.1: icmp_seq=2 ttl=64 time=0.046 ms
64 bytes from 192.168.1.1: icmp_seq=3 ttl=64 time=0.050 ms
```

## 3. Docker 容器如何访问外网 - bridge

### 创建并运行一个 docker 容器

```shell
[vagrant@docker-node1 ~]$ docker pull busybox
[vagrant@docker-node1 ~]$ sudo docker run -d --name test2 busybox /bin/sh -c "while true; do sleep 3600; done" #创建一个容器并执行shell命令保持运行状态
59cd729c09662cd28a34750ca3a436f6f73d1841e5f0b082a6a83febda832edd
[vagrant@docker-node1 ~]$ docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
59cd729c0966        busybox             "sh"                8 seconds ago       Up 7 seconds                            test1
```

### 查看 docker 网络

```shell
#查看当前机器docker有哪些网络
[vagrant@docker-node1 ~]$ sudo docker network ls
NETWORK ID          NAME                DRIVER              SCOPE
1f58b69450be        bridge              bridge              local
61019a27fd93        host                host                local
c4414af1fb82        none                null                local
```

```shell
#查看network bridge的信息
[vagrant@docker-node1 ~]$ docker network inspect 1f58b69450be
[
    {
        "Name": "bridge",
        "Id": "1f58b69450be74a84e407076418a5d0ed027fb3591793eb0fd6b78d90dc182eb",
        "Created": "2019-11-09T01:52:22.976987734Z",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": null,
            "Config": [
                {
                    "Subnet": "172.17.0.0/16"
                }
            ]
        },
        "Internal": false,
        "Attachable": false,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Containers": {
            "59cd729c09662cd28a34750ca3a436f6f73d1841e5f0b082a6a83febda832edd": {
                "Name": "test1",
                "EndpointID": "fe03e11ba8e905ed0dd9497289ecee33e83ed69ad852c8a836ab4e28f5a1f72b",
                "MacAddress": "02:42:ac:11:00:02",
                "IPv4Address": "172.17.0.2/16",
                "IPv6Address": ""
            }
        },
        "Options": {
            "com.docker.network.bridge.default_bridge": "true",
            "com.docker.network.bridge.enable_icc": "true",
            "com.docker.network.bridge.enable_ip_masquerade": "true",
            "com.docker.network.bridge.host_binding_ipv4": "0.0.0.0",
            "com.docker.network.bridge.name": "docker0",
            "com.docker.network.driver.mtu": "1500"
        },
        "Labels": {}
    }
]
```

> 在 Containers 中 有 test1 容器的 id，说明 test1 的 container 连接到了 bridge 网络上面

```shell
#宿主机的ip link
[vagrant@docker-node1 ~]$ ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 52:54:00:8a:fe:e6 brd ff:ff:ff:ff:ff:ff
    inet 10.0.2.15/24 brd 10.0.2.255 scope global noprefixroute dynamic eth0
       valid_lft 80475sec preferred_lft 80475sec
    inet6 fe80::5054:ff:fe8a:fee6/64 scope link
       valid_lft forever preferred_lft forever
3: eth1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 08:00:27:f7:58:d2 brd ff:ff:ff:ff:ff:ff
    inet 192.168.205.10/24 brd 192.168.205.255 scope global noprefixroute eth1
       valid_lft forever preferred_lft forever
    inet6 fe80::a00:27ff:fef7:58d2/64 scope link
       valid_lft forever preferred_lft forever
4: docker0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default
    link/ether 02:42:43:05:82:45 brd ff:ff:ff:ff:ff:ff
    inet 172.17.0.1/16 brd 172.17.255.255 scope global docker0
       valid_lft forever preferred_lft forever
    inet6 fe80::42:43ff:fe05:8245/64 scope link
       valid_lft forever preferred_lft forever
16: veth48a3664@if15: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue master docker0 state UP group default
    link/ether b6:25:3c:5f:c3:4e brd ff:ff:ff:ff:ff:ff link-netnsid 2
    inet6 fe80::b425:3cff:fe5f:c34e/64 scope link
       valid_lft forever preferred_lft forever
#容器的ip link
[vagrant@docker-node1 ~]$ docker exec test1 ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
15: eth0@if16: <BROADCAST,MULTICAST,UP,LOWER_UP,M-DOWN> mtu 1500 qdisc noqueue
    link/ether 02:42:ac:11:00:02 brd ff:ff:ff:ff:ff:ff
    inet 172.17.0.2/16 brd 172.17.255.255 scope global eth0
       valid_lft forever preferred_lft forever
#查看veth48a3664@if15连接到主机网络docker网络上面
[vagrant@docker-node1 ~]$ brctl show
bridge name	bridge id		STP enabled	interfaces
docker0		8000.024243058245	no		veth48a3664
```

> test1 容器的 `eth0@if16`与 宿主机的`veth48a3664@if15` 是一对 ip link。
> 然后`veth48a3664@if15`又连接到了 `docker0` 上面
>
> - 再创建一个运行容器 test2
> - 执行`$ ip a` 你会发现会多出一个 ip link
> - 再执行`$ brctl show` docker0 的 bridge 会对应两个 interfaces 值

![image-20191109114934452.png](https://cdn.nlark.com/yuque/0/2019/png/656687/1575966157336-a9f4abc2-be98-4e39-b8f6-9d2245b878c9.png#height=565&id=mH9aj&name=image-20191109114934452.png&originHeight=565&originWidth=1087&originalType=binary∶=1&size=108473&status=done&style=none&width=1087)

## 4. Docker 容器之间互相访问 - bridge

> docker0 与 eth0 之间搭建 `NAT`做网络地址转换来访问外网

![image-20191109115501375.png](https://cdn.nlark.com/yuque/0/2019/png/656687/1575966164423-05c66adb-8179-4e87-bc9e-a1d89c2bd3af.png#height=779&id=fN90f&name=image-20191109115501375.png&originHeight=779&originWidth=1304&originalType=binary∶=1&size=179479&status=done&style=none&width=1304)

## 容器之间的 link

### 方式一

```shell
#创建test2容器连接test1  --link test1
[vagrant@docker-node1 ~]$ sudo docker run -d --name test2 --link test1 busybox /bin/sh -c "while true; do sleep 3600; done"
1e4e88339dd7e2c0b955986f6686fc9460aeacaaa0cfeea514b7e9223019c572
#进入容器test2
[vagrant@docker-node1 ~]$ docker exec -it test2 /bin/sh
/ #
#ping test1容器名称
/ # ping test1
PING test1 (172.17.0.2): 56 data bytes
64 bytes from 172.17.0.2: seq=0 ttl=64 time=0.129 ms
64 bytes from 172.17.0.2: seq=1 ttl=64 time=0.169 ms
```

> 容器之间的 link，先创建 test1 容器后，在创建 test2 容器时将 test2 容器连接到 test1 容器上。这样在 test2 容器上就可以输入 test1 容器的名称就可以直接访问了

### 方式二

> - 创建一个 docker bridge
> - 让新建的容器连接的我们创建的 bridge
> - 让已有的容器连接到新建的 bridge 上面
> - 容器名称互通

#### 创建一个 network bridge

```shell
[vagrant@docker-node1 ~]$ docker network create -d bridge my-bridge
0b30512216c7b1f302571c4bdc240b5f97a544c1e8d35db189613e22f2fe61aa
[vagrant@docker-node1 ~]$ docker network ls
NETWORK ID          NAME                DRIVER              SCOPE
1f58b69450be        bridge              bridge              local
61019a27fd93        host                host                local
0b30512216c7        my-bridge           bridge              local
c4414af1fb82        none                null                local
[vagrant@docker-node1 ~]$ brctl show
bridge name	bridge id		STP enabled	interfaces
br-0b30512216c7		8000.0242d5b52a2a	no
docker0		8000.024243058245	no		veth48a3664
#br-0b30512216c7	就是新建的bridge ip a中也会有
```

#### 创建容器连接新建的 bridge

```shell
[vagrant@docker-node1 ~]$ sudo docker run -d --name test3 --network my-bridge busybox /bin/sh -c "while true; do sleep 3600; done"
de43cd76eed384f88a46903546757e2352bcf665ecfefb98100caabd2862ce98
[vagrant@docker-node1 ~]$ docker network ls
NETWORK ID          NAME                DRIVER              SCOPE
1f58b69450be        bridge              bridge              local
61019a27fd93        host                host                local
0b30512216c7        my-bridge           bridge              local
c4414af1fb82        none                null                local
[vagrant@docker-node1 ~]$ docker network inspect 0b30512216c7
[
    {
        "Name": "my-bridge",
        "Id": "0b30512216c7b1f302571c4bdc240b5f97a544c1e8d35db189613e22f2fe61aa",
        "Created": "2019-11-09T04:14:29.525781471Z",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": {},
            "Config": [
                {
                    "Subnet": "172.18.0.0/16",
                    "Gateway": "172.18.0.1"
                }
            ]
        },
        "Internal": false,
        "Attachable": false,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Containers": {
             #test3
            "de43cd76eed384f88a46903546757e2352bcf665ecfefb98100caabd2862ce98": {
                "Name": "test3",
                "EndpointID": "e06a98514abbee5cedf5e1a4dbde8df2dc5b2a3056398192abb4a2a078033eef",
                "MacAddress": "02:42:ac:12:00:02",
                "IPv4Address": "172.18.0.2/16",
                "IPv6Address": ""
            }
        },
        "Options": {},
        "Labels": {}
    }
]
```

#### 让已有的容器连接到新建的 bridge 上面

```shell
[vagrant@docker-node1 ~]$ docker network connect my-bridge test1
[vagrant@docker-node1 ~]$ docker network inspect 0b30512216c7
[
    {
        "Name": "my-bridge",
        "Id": "0b30512216c7b1f302571c4bdc240b5f97a544c1e8d35db189613e22f2fe61aa",
        "Created": "2019-11-09T04:14:29.525781471Z",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": {},
            "Config": [
                {
                    "Subnet": "172.18.0.0/16",
                    "Gateway": "172.18.0.1"
                }
            ]
        },
        "Internal": false,
        "Attachable": false,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Containers": {
        		#test1
            "59cd729c09662cd28a34750ca3a436f6f73d1841e5f0b082a6a83febda832edd": {
                "Name": "test1",
                "EndpointID": "f8116bfd4b909dba6ad280df477557654f42aa6ae810ff75c71d2a5690c7774b",
                "MacAddress": "02:42:ac:12:00:03",
                "IPv4Address": "172.18.0.3/16",
                "IPv6Address": ""
            },
        		#test3
            "de43cd76eed384f88a46903546757e2352bcf665ecfefb98100caabd2862ce98": {
                "Name": "test3",
                "EndpointID": "e06a98514abbee5cedf5e1a4dbde8df2dc5b2a3056398192abb4a2a078033eef",
                "MacAddress": "02:42:ac:12:00:02",
                "IPv4Address": "172.18.0.2/16",
                "IPv6Address": ""
            }
        },
        "Options": {},
        "Labels": {}
    }
]
```

#### 容器名称互通

```shell
[vagrant@docker-node1 ~]$ docker exec -it test3 /bin/sh
/ # ping test1
```

> 如果名称 ping 不通 可尝试更换 centos 镜像操作

## 5. 容器端口映射 - bridge

#### 创建 nginx 容器并且映射 80 端口

```shell
[vagrant@docker-node1 ~]$ docker run --name web -d -p 80:80 nginx
6a777778c883e3c88606de2cd1146cbc770cf49dba6268076ddfeaab21203b16
[vagrant@docker-node1 ~]$ docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                NAMES
6a777778c883        nginx               "nginx -g 'daemon of…"   12 seconds ago      Up 10 seconds       0.0.0.0:80->80/tcp   web
```

#### 宿主机测试

```shell
[vagrant@docker-node1 ~]$ curl 127.0.0.1
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
    body {
        width: 35em;
        margin: 0 auto;
        font-family: Tahoma, Verdana, Arial, sans-serif;
    }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
```

![image-20191109131535410.png](https://cdn.nlark.com/yuque/0/2019/png/656687/1575966177433-16c09c28-5af0-44ed-8248-9ec908166459.png#height=703&id=ehLoa&name=image-20191109131535410.png&originHeight=703&originWidth=1049&originalType=binary∶=1&size=103773&status=done&style=none&width=1049)

## 6. 容器网络 - host

```shell
# 创建容器 --network host
[vagrant@docker-node1 ~]$ sudo docker run -d --name test1 --network host busybox /bin/sh -c "while true; do sleep 3600; done"
e1547af5345c0b9a384714434e747adae9b3552edd9eafb8d77c64a071a4436a
# 查看network host ip与mac都没有
[vagrant@docker-node1 ~]$ docker network inspect host
[
    {
        "Name": "host",
        "Id": "61019a27fd930b620145d93c234ee2d559e5def8f0341231add11bbb60f96d99",
        "Created": "2019-11-09T01:52:22.957734872Z",
        "Scope": "local",
        "Driver": "host",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": null,
            "Config": []
        },
        "Internal": false,
        "Attachable": false,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Containers": {
            "e1547af5345c0b9a384714434e747adae9b3552edd9eafb8d77c64a071a4436a": {
                "Name": "test1",
                "EndpointID": "0dacf8d6460d25336ac6a5dcfd811448e550d5abec749368110b740ad47308b7",
                "MacAddress": "",
                "IPv4Address": "",
                "IPv6Address": ""
            }
        },
        "Options": {},
        "Labels": {}
    }
]
# 进入容器
[vagrant@docker-node1 ~]$ docker exec -it test1 /bin/sh
/ # ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast qlen 1000
    link/ether 52:54:00:8a:fe:e6 brd ff:ff:ff:ff:ff:ff
    inet 10.0.2.15/24 brd 10.0.2.255 scope global dynamic eth0
       valid_lft 73164sec preferred_lft 73164sec
    inet6 fe80::5054:ff:fe8a:fee6/64 scope link
       valid_lft forever preferred_lft forever
3: eth1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast qlen 1000
    link/ether 08:00:27:f7:58:d2 brd ff:ff:ff:ff:ff:ff
    inet 192.168.205.10/24 brd 192.168.205.255 scope global eth1
       valid_lft forever preferred_lft forever
    inet6 fe80::a00:27ff:fef7:58d2/64 scope link
       valid_lft forever preferred_lft forever
4: docker0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue
    link/ether 02:42:43:05:82:45 brd ff:ff:ff:ff:ff:ff
    inet 172.17.0.1/16 brd 172.17.255.255 scope global docker0
       valid_lft forever preferred_lft forever
    inet6 fe80::42:43ff:fe05:8245/64 scope link
       valid_lft forever preferred_lft forever
21: br-0b30512216c7: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc noqueue
    link/ether 02:42:d5:b5:2a:2a brd ff:ff:ff:ff:ff:ff
    inet 172.18.0.1/16 brd 172.18.255.255 scope global br-0b30512216c7
       valid_lft forever preferred_lft forever
    inet6 fe80::42:d5ff:feb5:2a2a/64 scope link
       valid_lft forever preferred_lft forever
31: veth964efd1@if30: <BROADCAST,MULTICAST,UP,LOWER_UP,M-DOWN> mtu 1500 qdisc noqueue master docker0
    link/ether b2:a8:f4:18:14:db brd ff:ff:ff:ff:ff:ff
    inet6 fe80::b0a8:f4ff:fe18:14db/64 scope link
       valid_lft forever preferred_lft forever
#退出容器执行ip a
/ # exit
[vagrant@docker-node1 ~]$ ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 52:54:00:8a:fe:e6 brd ff:ff:ff:ff:ff:ff
    inet 10.0.2.15/24 brd 10.0.2.255 scope global noprefixroute dynamic eth0
       valid_lft 73002sec preferred_lft 73002sec
    inet6 fe80::5054:ff:fe8a:fee6/64 scope link
       valid_lft forever preferred_lft forever
3: eth1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 08:00:27:f7:58:d2 brd ff:ff:ff:ff:ff:ff
    inet 192.168.205.10/24 brd 192.168.205.255 scope global noprefixroute eth1
       valid_lft forever preferred_lft forever
    inet6 fe80::a00:27ff:fef7:58d2/64 scope link
       valid_lft forever preferred_lft forever
4: docker0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default
    link/ether 02:42:43:05:82:45 brd ff:ff:ff:ff:ff:ff
    inet 172.17.0.1/16 brd 172.17.255.255 scope global docker0
       valid_lft forever preferred_lft forever
    inet6 fe80::42:43ff:fe05:8245/64 scope link
       valid_lft forever preferred_lft forever
21: br-0b30512216c7: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc noqueue state DOWN group default
    link/ether 02:42:d5:b5:2a:2a brd ff:ff:ff:ff:ff:ff
    inet 172.18.0.1/16 brd 172.18.255.255 scope global br-0b30512216c7
       valid_lft forever preferred_lft forever
    inet6 fe80::42:d5ff:feb5:2a2a/64 scope link
       valid_lft forever preferred_lft forever
31: veth964efd1@if30: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue master docker0 state UP group default
    link/ether b2:a8:f4:18:14:db brd ff:ff:ff:ff:ff:ff link-netnsid 2
    inet6 fe80::b0a8:f4ff:fe18:14db/64 scope link
       valid_lft forever preferred_lft forever
```

> 这里发现 host 创建的容器 namespace 与宿主机 namespace 一致
> 说明：`host没有自己的namespace而是与宿主机共享一套namespace`

## 7. 容器网络 - none

```shell
# 创建容器 --network none
[vagrant@docker-node1 ~]$ sudo docker run -d --name test1 --network none busybox /bin/sh -c "while true; do sleep 3600; done"
332e3f36b0c53d92cf929ba45403b775b0d9d9cae7a6d5249fccf14ae4e270e1
# 查看network none ip与mac都没有
[vagrant@docker-node1 ~]$ docker network inspect none
[
    {
        "Name": "none",
        "Id": "c4414af1fb8265305bf9124919a397c1402cf00bce4f755a6677f9795ef5db6e",
        "Created": "2019-11-09T01:52:22.949668664Z",
        "Scope": "local",
        "Driver": "null",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": null,
            "Config": []
        },
        "Internal": false,
        "Attachable": false,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Containers": {
            "332e3f36b0c53d92cf929ba45403b775b0d9d9cae7a6d5249fccf14ae4e270e1": {
                "Name": "test1",
                "EndpointID": "cc45c9c5c1d5228c476dc1291d263a858ec65dd7d301913407a5711b756bb268",
                "MacAddress": "",
                "IPv4Address": "",
                "IPv6Address": ""
            }
        },
        "Options": {},
        "Labels": {}
    }
]
```

# 2.多机

## 1. 不同机器的 docker 容器的通信

![image-20191109194620150.png](https://cdn.nlark.com/yuque/0/2019/png/656687/1575966188643-390f23bc-2b3a-4b4b-8548-71edfe895079.png#height=602&id=PMgil&name=image-20191109194620150.png&originHeight=602&originWidth=1429&originalType=binary∶=1&size=134503&status=done&style=none&width=1429)

> docker 容器多机通信使用的`vxlan的概念`解决的

### 创建两台虚拟机 docker-node1 与 docker-node2

> docker-node1 192.168.205.10
> docker-node2 192.168.205.11

### 搭建分布式存储 etcd

> `搭建etcd`主要的目的是为了确保多机时确保每个机器创建 docker 容器时 ip 不重复

#### 在 docker-node1 上

```shell
ubuntu@docker-node1:~$ wget https://github.com/coreos/etcd/releases/download/v3.0.12/etcd-v3.0.12-linux-amd64.tar.gz
ubuntu@docker-node1:~$ tar zxvf etcd-v3.0.12-linux-amd64.tar.gz
ubuntu@docker-node1:~$ cd etcd-v3.0.12-linux-amd64
ubuntu@docker-node1:~$ nohup ./etcd --name docker-node1 --initial-advertise-peer-urls http://192.168.205.10:2380 \
--listen-peer-urls http://192.168.205.10:2380 \
--listen-client-urls http://192.168.205.10:2379,http://127.0.0.1:2379 \
--advertise-client-urls http://192.168.205.10:2379 \
--initial-cluster-token etcd-cluster \
--initial-cluster docker-node1=http://192.168.205.10:2380,docker-node2=http://192.168.205.11:2380 \
--initial-cluster-state new&
```

#### 在 docker-node2 上

```shell
ubuntu@docker-node2:~$ wget https://github.com/coreos/etcd/releases/download/v3.0.12/etcd-v3.0.12-linux-amd64.tar.gz
ubuntu@docker-node2:~$ tar zxvf etcd-v3.0.12-linux-amd64.tar.gz
ubuntu@docker-node2:~$ cd etcd-v3.0.12-linux-amd64/
ubuntu@docker-node2:~$ nohup ./etcd --name docker-node2 --initial-advertise-peer-urls http://192.168.205.11:2380 \
--listen-peer-urls http://192.168.205.11:2380 \
--listen-client-urls http://192.168.205.11:2379,http://127.0.0.1:2379 \
--advertise-client-urls http://192.168.205.11:2379 \
--initial-cluster-token etcd-cluster \
--initial-cluster docker-node1=http://192.168.205.10:2380,docker-node2=http://192.168.205.11:2380 \
--initial-cluster-state new&
```

#### 检查 cluster 状态

```shell
ubuntu@docker-node2:~/etcd-v3.0.12-linux-amd64$ ./etcdctl cluster-health
member 21eca106efe4caee is healthy: got healthy result from http://192.168.205.10:2379
member 8614974c83d1cc6d is healthy: got healthy result from http://192.168.205.11:2379
cluster is healthy
```

### 重启 docker 服务

在 docker-node1 上

```shell
$ sudo service docker stop
$ sudo /usr/bin/dockerd -H tcp://0.0.0.0:2375 -H unix:///var/run/docker.sock --cluster-store=etcd://192.168.205.10:2379 --cluster-advertise=192.168.205.10:2375&
```

在 docker-node2 上

```shell
$ sudo service docker stop
$ sudo /usr/bin/dockerd -H tcp://0.0.0.0:2375 -H unix:///var/run/docker.sock --cluster-store=etcd://192.168.205.11:2379 --cluster-advertise=192.168.205.11:2375&
```

### 创建 overlay network

> 在 docker-node1 上创建一个 demo 的 overlay network

```shell
ubuntu@docker-node1:~$ sudo docker network ls
NETWORK ID          NAME                DRIVER              SCOPE
0e7bef3f143a        bridge              bridge              local
a5c7daf62325        host                host                local
3198cae88ab4        none                null                local
ubuntu@docker-node1:~$ sudo docker network create -d overlay demo
3d430f3338a2c3496e9edeccc880f0a7affa06522b4249497ef6c4cd6571eaa9
ubuntu@docker-node1:~$ sudo docker network ls
NETWORK ID          NAME                DRIVER              SCOPE
0e7bef3f143a        bridge              bridge              local
3d430f3338a2        demo                overlay             global
a5c7daf62325        host                host                local
3198cae88ab4        none                null                local
ubuntu@docker-node1:~$ sudo docker network inspect demo
[
    {
        "Name": "demo",
        "Id": "3d430f3338a2c3496e9edeccc880f0a7affa06522b4249497ef6c4cd6571eaa9",
        "Scope": "global",
        "Driver": "overlay",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": {},
            "Config": [
                {
                    "Subnet": "10.0.0.0/24",
                    "Gateway": "10.0.0.1/24"
                }
            ]
        },
        "Internal": false,
        "Containers": {},
        "Options": {},
        "Labels": {}
    }
]
```

> 我们会看到在 node2 上，这个 demo 的 overlay network 会被同步创建

```
ubuntu@docker-node2:~$ sudo docker network ls
NETWORK ID          NAME                DRIVER              SCOPE
c9947d4c3669        bridge              bridge              local
3d430f3338a2        demo                overlay             global
fa5168034de1        host                host                local
c2ca34abec2a        none                null                local
```

> 通过查看 etcd 的 key-value, 我们获取到，这个 demo 的 network 是通过 etcd 从 node1 同步到 node2 的

```shell
ubuntu@docker-node2:~/etcd-v3.0.12-linux-amd64$ ./etcdctl ls /docker
/docker/network
/docker/nodes
ubuntu@docker-node2:~/etcd-v3.0.12-linux-amd64$ ./etcdctl ls /docker/nodes
/docker/nodes/192.168.205.11:2375
/docker/nodes/192.168.205.10:2375
ubuntu@docker-node2:~/etcd-v3.0.12-linux-amd64$ ./etcdctl ls /docker/network/v1.0/network
/docker/network/v1.0/network/3d430f3338a2c3496e9edeccc880f0a7affa06522b4249497ef6c4cd6571eaa9
ubuntu@docker-node2:~/etcd-v3.0.12-linux-amd64$ ./etcdctl get /docker/network/v1.0/network/3d430f3338a2c3496e9edeccc880f0a7affa06522b4249497ef6c4cd6571eaa9 | jq .
{
  "addrSpace": "GlobalDefault",
  "enableIPv6": false,
  "generic": {
    "com.docker.network.enable_ipv6": false,
    "com.docker.network.generic": {}
  },
  "id": "3d430f3338a2c3496e9edeccc880f0a7affa06522b4249497ef6c4cd6571eaa9",
  "inDelete": false,
  "ingress": false,
  "internal": false,
  "ipamOptions": {},
  "ipamType": "default",
  "ipamV4Config": "[{\"PreferredPool\":\"\",\"SubPool\":\"\",\"Gateway\":\"\",\"AuxAddresses\":null}]",
  "ipamV4Info": "[{\"IPAMData\":\"{\\\"AddressSpace\\\":\\\"GlobalDefault\\\",\\\"Gateway\\\":\\\"10.0.0.1/24\\\",\\\"Pool\\\":\\\"10.0.0.0/24\\\"}\",\"PoolID\":\"GlobalDefault/10.0.0.0/24\"}]",
  "labels": {},
  "name": "demo",
  "networkType": "overlay",
  "persist": true,
  "postIPv6": false,
  "scope": "global"
}
```

### 创建连接 demo 网络的容器

#### 在 docker-node1 上

```shell
vagrant@docker-node1:~$ sudo docker run -d --name test1 --net demo busybox sh -c "while true; do sleep 3600; done"
Unable to find image 'busybox:latest' locally
latest: Pulling from library/busybox
56bec22e3559: Pull complete
Digest: sha256:29f5d56d12684887bdfa50dcd29fc31eea4aaf4ad3bec43daf19026a7ce69912
Status: Downloaded newer image for busybox:latest
a95a9466331dd9305f9f3c30e7330b5a41aae64afda78f038fc9e04900fcac54
vagrant@docker-node1:~$ sudo docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS               NAMES
a95a9466331d        busybox             "sh -c 'while true; d"   4 seconds ago       Up 3 seconds                            test1
vagrant@docker-node1:~$ sudo docker exec test1 ifconfig
eth0      Link encap:Ethernet  HWaddr 02:42:0A:00:00:02
          inet addr:10.0.0.2  Bcast:0.0.0.0  Mask:255.255.255.0
          inet6 addr: fe80::42:aff:fe00:2/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1450  Metric:1
          RX packets:15 errors:0 dropped:0 overruns:0 frame:0
          TX packets:8 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0
          RX bytes:1206 (1.1 KiB)  TX bytes:648 (648.0 B)

eth1      Link encap:Ethernet  HWaddr 02:42:AC:12:00:02
          inet addr:172.18.0.2  Bcast:0.0.0.0  Mask:255.255.0.0
          inet6 addr: fe80::42:acff:fe12:2/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:8 errors:0 dropped:0 overruns:0 frame:0
          TX packets:8 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0
          RX bytes:648 (648.0 B)  TX bytes:648 (648.0 B)

lo        Link encap:Local Loopback
          inet addr:127.0.0.1  Mask:255.0.0.0
          inet6 addr: ::1/128 Scope:Host
          UP LOOPBACK RUNNING  MTU:65536  Metric:1
          RX packets:0 errors:0 dropped:0 overruns:0 frame:0
          TX packets:0 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1
          RX bytes:0 (0.0 B)  TX bytes:0 (0.0 B)

vagrant@docker-node1:~$
```

#### 在 docker-node2 上

```shell
vagrant@docker-node2:~$ sudo docker run -d --name test2 --net demo busybox sh -c "while true; do sleep 3600; done"
Unable to find image 'busybox:latest' locally
latest: Pulling from library/busybox
56bec22e3559: Pull complete
Digest: sha256:29f5d56d12684887bdfa50dcd29fc31eea4aaf4ad3bec43daf19026a7ce69912
Status: Downloaded newer image for busybox:latest
fad6dc6538a85d3dcc958e8ed7b1ec3810feee3e454c1d3f4e53ba25429b290b
docker: Error response from daemon: service endpoint with name test1 already exists.
vagrant@docker-node2:~$ sudo docker run -d --name test2 --net demo busybox sh -c "while true; do sleep 3600; done"
9d494a2f66a69e6b861961d0c6af2446265bec9b1d273d7e70d0e46eb2e98d20
```

#### 验证连通性

```shell
vagrant@docker-node2:~$ sudo docker exec -it test2 ifconfig
eth0      Link encap:Ethernet  HWaddr 02:42:0A:00:00:03
          inet addr:10.0.0.3  Bcast:0.0.0.0  Mask:255.255.255.0
          inet6 addr: fe80::42:aff:fe00:3/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1450  Metric:1
          RX packets:208 errors:0 dropped:0 overruns:0 frame:0
          TX packets:201 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0
          RX bytes:20008 (19.5 KiB)  TX bytes:19450 (18.9 KiB)

eth1      Link encap:Ethernet  HWaddr 02:42:AC:12:00:02
          inet addr:172.18.0.2  Bcast:0.0.0.0  Mask:255.255.0.0
          inet6 addr: fe80::42:acff:fe12:2/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:8 errors:0 dropped:0 overruns:0 frame:0
          TX packets:8 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0
          RX bytes:648 (648.0 B)  TX bytes:648 (648.0 B)

lo        Link encap:Local Loopback
          inet addr:127.0.0.1  Mask:255.0.0.0
          inet6 addr: ::1/128 Scope:Host
          UP LOOPBACK RUNNING  MTU:65536  Metric:1
          RX packets:0 errors:0 dropped:0 overruns:0 frame:0
          TX packets:0 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1
          RX bytes:0 (0.0 B)  TX bytes:0 (0.0 B)
# 在node2的容器中 ping node1的容器ip
vagrant@docker-node1:~$ sudo docker exec test1 sh -c "ping 10.0.0.3"
PING 10.0.0.3 (10.0.0.3): 56 data bytes
64 bytes from 10.0.0.3: seq=0 ttl=64 time=0.579 ms
64 bytes from 10.0.0.3: seq=1 ttl=64 time=0.411 ms
64 bytes from 10.0.0.3: seq=2 ttl=64 time=0.483 ms
```
