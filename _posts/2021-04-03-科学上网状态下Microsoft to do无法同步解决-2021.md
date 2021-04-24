---
layout:     post                    # 使用的布局（不需要改）
title:      Microsoft to do无法同步解决             # 标题 
subtitle:   Microsoft to do无法同步解决 #副标题
date:       2021-04-03         # 时间
author:     Jasper                   # 作者
header-img: img/images.jfif    #这篇文章标题背景图片
catalog: true                       # 是否归档
tags:                               #标签
    - 同步问题， Microsoft to do
---

# Microsoft to do无法同步解决
<font size=5>[参考博客](https://zhuanlan.zhihu.com/p/321254257)</font>

**最近发现一款非常好用的多平台任务管理的软件——Microsoft to do，它可以再Windows、ios、Android上使用，多平台的同步简直不要太爽，并且简洁的界面深得我的喜爱。但是我最近发现再魔法上网的时候会发生windows上无法同步的问题，在网上搜索了一下，找到了合适的解决办法，遂记录下来，方便日后查询。**

# 解决办法
通过注册表找到 To Do软件的SID
1. 使用“Win+R”快捷键打开运行，输入“regedit”打开注册表，进入到注册表的以下目录：
~~~
\HKEY_CURRENT_USER\SOFTWARE\Classes\Local Settings\Software\Microsoft\Windows\CurrentVersion\AppContainer\Mappings
~~~
2. 通过DisplayName找到todo应用，左侧的“S-1-15-……”就是SID，复制下来
3. 以管理员方式打开Windows PowerShell或CMD
执行以下命令：
~~~
CheckNetIsolation.exe loopbackexempt -a -p=S-1-15-2……
~~~
4. 验证
执行命令
~~~
CheckNetIsolation.exe LoopbackExempt -s
~~~
**成功了，可以打开Microsoft To Do软件试一下，发现就可以同步了，而且速度也非常快！**
