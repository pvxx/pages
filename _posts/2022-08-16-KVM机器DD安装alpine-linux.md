### 1、安装

- 安装说明如下

[官方教程](https://wiki.alpinelinux.org/wiki/Replacing_non-Alpine_Linux_with_Alpine_remotely)

### 2、 升级

```
setup-apkrepos #选择e编辑
```
软件源如下：

```
#/media/vda/apks
http://dl-4.alpinelinux.org/alpine/v3.9/main
http://dl-4.alpinelinux.org/alpine/v3.5/community
#http://dl-4.alpinelinux.org/alpine/edge/main
#http://dl-4.alpinelinux.org/alpine/edge/community
#http://dl-4.alpinelinux.org/alpine/edge/testing

http://dl-cdn.alpinelinux.org/alpine/v3.9/main
#http://dl-cdn.alpinelinux.org/alpine/v3.5/community
#http://dl-cdn.alpinelinux.org/alpine/edge/main
#http://dl-cdn.alpinelinux.org/alpine/edge/community
#http://dl-cdn.alpinelinux.org/alpine/edge/testing


```
执行如下命令进行升级：
```
apk update
apk add --upgrade apk-tools
apk upgrade --available
sync
reboot
```

### 3、开放ssh


- 注意：需要进vnc修改ssh的权限 ，这样才能放开ssh登录

~~~
PermitRootLogin yes
PermitPasswordLogin yes
~~~


### 4、其它步骤

- 安装官方要求安装后，制作成镜像。
-安装docker，需要修改 

~~~
vi /etc/apk/repositories
http://dl-cdn.alpinelinux.org/alpine/v3.5/main
#http://dl-cdn.alpinelinux.org/alpine/v3.5/community 取消注释
#http://dl-cdn.alpinelinux.org/alpine/edge/main
#http://dl-cdn.alpinelinux.org/alpine/edge/community 取消注释
#http://dl-cdn.alpinelinux.org/alpine/edge/testing
~~~

~~~
apk update
apk add docker
rc-update add docker boot
service docker start
apk add docker-compose


~~~

安装docker

https://wiki.alpinelinux.org/wiki/Docker
