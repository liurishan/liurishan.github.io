---
layout:     post                    # 使用的布局（不需要改）
title:      ubuntu和win10双系统时间同步问题               # 标题 
subtitle:   时间同步 #副标题
date:       2020-08-01              # 时间
author:     Jasper                     # 作者
header-img: img/224939-1582123779c853.jpg    #这篇文章标题背景图片
catalog: true                       # 是否归档
tags:                               #标签
    - 双系统，时间同步
---

### Windows10和Ubuntu20.04时间同步

~~~
sudo apt_get update

timedatectl set-local-rtc 1 --adjust-system-clock

sudo apt-get install ntpdate

sudo ntpdate time.windows.com

sudo hwclock --localtime --systohc
~~~
