Docker :

Web 端口：33333   默认账号密码 admin

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
