- Host 模式：

~~~
docker run -d  --privileged=true  --restart unless-stopped --net=host dockersimage:tag
~~~

- bridge 模式默认是这个模式

- Host模式讨论：

进入容器内的ip地址如下

~~~
/ # ip addr
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP qlen 1000
    link/ether 32:3c:d0:4c:8f:06 brd ff:ff:ff:ff:ff:ff
    inet 165.232.138.175/20 brd 165.232.143.255 scope global eth0
       valid_lft forever preferred_lft forever
    inet 10.48.0.6/16 brd 10.48.255.255 scope global eth0
       valid_lft forever preferred_lft forever
    inet6 2604:a880:4:1d0::7e6:1000/64 scope global
       valid_lft forever preferred_lft forever
    inet6 fe80::303c:d0ff:fe4c:8f06/64 scope link
       valid_lft forever preferred_lft forever
3: eth1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP qlen 1000
    link/ether 02:a3:35:13:2d:b8 brd ff:ff:ff:ff:ff:ff
    inet 10.124.0.3/20 brd 10.124.15.255 scope global eth1
       valid_lft forever preferred_lft forever
    inet6 fe80::a3:35ff:fe13:2db8/64 scope link
       valid_lft forever preferred_lft forever
4: docker0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP
    link/ether 02:42:f0:49:56:12 brd ff:ff:ff:ff:ff:ff
    inet 172.17.0.1/16 scope global docker0
       valid_lft forever preferred_lft forever
    inet6 fe80::42:f0ff:fe49:5612/64 scope link
       valid_lft forever preferred_lft forever
70: veth8e61732@if69: <BROADCAST,MULTICAST,UP,LOWER_UP,M-DOWN> mtu 1500 qdisc noqueue master docker0 state UP
    link/ether 8a:6a:e3:5a:08:20 brd ff:ff:ff:ff:ff:ff
    inet6 fe80::886a:e3ff:fe5a:820/64 scope link
       valid_lft forever preferred_lft forever

~~~

不进容器，ip地址如下：

~~~
[root@ssd ~]# ip addr
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 32:3c:d0:4c:8f:06 brd ff:ff:ff:ff:ff:ff
    inet 165.232.138.175/20 brd 165.232.143.255 scope global eth0
       valid_lft forever preferred_lft forever
    inet 10.48.0.6/16 brd 10.48.255.255 scope global eth0
       valid_lft forever preferred_lft forever
    inet6 2604:a880:4:1d0::7e6:1000/64 scope global
       valid_lft forever preferred_lft forever
    inet6 fe80::303c:d0ff:fe4c:8f06/64 scope link
       valid_lft forever preferred_lft forever
3: eth1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 02:a3:35:13:2d:b8 brd ff:ff:ff:ff:ff:ff
    inet 10.124.0.3/20 brd 10.124.15.255 scope global eth1
       valid_lft forever preferred_lft forever
    inet6 fe80::a3:35ff:fe13:2db8/64 scope link
       valid_lft forever preferred_lft forever
4: docker0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default
    link/ether 02:42:f0:49:56:12 brd ff:ff:ff:ff:ff:ff
    inet 172.17.0.1/16 scope global docker0
       valid_lft forever preferred_lft forever
    inet6 fe80::42:f0ff:fe49:5612/64 scope link
       valid_lft forever preferred_lft forever
70: veth8e61732@if69: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue master docker0 state UP group default
    link/ether 8a:6a:e3:5a:08:20 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet6 fe80::886a:e3ff:fe5a:820/64 scope link
       valid_lft forever preferred_lft forever
72: veth085fa28@if71: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue master docker0 state UP group default
    link/ether 56:95:1a:67:13:68 brd ff:ff:ff:ff:ff:ff link-netnsid 1
    inet6 fe80::5495:1aff:fe67:1368/64 scope link
       valid_lft forever preferred_lft forever

~~~

traceroute ：

~~~
/ # traceroute  -4 google.com
traceroute to google.com (172.217.1.206), 30 hops max, 46 byte packets
 1  *  *  *
 2  10.88.3.226 (10.88.3.226)  0.882 ms  10.88.3.216 (10.88.3.216)  0.462 ms  0.285 ms
 3  *  *  *
 4  138.197.245.101 (138.197.245.101)  1.507 ms  138.197.245.105 (138.197.245.105)  1.506 ms  1.481 ms
 5  209.85.149.170 (209.85.149.170)  1.701 ms  138.68.33.9 (138.68.33.9)  3.361 ms  3.371 ms
 6  *  *  *
 7  108.170.242.241 (108.170.242.241)  2.853 ms  108.170.243.1 (108.170.243.1)  2.954 ms  108.170.242.241 (108.170.242.241)  2.529 ms
 8  108.170.242.253 (108.170.242.253)  1.562 ms  108.170.242.254 (108.170.242.254)  2.251 ms  108.170.242.238 (108.170.242.238)  1.558 ms
 9  216.239.58.215 (216.239.58.215)  3.034 ms  216.239.62.41 (216.239.62.41)  3.612 ms  74.125.253.190 (74.125.253.190)  2.481 ms
