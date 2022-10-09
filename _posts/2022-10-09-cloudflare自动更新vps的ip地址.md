> Linux 服务器 VPS 一键 DDNS 脚本, Cloudflare DDNS 脚本, 服务器 VPS 自动换 IP 后 Cloudflare 自动解析, CF 自动解析动态 IP.

使用 Cloudflare API，支持 IPv4，轻松地更新 DNS，还可添加定时任务达到自动更新 IP 的目的。

下载脚本[](#下载脚本)
-------------

```
1wget -qO /usr/local/bin/cf-ddns.sh https://moththe.com/posts/cf_ddns/cf-ddns.sh

```

使用方法[](#使用方法)
-------------

1. 修改脚本中 `auth_email`,`auth_key`,`zone_name`,`record_name` 对应内容为自己的信息

2. 给脚本执行权限 `chmod +x /usr/local/bin/cf-ddns.sh`

3. 执行 `crontab -e` 并添加以下定时任务：

   ```
   */1 * * * * /usr/local/bin/cf-ddns.sh  #每分钟执行一次
   ```

   

> 怎么获取我的 Cloudflare API？ 访问 [https://www.cloudflare.com/a/account/my-account](https://www.cloudflare.com/a/account/my-account) ，在上方选择 API Tokens 标签页，然后查看页面下方的 Global API Key。

脚本内容[](#脚本内容)
-------------

```
 1#!/bin/bash
 2
 3# CHANGE THESE
 4auth_email="user@example.com"
 5auth_key="c2547eb745079dac9320b638f5e225cf483cc5cfdda41" # found in cloudflare account settings
 6zone_
 7record_
 8
 9# MAYBE CHANGE THESE
10ip=$(curl -s http://ipv4.icanhazip.com)
11ip_file="ip.txt"
12id_file="cloudflare.ids"
13log_file="cloudflare.log"
14
15# LOGGER
16log() {
17    if [ "$1" ]; then
18        echo -e "[$(date)] - $1" >> $log_file
19    fi
20}
21
22# SCRIPT START
23log "Check Initiated"
24
25if [ -f $ip_file ]; then
26    old_ip=$(cat $ip_file)
27    if [ $ip == $old_ip ]; then
28        echo "IP has not changed."
29        exit 0
30    fi
31fi
32
33if [ -f $id_file ] && [ $(wc -l $id_file | cut -d " " -f 1) == 2 ]; then
34    zone_identifier=$(head -1 $id_file)
35    record_identifier=$(tail -1 $id_file)
36else
37    zone_identifier=$(curl -s -X GET "https://api.cloudflare.com/client/v4/zones? | grep -Po '(?<="id":")[^"]*' | head -1 )
38    record_identifier=$(curl -s -X GET "https://api.cloudflare.com/client/v4/zones/$zone_identifier/dns_records?  | grep -Po '(?<="id":")[^"]*')
39    echo "$zone_identifier" > $id_file
40    echo "$record_identifier" >> $id_file
41fi
42
43update=$(curl -s -X PUT "https://api.cloudflare.com/client/v4/zones/$zone_identifier/dns_records/$record_identifier" -H "X-Auth-Email: $auth_email" -H "X-Auth-Key: $auth_key" -H "Content-Type: application/json" --data "{\"id\":\"$zone_identifier\",\"type\":\"A\",\"name\":\"$record_name\",\"content\":\"$ip\"}")
44
45if [[ $update == *"\"success\":false"* ]]; then
46    message="API UPDATE FAILED. DUMPING RESULTS:\n$update"
47    log "$message"
48    echo -e "$message"
49    exit 1 
50else
51    message="IP changed to: $ip"
52    echo "$ip" > $ip_file
53    log "$message"
54    echo "$message"
55fi


```

shell