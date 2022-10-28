> Linux 服务器 VPS 一键 DDNS 脚本, Cloudflare DDNS 脚本, 服务器 VPS 自动换 IP 后 Cloudflare 自动解析, CF 自动解析动态 IP.

使用 Cloudflare API，支持 IPv4，轻松地更新 DNS，还可添加定时任务达到自动更新 IP 的目的。

下载脚本[](#下载脚本)
-------------

```
wget -qO /usr/local/bin/cf-ddns.sh https://raw.githubusercontent.com/pvxx/tools/main/cloudflare%20ddns/cf-ddns.sh
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
#!/bin/bash
# CHANGE THESE
auth_email="user@example.com"
auth_key="c2547eb745079dac9320b638f5e225cf483cc5cfdda41" # found in cloudflare account settings
zone_name="example.com"
record_name="www.example.com"

# MAYBE CHANGE THESE
ip=$(curl -s http://ipv4.icanhazip.com)
ip_file="ip.txt"
id_file="cloudflare.ids"
log_file="cloudflare.log"

# LOGGER
log() {
    if [ "$1" ]; then
        echo -e "[$(date)] - $1" >> $log_file
    fi
}

# SCRIPT START
log "Check Initiated"

if [ -f $ip_file ]; then
    old_ip=$(cat $ip_file)
    if [ $ip == $old_ip ]; then
        echo "IP has not changed."
        exit 0
    fi
fi

if [ -f $id_file ] && [ $(wc -l $id_file | cut -d " " -f 1) == 2 ]; then
    zone_identifier=$(head -1 $id_file)
    record_identifier=$(tail -1 $id_file)
else
    zone_identifier=$(curl -s -X GET "https://api.cloudflare.com/client/v4/zones?name=$zone_name" -H "X-Auth-Email: $auth_email" -H "X-Auth-Key: $auth_key" -H "Content-Type: application/json" | grep -Po '(?<="id":")[^"]*' | head -1 )
    record_identifier=$(curl -s -X GET "https://api.cloudflare.com/client/v4/zones/$zone_identifier/dns_records?name=$record_name" -H "X-Auth-Email: $auth_email" -H "X-Auth-Key: $auth_key" -H "Content-Type: application/json"  | grep -Po '(?<="id":")[^"]*')
    echo "$zone_identifier" > $id_file
    echo "$record_identifier" >> $id_file
fi

update=$(curl -s -X PUT "https://api.cloudflare.com/client/v4/zones/$zone_identifier/dns_records/$record_identifier" -H "X-Auth-Email: $auth_email" -H "X-Auth-Key: $auth_key" -H "Content-Type: application/json" --data "{\"id\":\"$zone_identifier\",\"type\":\"A\",\"name\":\"$record_name\",\"content\":\"$ip\"}")

if [[ $update == *"\"success\":false"* ]]; then
    message="API UPDATE FAILED. DUMPING RESULTS:\n$update"
    log "$message"
    echo -e "$message"
    exit 1 
else
    message="IP changed to: $ip"
    echo "$ip" > $ip_file
    log "$message"
    echo "$message"
fi

```

shell
