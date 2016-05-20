---
title: Shadowsocks搭建教程
comments: true
date: 2016-05-20 16:04:48
tags:
---

选择的是[BandwagonHOST]的$2.99，节点：LA，月付，支持支付宝。

直接略过如何创建VPS，下面说一下如何配置Shadowsocks。

创建好VPS后可以收到IP、root密码及端口号，使用这些登录进服务器。

1. 各种安装。```
yum install epel-release
yum update
yum install python-setuptools m2crypto supervisor
easy_install pip
pip install shadowsocks
```
2. `vi /etc/shadowsocks.json`，然后加入```
{
    "server":"0.0.0.0",
    "server_port":8388, // 设置为自己喜欢的port
    "local_port":1080,
    "password":"yourpassword", // 设置密码
    "timeout":600,
    "method":"rc4-md5"
}
```
3. `vi /etc/supervisord.conf`，尾部加入```
[program:shadowsocks]
command=ssserver -c /etc/shadowsocks.json
autostart=true
autorestart=true
user=root
```
4. `vi /etc/rc.local`，然后加入`service supervisord start`，保存退出
5. `reboot`

完工。

对于第二步，如果想要配置多个账户的话，需要将设置改为下面这种，不过不建议。

```
    "server":"0.0.0.0",
    "local_address":"127.0.0.1",
    "local_port":1080,
    "port_password":{
        "10080":"yourpassword",
        "10081":"yourpassword",
    },
    "timeout":300,
    "method":"aes-256-cfb"
}
```

详细教程请参考：[【倾力原创】史上最详尽Shadowsocks从零开始一站式翻墙教程]





[BandwagonHOST]:https://bandwagonhost.com/index.php
[【倾力原创】史上最详尽Shadowsocks从零开始一站式翻墙教程]:http://shadowsocks.blogspot.com/2015/01/shadowsocks.html
