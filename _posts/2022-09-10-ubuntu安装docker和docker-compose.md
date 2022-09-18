### 使用脚本安装

```
apt install -y curl
curl -fsSL https://get.docker.com | bash -s docker
```

### 如果是国内VPS，使用阿里云镜像加速

```
curl -fsSL https://get.docker.com | bash -s docker --mirror Aliyun
```

### 安装docker-compose

```
# 下载最新版本的 docker-compose 到 /usr/bin 目录下
curl -L https://github.com/docker/compose/releases/download/v2.11.0/docker-compose-`uname -s`-`uname -m` -o /usr/bin/docker-compose
# 给 docker-compose 授权
chmod +x /usr/bin/docker-compose
```

