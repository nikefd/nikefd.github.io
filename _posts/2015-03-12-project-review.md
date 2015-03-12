---
layout: post
title: 项目经验回顾总结
category: techonology
---

###项目经验回顾总结

####项目一

pandaboard下板你都做了些什么，学会了什么东西？

参考答案：

这个是我个人简历上面的东西，这个回答的非常糟糕，根本不知道自己在讲什么

现在回顾一下，主要参考[我之前写过的一篇文章][1]

都做了些什么：

 - 对sd卡进行分区并写文件系统（用到了fdisk）
 - 下载u-boot源码，修改配置文件，下载交叉编译器，对其进行交叉编译
 - 将生成的u-boot镜像拷贝到sd卡中
 - 安装超级终端kermit，修改配置文件（如通信串口，速率等信息）
 - 下载内核以及所需要的工具链，采用对应的配置文件编译，编译好后将镜像拷贝至sd卡中
 - 编写引导文件使内核自动引导 

学会了哪些东西？

- 知道了u-boot的基本工作流程：

	1. 首先，固化在ROM上的code初始化相应的外设，确定启动模式，读取下一阶段启动文件，即 SPL 文件（下文中的MLO），SPL（Second Program Loader) 由 ROM Code 加载到片上 SRAM 中。
	2. 其次，SPL负责完成最小系统，包括 ARM core，时钟系统，DDR，外设/存储设备等的初始化，在 DDR 中加载并运行 U-Boot 文件
	3. 然后，U-Boot 会在 SPL 系统配置的基础上，进一步配置包括网口，USB，Nand Flash 等外设或存储设备，之后会有3s的等待时间，若没有用户打断行为，则执行bootcmd脚本（注：定义在U-boot/include/configs/ti_omap4_common.h)，根据脚本依次查询boot.scr, uEnv.txt等文件（注：详细的自己看脚本去，可在文件中写code来load kernel），存在的话则执行文件中内容，否则跳到uboot shell界面。

- 了解mmc的结构


- 了解elf封装格式


[1]: http://nikefd.elastos.org/2015/01/19/9/