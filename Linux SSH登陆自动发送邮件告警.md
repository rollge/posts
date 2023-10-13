---
title: "Linux SSH登陆自动发送邮件告警"
date: 2023-10-13T10:58:34+08:00
draft: false
tags: [linux, ssh, curl, email]
description: "本文介绍如何在Linux系统中，使用SSH登陆时，自动发送邮件告警。我们介绍如何仅使用curl命令，实现SSH登陆时，自动发送邮件告警。"
---

本文介绍如何在Linux系统中，使用SSH登陆时，自动发送邮件告警。我们介绍如何仅使用curl命令，实现SSH登陆时，自动发送邮件告警。

**环境：**
```sh
OS: Debian 12
```

**如果没有curl，先安装：**
```sh
sudo apt install curl
```

**创建发送邮件脚本：**
```sh
#!/bin/bash

## IP白名单，在白名单内的IP地址登录后不需要报警
trustedIp=(
    "192.168.15.15"
)

alert() {
    hname=`hostnamectl hostname`  ## 获取主机名

    msg="User [$USER] just logged in [$hname] from [$1]" ## 报警的信息内容

    ## 邮件内容
    email_content='From: notify <notify@163.com>
To: Alert <alert@163.com>
Subject: SSH Alert
Date: '"$(date -R)"'

'"${msg}"'
';


    ## 使用curl发送邮件
    ## 需要知道你的邮箱的smtp服务器地址，端口，用户名，密码
    ## 本文使用的是163邮箱，如果你使用的是其他邮箱，需要修改下面的参数
    echo "$email_content" | curl -s --url 'smtps://smtp.163.com:465' \
        --mail-from "notify@163.com" \
        --mail-rcpt "alert@163.com" \
        --user 'notify@163.com:<密码>' \
        --upload-file -
}


ip=`echo $SSH_CONNECTION | cut -d " " -f 1`

shouldSend=1

for aip in ${trustedIp[@]};do
    if [ $aip == $ip ];then
        shouldSend=0
    fi
done

if [ $shouldSend == 1 ];then
    alert $ip
fi
```

将以上脚本引入到`~/.ssh/rc`中即可，每次SSH登陆时，都会执行该脚本，发送邮件告警。



---
作者：卷哥Rollge

发表时间：2023年10月13日 10:58:34

勘误：[me@rollge.com](mailto:me@rollge.com)

**转载请注明来源，本文遵循 [CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.zh)**