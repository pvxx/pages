创建1个文件夹，需要生成证书放里面

~~~
ls /root/config
cert.pem
key.pem
default.conf
~~~

运行vaultwarden

~~~
docker run -d --name vaultwarden -v /vw-data/:/data/ -p 8080:80 -p 3013:3012 vaultwarden/server:latest
~~~

运行nginx

~~~
docker run --name nginx --privileged=true -p 80:80 -p 443:443   -v /root/config/default.conf:/etc/nginx/conf.d/default.conf -v /root/config/:/etc/nginx/cert/ -d nginx
~~~



default.conf 内容如下，域名需要修改。

~~~
# The `upstream` directives ensure that you have a http/1.1 connection
# This enables the keepalive option and better performance
#
# Define the server IP and ports here.
upstream vaultwarden-default {
  zone vaultwarden-default 64k;
  server pass.pvxx.com:8080;
  keepalive 2;
}
upstream vaultwarden-ws {
  zone vaultwarden-ws 64k;
  server pass.pvxx.com:3013;
  keepalive 2;
}

# Redirect HTTP to HTTPS
server {
    listen 80;
    server_name pass.pvxx.com;
    return 301 https://$host$request_uri;
}

server {
    listen 443 ssl http2;
    listen [::]:443 ssl http2;
    server_name pass.pvxx.com;

    # Specify SSL Config when needed
    ssl_certificate /etc/nginx/cert/cert.pem;
    ssl_certificate_key /etc/nginx/cert/key.pem;

    client_max_body_size 128M;

    location / {
      proxy_http_version 1.1;
      proxy_set_header "Connection" "";

      proxy_set_header Host $host;
      proxy_set_header X-Real-IP $remote_addr;
      proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
      proxy_set_header X-Forwarded-Proto $scheme;

      proxy_pass http://vaultwarden-default;
    }

    location /notifications/hub/negotiate {
      proxy_http_version 1.1;
      proxy_set_header "Connection" "";

      proxy_set_header Host $host;
      proxy_set_header X-Real-IP $remote_addr;
      proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
      proxy_set_header X-Forwarded-Proto $scheme;

      proxy_pass http://vaultwarden-default;
    }

    location /notifications/hub {
      proxy_http_version 1.1;
      proxy_set_header Upgrade $http_upgrade;
      proxy_set_header Connection "upgrade";

      proxy_set_header Host $host;
      proxy_set_header X-Real-IP $remote_addr;
      proxy_set_header Forwarded $remote_addr;
      proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
      proxy_set_header X-Forwarded-Proto $scheme;

      proxy_pass http://vaultwarden-ws;
    }

    # Optionally add extra authentication besides the ADMIN_TOKEN
    # Remove the comments below `#` and create the htpasswd_file to have it active
    #
    #location /admin {
    #  # See: https://docs.nginx.com/nginx/admin-guide/security-controls/configuring-http-basic-authentication/
    #  auth_basic "Private";
    #  auth_basic_user_file /path/to/htpasswd_file;
    #
    #  proxy_http_version 1.1;
    #  proxy_set_header "Connection" "";
    #
    #  proxy_set_header Host $host;
    #  proxy_set_header X-Real-IP $remote_addr;
    #  proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    #  proxy_set_header X-Forwarded-Proto $scheme;
    #
    #  proxy_pass http://vaultwarden-default;
    #}
}
~~~

---------------------------------------------------------------------------------
个人搭建：系统ubuntu 18.04

/root/config文件夹只放证书，default.conf已存放到容器内。

~~~
mkdir /root/config
wget https://raw.githubusercontent.com/pvxx/vaultwarden/main/mypasswd.net/cert.pem -O /root/config/cert.pem
wget https://raw.githubusercontent.com/pvxx/vaultwarden/main/mypasswd.net/key.pem -O /root/config/key.pem
wget https://raw.githubusercontent.com/pvxx/vaultwarden/main/mypasswd.net/default.conf -O /root/config/default.conf
docker run --name nginx --privileged=true -p 80:80 -p 443:443  -v /root/config/default.conf:/etc/nginx/conf.d/default.conf  -v /root/config/:/etc/nginx/cert/ -d pppv/nginx
docker run -d --name vaultwarden  --privileged=true -v /vw-data/:/data/ -p 8080:80 -p 3013:3012  pppv/vaultwarden
~~~

[参考https://github.com/dani-garcia/vaultwarden](https://github.com/dani-garcia/vaultwarden)
