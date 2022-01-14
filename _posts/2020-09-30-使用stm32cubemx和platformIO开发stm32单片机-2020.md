---
layout:     post                    # 使用的布局（不需要改）
title:      使用stm32cubemx和platformIO开发stm32单片机               # 标题 
subtitle:   抛弃Keil吧！ #副标题
date:       2020-09-30             # 时间
author:     Jasper                   # 作者
header-img: img/204822-15674285021315.jpg    #这篇文章标题背景图片
catalog: true                       # 是否归档
tags:                               #标签
    - 开发单片机, stm32cubemx, PlatformIO, stm32
---

# 使用VScode开发stm32单片机
---
## 软件的安装

### 首先要安装STM32cubemx
* 这个软件是ST官网上免费下载的，可以用图形化界面直接生成单片机的初始化程序，使用的是hal库（比较方便，但是生成的程序比较臃肿）


### 安装VScode并安装platformIO插件
* 安装的过程非常慢，耐性等待


<font color="green">注意：hal库和CMSIS库的库函数不太一样。</font>

### 步骤
1. 先在stm32cubemx创建初始化配置工程
    * 在左侧边栏中找到system core，点击SYS，将Debug的选项改成串口线（serial wire）
    * 点击RCC，将HSE的选项改成晶振（crystal/ceramic resonator）
    * 来到project manager，在code generator中勾选“生成外设初始化——”选项

 ![配置设置](/img/生成文件配置.png)

2. 然后在工程文件创建platformio.ini，把以前的工程文件粘贴到里面。
~~~
[env:genericSTM32F103VE]
platform = ststm32
board = genericSTM32F103VE
framework = stm32cube
upload_protocol = jlink
build_flags = -g
debug_tool = jlink
[platformio]
include_dir=Inc
src_dir=Src
~~~
3. 然后就可以根据数据手册编写程序了。


## 配置platformio生成hex文件
* 首先，在工程目录下新建extra_script.py文件（和platformio.ini在同一目录下），代码如下：

~~~
Import("env")

env.AddPostAction(
    "$BUILD_DIR/${PROGNAME}.elf",
    env.VerboseAction(" ".join([
        "$OBJCOPY", "-O", "ihex", "-R", ".eeprom",
        "$BUILD_DIR/${PROGNAME}.elf", "$BUILD_DIR/${PROGNAME}.hex"
    ]), "Building $BUILD_DIR/${PROGNAME}.hex")
)
~~~

然后，在VS Code左侧项目列表打开platformio.ini文件，在最后一行增加如下代码：
~~~
extra_scripts = extra_script.py
~~~

### Include 路径问题
问题描述：创建好的工程，打开后出现无法找到Project/Inc或者Project/Src中的文件

解决方法一：找到.pio/generic.../idedata.json文件
~~~
将其中d:\\...改为D:\\
~~~

解决方法二：找到.vscode/c_cpp_properties.json文件
~~~
在"includePath": []和"browse": {"path": []}中
添加"${workspaceFolder}/**",

~~~

<font color="red">注意两种方法修改并保存后，要将修改的文件改为只读，否则platformIO插件在重启的时候将自动修改两个文件变回原来的内容。</font>
