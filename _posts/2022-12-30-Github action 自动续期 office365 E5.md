---
titile: github actions自动调用api续期Microsoft 365 E5
---





#### 1、简介



https://github.com/wangziyingwen/AutoApiSecret



项目说明中有完整的文档说明！博主，自己走了一遍教程，并记录在此。



#### 2、准备



1）E5 管理账号

2）Github 账号（不建议用常用账号）

3）记事本（用于记录 ID 密码等！）

#### 3、登录以及授权等

这里默认你已经拥有了 E5。如果没有，需要跟着前面的教程申请一个。

**1）登陆 E5 管理账号注册应用。**

登录地址：https://portal.azure.com/ 

登录后，首页找到【Azure Active Directory】→【应用注册】→ 【新注册】

**2）注册应用程序的具体填写**



2.1【名称】 按需填写，受支持的账户类型 选择【**任何组织目录 (任何 Azure AD 目录 – 多租户) 中的帐户】**



2.2【重定向 URI】填写：http://localhost:53682/



2.3 最后点击【注册】，具体看图。



**3）记录【应用程序 (客户端)ID】**



注册成功后，将【应用程序 (客户端)ID】记录下来，后面要用到！

**4）添加权限**





点击左侧菜单【API 权限】→【添加权限】 →【Microsoft Graph】→ 选中【委托的权限】，以下权限分别搜索勾选！勾选完点击按钮【添加权限】





- **Files（Files.Read.All   Files.ReadWrite.All）**
- **Sites（Sites.Read.All   Sites.ReadWrite.All）**
- **User（User.Read.All   User.ReadWrite.All）**
- **Directory（Directory.Read.All   Directory.ReadWrite.All）**
- **Mail（Mail.Read   Mail.ReadWrite）**
- **MailboxSettings（MailboxSettings.Read   MailboxSettings.ReadWrite）**





具体看图：





