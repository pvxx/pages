## 方案一

###  place1/wg-access-server

Web 端口：33333   默认账号密码 admin

[https://github.com/Place1/wg-access-server](https://github.com/Place1/wg-access-server)

~~~
docker run \
  -it \
   -d \
  --cap-add NET_ADMIN \
  --privileged=true \
  --device /dev/net/tun:/dev/net/tun \
  -v wg-access-server-data:/data \
  -e "WG_ADMIN_PASSWORD=admin" \
  -e "WG_WIREGUARD_PRIVATE_KEY=0E2RfeIHlvfYl19LpR/EFwO4q4SZwBRCiIhzKY4q2mo=" \
  -p 33333:8000/tcp \
  -p 51820:51820/udp \
  place1/wg-access-server
~~~



## 方案二

### weejewel/wg-easy

首先升级内核（以下是一键安装wireguard脚本，可以根据脚本研究只升级内核）：

```
curl -O https://raw.githubusercontent.com/atrandys/wireguard/master/wg_mult.sh && chmod +x wg_mult.sh && ./wg_mult.sh
```

然后一键安装docker

[https://github.com/WeeJeWel/wg-easy](https://github.com/WeeJeWel/wg-easy)

```
docker run -d \
  --name=wg-easy \
  -e WG_HOST=🚨YOUR_SERVER_IP \
  -e PASSWORD=🚨YOUR_ADMIN_PASSWORD \
  -v ~/.wg-easy:/etc/wireguard \
  -p 51820:51820/udp \
  -p 51821:51821/tcp \
  --cap-add=NET_ADMIN \
  --cap-add=SYS_MODULE \
  --sysctl="net.ipv4.conf.all.src_valid_mark=1" \
  --sysctl="net.ipv4.ip_forward=1" \
  --restart unless-stopped \
  weejewel/wg-easy
```



## wireguard配置生成器

### vx3r/wg-gen-web 【wireguard配置生成器】

```
docker run --rm -it -v /tmp/wireguard:/data -p 8080:8080 -e "WG_CONF_DIR=/data" vx3r/wg-gen-web:latest
```

