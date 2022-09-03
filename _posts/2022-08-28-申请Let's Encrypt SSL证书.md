

环境：centos7

- 安装socat模块

~~~
yum install socat -y
~~~

- 安装acme.sh

通过下面命令安装 acme.sh ，Email 用来接收重要重要通知，如证书快到期未更新会收到通知。

~~~
curl https://get.acme.sh | sh -s email=my@example.com
~~~

执行以下命令，更新环境变量

~~~
source ~/.bashrc
~~~

生成证书

~~~
cd  /root/.acme.sh
acme.sh --issue -d example.com -d "*.example.com" --dns \
--yes-I-know-dns-manual-mode-enough-go-ahead-please
~~~

按照说明增加txt记录，继续执行以下命令：

~~~
acme.sh --renew -d example.com -d "*.example.com" \
--yes-I-know-dns-manual-mode-enough-go-ahead-please
~~~

证书放在 /root/.acme.sh/domain 内,重命名拷贝到root目录，都是pem格式

~~~
acme.sh --install-cert -d domain.com \
--key-file       /root/key.pem  \
--fullchain-file /root/cert.pem
~~~

参考： https://zhuanlan.zhihu.com/p/445852299

https://github.com/acmesh-official/acme.sh