10  142.250.237.174 (142.250.237.174)  9.779 ms  9.110 ms  142.251.51.40 (142.251.51.40)  9.669 ms
11  142.250.235.188 (142.250.235.188)  43.552 ms  142.250.235.178 (142.250.235.178)  40.428 ms  142.250.235.182 (142.250.235.182)  42.095 ms
12  142.251.67.131 (142.251.67.131)  60.385 ms  142.251.67.137 (142.251.67.137)  57.318 ms  142.251.67.133 (142.251.67.133)  50.817 ms
13  142.251.78.158 (142.251.78.158)  63.228 ms  *  142.251.65.6 (142.251.65.6)  63.972 ms
14  142.250.236.146 (142.250.236.146)  64.887 ms  142.251.49.198 (142.251.49.198)  64.497 ms  *
15  142.251.49.163 (142.251.49.163)  64.558 ms  216.239.35.163 (216.239.35.163)  64.844 ms  216.239.63.233 (216.239.63.233)  64.424 ms
16  108.170.246.1 (108.170.246.1)  63.924 ms  108.170.240.97 (108.170.240.97)  64.097 ms  108.170.246.1 (108.170.246.1)  63.445 ms
17  142.251.77.63 (142.251.77.63)  63.424 ms  63.437 ms  142.251.77.65 (142.251.77.65)  63.428 ms
18  den16s02-in-f14.1e100.net (172.217.1.206)  63.513 ms  63.371 ms  63.381 ms

~~~

- 结论：host模式，容器网络直接通过eth0出去。网卡复制物理机状态。


- Bridge 模式讨论

进入容器内部：

~~~

root@e91318faca3f:/# traceroute -4 google.com
traceroute to google.com (142.251.45.14), 30 hops max, 60 byte packets
 1  172.17.0.1 (172.17.0.1)  0.034 ms  0.014 ms  0.009 ms
 2  * * *
 3  10.88.3.220 (10.88.3.220)  1.384 ms 10.88.3.226 (10.88.3.226)  2.042 ms 10.88.3.220 (10.88.3.220)  1.349 ms
 4  * * *
 5  138.197.245.105 (138.197.245.105)  2.464 ms  2.466 ms  2.455 ms
 6  138.68.33.9 (138.68.33.9)  3.915 ms  3.234 ms  3.192 ms
 7  * * *
 8  142.251.224.30 (142.251.224.30)  2.233 ms 108.170.243.1 (108.170.243.1)  3.434 ms 142.251.228.80 (142.251.228.80)  1.808 ms
 9  108.170.242.253 (108.170.242.253)  3.581 ms 108.170.242.238 (108.170.242.238)  2.182 ms  1.994 ms
10  74.125.253.190 (74.125.253.190)  2.428 ms 216.239.62.41 (216.239.62.41)  2.840 ms  2.806 ms
11  142.250.237.174 (142.250.237.174)  9.828 ms 142.251.51.43 (142.251.51.43)  9.896 ms 142.251.51.40 (142.251.51.40)  9.871 ms
12  142.250.235.178 (142.250.235.178)  40.617 ms  40.610 ms 142.250.235.182 (142.250.235.182)  41.420 ms
13  142.251.67.131 (142.251.67.131)  50.227 ms 142.251.67.141 (142.251.67.141)  50.465 ms 142.251.67.133 (142.251.67.133)  50.489 ms
14  142.251.65.6 (142.251.65.6)  64.262 ms *  64.022 ms
15  209.85.247.106 (209.85.247.106)  65.462 ms 142.251.49.94 (142.251.49.94)  64.392 ms 142.251.49.166 (142.251.49.166)  63.906 ms
16  142.251.49.70 (142.251.49.70)  64.248 ms 209.85.244.166 (209.85.244.166)  64.409 ms 142.251.49.70 (142.251.49.70)  64.261 ms
17  108.170.240.97 (108.170.240.97)  65.429 ms 108.170.246.1 (108.170.246.1)  63.435 ms  63.378 ms
18  142.251.70.83 (142.251.70.83)  63.278 ms  63.346 ms 142.251.70.85 (142.251.70.85)  63.621 ms
19  iad66s01-in-f14.1e100.net (142.251.45.14)  64.041 ms  63.717 ms  63.732 ms

~~~


通过跟踪路由，网络经过172.17.0.1转发出去，而172.17.0.1是docker的地址。

查看容器ip，172.17.0.2跟docker的ip地址一个网段，故可以相互通信

~~~

eth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 172.17.0.2  netmask 255.255.0.0  broadcast 0.0.0.0
        inet6 fe80::42:acff:fe11:2  prefixlen 64  scopeid 0x20<link>
        ether 02:42:ac:11:00:02  txqueuelen 0  (Ethernet)
        RX packets 2342  bytes 9248146 (8.8 MiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 1946  bytes 728759 (711.6 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10<host>
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 6  bytes 504 (504.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 6  bytes 504 (504.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

~~~


主机路由表如下：

~~~

[root@ssd ~]# route
Kernel IP routing table
Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
default         gateway         0.0.0.0         UG    0      0        0 eth0
10.48.0.0       0.0.0.0         255.255.0.0     U     0      0        0 eth0
10.124.0.0      0.0.0.0         255.255.240.0   U     0      0        0 eth1
165.232.128.0   0.0.0.0         255.255.240.0   U     0      0        0 eth0
172.17.0.0      0.0.0.0         255.255.0.0     U     0      0        0 docker0
~~~

根据路由表，容器先经过docker0接口，再经过gateway转发出去。