[![img](https://www.daniao.org/wp-content/uploads/2020/06/github-action-e5-5-min.png)](https://www.daniao.org/wp-content/uploads/2020/06/github-action-e5-5-min.png)





**5）同意授权**



在 API 权限页面，如果界面上有【代表 xxx 授予管理员同意】按钮，一定要点一下，然后同意授权！如果没有这个按钮，就不用管了！





[![img](https://www.daniao.org/wp-content/uploads/2020/06/github-action-e5-6-min.png)](https://www.daniao.org/wp-content/uploads/2020/06/github-action-e5-6-min.png)





同意授权后，如图：





[![img](https://www.daniao.org/wp-content/uploads/2020/06/github-action-e5-7-min.png)](https://www.daniao.org/wp-content/uploads/2020/06/github-action-e5-7-min.png)





#### 4、证书和密码



**1）点击左侧菜单【证书和密码】-> 【+ 新客户端密码】**



【说明】随便填，【截止期限】随便选！点击【添加】按钮。



然后页面下方可见新建的密码，然后将【值】复制记录下来！后面会用到！





[![img](https://www.daniao.org/wp-content/uploads/2020/06/github-action-e5-8-min.png)](https://www.daniao.org/wp-content/uploads/2020/06/github-action-e5-8-min.png)







[![img](https://www.daniao.org/wp-content/uploads/2020/06/github-action-e5-9-min.png)](https://www.daniao.org/wp-content/uploads/2020/06/github-action-e5-9-min.png)





#### 5、利用 rclone 来获取 Token!



**1）下载 rclone 以及获取 token**



【[下载 rclone](https://downloads.rclone.org/v1.55.1/rclone-v1.55.1-windows-amd64.zip)】到电脑某个盘符下，在 rclone.exe 同目录中，按 Shift + 鼠标右键，选择在【此处打开 cmd 窗口】或【在此处打开 power shell 窗口】





[![img](https://www.daniao.org/wp-content/uploads/2020/06/github-action-e5-10-min.png)](https://www.daniao.org/wp-content/uploads/2020/06/github-action-e5-10-min.png)





然后在弹出执行命令！



```
	./rclone authorize "onedrive" "应用程序(客户端)ID" "应用程序密码"
```



**注意**：ID 和密码，就是你刚刚保存在记事本的。



执行命令后弹出网页登陆 E5 管理账号，然后接受授权即可！授权成功，如图：





[![img](https://www.daniao.org/wp-content/uploads/2020/06/github-action-e5-11-min.png)](https://www.daniao.org/wp-content/uploads/2020/06/github-action-e5-11-min.png)





**2）授权成功后，窗口弹出得到的 Token 信息！复制内容**



仅复制 【Paste the following into your remote machine —>】开头【<—End paste】结尾的中间部分内容！





![img](https://cdn.jsdelivr.net/gh/IMGRU/IMG/2020/06/12/5ee339ce66214.png)





**3）格式化 token**



利用搜索引擎找一个【JSON 在线格式】的网站，将复制的内容格式化一下。也可以在谷歌应用商店安装 JSON-handle 【[下载地址](https://chrome.google.com/webstore/detail/json-handle/iahnhfdhidomcpggpaimmmahffihkfnj?hl=zh-CN)】，格式化后复制 refresh_token 值内容！不要双引号！保留 token，后面会用到。





[![img](https://www.daniao.org/wp-content/uploads/2020/06/github-action-e5-13-min.png)](https://www.daniao.org/wp-content/uploads/2020/06/github-action-e5-13-min.png)





#### 6、克隆项目



可以直接 fork 项目，但是为了安全，还是建一个私有仓库更稳一点。



**1）登陆 Github 账号，新建项目（ New repository）！**



名称随便，可设置为私有 (Private)，推荐私有。





[![img](https://www.daniao.org/wp-content/uploads/2020/06/github-action-e5-14-min.png)](https://www.daniao.org/wp-content/uploads/2020/06/github-action-e5-14-min.png)





**2）Import 导入一个项目！**



项目地址：https://github.com/wangziyingwen/AutoApiSecret





[![img](https://www.daniao.org/wp-content/uploads/2020/06/github-action-e5-15-min.png)](https://www.daniao.org/wp-content/uploads/2020/06/github-action-e5-15-min.png)







[![img](https://www.daniao.org/wp-content/uploads/2020/06/github-action-e5-16-min.png)](https://www.daniao.org/wp-content/uploads/2020/06/github-action-e5-16-min.png)





导入的过程略慢！，完成导入如上图。



#### 7、配置参数



**1）在线编辑你项目里的 1.txt 文件，将整个 refresh_token 覆盖粘贴进去（原内容不要保留）。**



**2）依次点击上栏 【Setting】-> 【Secrets】 -> 【Add a new secret】**





[![img](https://www.daniao.org/wp-content/uploads/2020/06/github-action-e5-17-min.png)](https://www.daniao.org/wp-content/uploads/2020/06/github-action-e5-17-min.png)





**3）分别新建：CONFIG_ID、CONFIG_KEY**



替换你的 ID 和 密码！一定要注意前面 r 和 单引号！



**3.1 ) CONFIG_ID**



具体设置看图：





[![img](https://www.daniao.org/wp-content/uploads/2020/06/github-action-e5-18-min.png)](https://www.daniao.org/wp-content/uploads/2020/06/github-action-e5-18-min.png)





**3.2 ) CONFIG_KEY**



具体设置看图：

[![img](https://www.daniao.org/wp-content/uploads/2020/06/github-action-e5-19-min.png)](https://www.daniao.org/wp-content/uploads/2020/06/github-action-e5-19-min.png)





全部设置好如图：





[![img](https://www.daniao.org/wp-content/uploads/2020/06/github-action-e5-20-min.png)](https://www.daniao.org/wp-content/uploads/2020/06/github-action-e5-20-min.png)





#### 8、Generate token



**1）打开地址：https://github.com/settings/tokens 点击【Generate new token】**





[![img](https://www.daniao.org/wp-content/uploads/2020/06/github-action-e5-21-min.png)](https://www.daniao.org/wp-content/uploads/2020/06/github-action-e5-21-min.png)





**2 ) 设置名字：GITHUB_TOKEN** 



勾选 repo , admin:repo_hook , workflow 等选项，点击按钮【Generate token】！





[![img](https://www.daniao.org/wp-content/uploads/2020/06/github-action-e5-22-min.png)](https://www.daniao.org/wp-content/uploads/2020/06/github-action-e5-22-min.png)

[![img](https://www.daniao.org/wp-content/uploads/2020/06/github-action-e5-23-min.png)](https://www.daniao.org/wp-content/uploads/2020/06/github-action-e5-23-min.png)





#### 9、测试效果



点击一下 Github 项目的星星（Star）会调用一次脚本，再点击上面的【Action】就能看到每次的运行日志，具体看图：



**运行中如图：**





[![img](https://www.daniao.org/wp-content/uploads/2020/06/github-action-e5-24-min.png)](https://www.daniao.org/wp-content/uploads/2020/06/github-action-e5-24-min.png)





**运行成功如图：**





[![img](https://www.daniao.org/wp-content/uploads/2020/06/github-action-e5-25-min.png)](https://www.daniao.org/wp-content/uploads/2020/06/github-action-e5-25-min.png)





**如图展示日志，说明成功啦！！**





[![img](https://www.daniao.org/wp-content/uploads/2020/06/github-action-e5-26-min.png)](https://www.daniao.org/wp-content/uploads/2020/06/github-action-e5-26-min.png)





因为只是测试成功，项目设定是每 3 小时运行一次，所以 3 小时候后再确认下是否自动运行了（ation 里是否多出来几个）。不过测试成功的话应该是没问题了，过了 3 个小时候，在截图确认。运行了一天，发现运行的很好，如图：





[![img](https://www.daniao.org/wp-content/uploads/2020/06/github-action-e5-29-min.png)](https://www.daniao.org/wp-content/uploads/2020/06/github-action-e5-29-min.png)





#### GithubAction 介绍



提供的虚拟环境：



· 2-core CPU · 7 GB RAM 内存 · 14 GB SSD 硬盘空间



使用限制：



- 每个仓库只能同时支持 20 个 workflow 并行。
- 每小时可以调用 1000 次 GitHub API 。
- 每个 job 最多可以执行 6 个小时。
- 免费版的用户最大支持 20 个 job 并发执行，macOS 最大只支持 5 个。
- 私有仓库每月累计使用时间为 2000 分钟，超过后 $ 0.008 / 分钟，公共仓库则无限制。



（如果用的公共仓库，按理，可以设定无限循环调用，然后 6 小时启动一次，保证 24 小时全天候调用）



#### 10、最后



- 一顿操作猛如虎，看看续期还么有，无论怎么刷 API 也不能 100% 保证能续订服务！

- 利用 github action 实现**定时自动调用 api**，保持 E5 开发活跃。

- **免费，不需要额外设备 / 服务器**，部署完不用管啦。

- 加密版，隐藏应用 id + 机密，保护账号安全。

- 项目设定每 3 小时自动运行一次，每次调用 3 轮。定时自动启动修改地方文件 .github/workflow/AutoApiSecret.yml（自行百度 cron 定时任务格式，最短每 5 分钟一次）私有仓库有限制谨慎修改！！！

  