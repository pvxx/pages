
一共两行代码

```
docker run --network host   -d --restart=always  pppv/kcptun server -c  "/etc/config.json" 
docker run -d  --privileged=true  --restart unless-stopped --net=host pppv/xx

```


使用windows版本的kcptun 


https://github.com/dfdragon/kcptun_gclient/releases/download/v1.1.3/kcptun_gclientv.1.1.3.zip



```

"client_windows_amd64.exe" -l :8888 -r my.domain.com:12345 -key "passwd" -crypt none -mode fast3

```

V2rayNG 直接粘贴配置

```
ss://YWVzLTI1Ni1nY206NkxzWTc3QzhwRQ==@127.0.0.1:8888#%E6%9C%AA%E5%88%86%E7%BB%84-kcptun-%E6%9C%AC%E5%9C%B0

```
