- 借助 supercronic

[项目地址](https://github.com/aptible/supercronic)

- 下载改名

```
cd /root
wget https://github.com/aptible/supercronic/releases/download/v0.2.1/supercronic-linux-amd64
mv supercronic-linux-amd64 supercronic

```

- Dockerfile (存放在/root)

```
FROM python:3.7.13-alpine3.16
COPY ./hostloc_auto_get_points.py /root/hostloc_auto_get_points.py
COPY ./supercronic /usr/local/bin/supercronic
RUN chmod +x /usr/local/bin/supercronic
COPY ./my-cron /root/my-cron
CMD ["/usr/local/bin/supercronic", "/root/my-cron"]

```

- 需要下载 hostloc_auto_get_points

[下载地址](https://raw.githubusercontent.com/Jox2018/hostloc_getPoints/main/hostloc_auto_get_points.py)

```
cd root
wget https://github.com/Jox2018/hostloc_getPoints/blob/main/hostloc_auto_get_points.py

```

- 编制 my-cron (存放在/root)

```
5 4 * * * /usr/local/bin/python3   /root/hostloc_auto_get_points.py

# 通过 https://crontab.guru/ 可以获取规则

```

- docker build 

```
docker build -t python:hostloc  .

```

- 运行容器


```
docker run -d  --privileged=true  --restart unless-stopped --net=host python:hostloc

```
