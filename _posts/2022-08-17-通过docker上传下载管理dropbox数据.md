- 主要用到的软件如下：
https://github.com/andreafabrizi/Dropbox-Uploader

- 思路：将软件做成docker，apitoken存储在docker内。通过docker进行快速上传下载管理，比较牛逼的时。更换电脑时，只需要下载docker即可，不需要重新配置api环境。

- 进入项目页，用wget先下载整个项目 

https://codeload.github.com/andreafabrizi/Dropbox-Uploader/zip/refs/heads/master

- 解压后进入项目路径，修改Dockerfile如下：

~~~
FROM alpine:3.13
RUN apk add --no-cache bash curl
COPY ./dropbox_uploader.sh /root
COPY ./dropShell.sh /root
RUN  mkdir -p /workdir

VOLUME  /workdir

WORKDIR /workdir

ENTRYPOINT ["/root/dropbox_uploader.sh"]

~~~

- 构建项目

~~~
Docker build -t image_name .
~~~

- 第一次运行项目，需要录入apitoken数据，按照要求，输入相关api数据进行认证。

~~~
docker run -i   -v /root:/workdir  image_name  list
~~~

- 然后，提交容器（带用户数据）生成镜像。

~~~
Docker commit container_id  image_name
~~~

- 将数据推送到docker hub

~~~
docker tag image_id  username/image_name
docker push username/image_name
~~~

- 上传下载数据（数据存放在/root目录）：

~~~
cd /root
docker run -i --rm  --privileged=true  -v /root:/workdir  image_name upload  local_file remote_dir
docker run -i --rm  --privileged=true  -v /root:/workdir  image_name download  remotefile
docker run -i --rm  --privileged=true  -v /root:/workdir  image_name delete   remotefile
docker run -i --rm  --privileged=true  -v /root:/workdir  image_name download  /a.txt   #举例
docker run -i --rm  --privileged=true  -v /root:/workdir  image_name upload  a.txt /    #举例
docker run -i --rm  --privileged=true  -v /root:/workdir  image_name delete  /a.txt    /举例
~~~






