- 安装说明如下

[官方教程](https://wiki.alpinelinux.org/wiki/Replacing_non-Alpine_Linux_with_Alpine_remotely)

- 安装官方要求安装后，制作成镜像。

- 注意：需要进vnc修改ssh的权限 ，这样才能放开ssh登录

~~~
PermitRootLogin yes
PermitPasswordLogin yes
~~~

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
