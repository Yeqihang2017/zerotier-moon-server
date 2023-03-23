# zerotier-moon-server
用腾讯云搭建zerotier moon中转服务器



## 前言

本文参考[Zerotier Moon搭建教程 - 腾讯云开发者社区-腾讯云 (tencent.com)](https://cloud.tencent.com/developer/article/2011784)

并结合个人遇到的问题给出解决方案

***



## 安装

### Step 1：安装zerotier-one

`curl -s https://install.zerotier.com | sudo bash`

安装完成后输入

`zerotier -v`

查看安装是否成功

### Step 2：加入虚拟网络

输入

`zerotier-cli join xxxxxxxx`

加入zerotier网络，其中xxxxxxxx为你自己的网络id

> ps:
>
> 如果提示：zerotier-cli: missing port and zerotier-one.port not found in /var/lib/zerotier-one
>
> 则使用指令
>
> `zerotier-one -d`
>
> 启动zerotier服务再使用zerotier-cli
>
> 如果在使用`zerotier-one -d`指令时出现错误：fatal error: cannot bind to local control interface port 9993
>
> 则尝试修改默认端口，例如通过在目录*/var/lib/zerotier-one*中新建*local.conf*文件并复制以下代码，修改端口9994为默认端口
>
> ```
> {
>   "settings": {
>     "primaryPort": 9994
>   }
> }
> ```

### Step 3:配置moon

进入zerotier-one目录

`cd /var/lib/zerotier-one`

生成moon.json配置文件

`zerotier-idtool initmoon identity.public >> moon.json`

使用vim编辑moon.json配置文件

`vim moon.json`

找到stableEndpoints项并编辑

`"stableEndpoints": ["你服务器公网ip/默认端口"]`

> ps:
>
> 默认端口为9993
>
> 如果你在Step 2修改了默认端口，则改为你的默认端口

生成.moon文件

`zerotier-idtool genmoon moon.json`

将生成的 000000xxxxxxxxxx.moon 移动到 `moons.d` 目录

`mkdir moons.d`

`mv 000000xxxxxxxxxx.moon moons.d`

### Step 4:重启zerotier服务

`systemctl restart zerotier-one`

