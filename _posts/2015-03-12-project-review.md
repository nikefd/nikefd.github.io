---
layout: post
title: 项目经验回顾总结
category: technology
---

###项目一（u-boot编译下板及内核加载）

pandaboard下板你都做了些什么，学会了什么东西？

参考答案：

主要参考我之前写过的两篇文章[文章一][1]、[文章二][2]

都做了些什么：

 - 对sd卡进行分区并写文件系统（用到了fdisk）
 - 下载u-boot源码，修改配置文件，下载交叉编译器，对其进行交叉编译
 - 将生成的u-boot镜像拷贝到sd卡中
 - 安装超级终端kermit，修改配置文件（如通信串口，速率等信息）
 - 下载内核以及所需要的工具链，采用对应的配置文件编译，编译好后将镜像拷贝至sd卡中
 - 编写引导文件使内核自动引导（将内核加载到内存）

学会了哪些东西？

- 知道了u-boot的基本工作流程：

	1. 首先，固化在ROM上的code初始化相应的外设，确定启动模式，读取下一阶段启动文件，即 SPL 文件（下文中的MLO，即ROM loads x-load），SPL（Second Program Loader) 由 ROM Code 加载到片上 SRAM 中。
	2. 其次，SPL负责完成最小系统，包括 ARM core，时钟系统，DDR，外设/存储设备等的初始化，在 DDR 中加载并运行 U-Boot 文件
	3. 然后，U-Boot 会在 SPL 系统配置的基础上，进一步配置包括网口，USB，Nand Flash 等外设或存储设备，之后会有3s的等待时间，若没有用户打断行为，则执行bootcmd脚本（注：定义在U-boot/include/configs/ti_omap4_common.h)，根据脚本依次查询boot.scr, uEnv.txt等文件（注：详细的自己看脚本去，可在文件中写code来load kernel），存在的话则执行文件中内容，否则跳到uboot shell界面。

- 了解elf格式

	ELF(Executable and Linkable Format)即可执行连接文件格式。头文件可以重定位地址。
	知道将内核加载到内存的哪个地址也是也是可以自己指定的，只要不与被占用的地址冲突。

- 用gdb进行远程调试（对arm开发板上的程序进行调试）

	首先采用交叉编译器进行编译
		
		$ qemu-system-arm -M kzm -S -s -nographic -kernel images/sel4test-driver-image-arm-imx31

	然后运行用来调试arm结构的gdb

		$ ./arm-none-eabi-gdb

	读取内核镜像

		(gdb) file ~/HD-Elastos/goose/images/sel4test-driver-image-arm-imx31

	连接目标开发板ip及端口

		(gdb) target remote :1234

- 知道模拟器（simulation)与仿真器(emulation)与虚拟机区别
	
	simulation是模拟出原系统的一个抽象模型，而不需要真的去做真实系统要做的事情。 emulation则更进一步，要真正地去做所有真实系统能做的事情，只不过做的“过程”不同，通常是为了模拟不同指令集、不同体系架构的 CPU，所以多数情况要对微指令进行解释执行。
	
	虚拟机，或者说虚拟化技术 virtualization，基本都是去模拟一套相同指令集相同架构的硬件平台，因此在做好保护的前提下，很多时候可以直接利用 CPU 去执行目标指令。虽然还是模拟物理 CPU 而不借助于 Host OS 的功能，毕竟少了一层指令集转换，运行速度会提高不少。

- 知道编译的时候可以自己写链接脚本指定代码段的地址以及数据段的地址

 用到了哪些工具：fdisk工具（分区、格式化等），编译所需的工具链

 解决了什么问题：编写操作文档，帮助别人更快的下板

###项目二（DreamWhere网站开发）

- 完成的功能

   	1. 注册登录  
   	- 设定旅游目标  
   	- 书写奋斗历程  
   	- 查看成功经历  
   	- 分享成功经历  
   	- 查看别人分享的成功经历  

- 解决了什么问题：
 
	针对年轻人空有说走就走的旅行想法却很少实施的现状，开发一个能够激发年轻人对旅游的向往并为之实践奋斗的网站显得尤为必要。
 
- 主要用到了哪些方法：

 	人机交互的一些方法：

	1. 编写用户引导  
	2. 尽量以图片的形式来展示  
 	3. 采用尽量少的层级结构，可以快速的到达任何一个界面  
 	4. 费次定律：屏幕边缘具有无限大的尺寸，按钮放边上；较小目标需要设置外边界，；有时，界面元素越小越好，比如删除按钮。  
 	5. 用撤销取代对用户的干扰  
 	6. 可发现性  
 	7. 超链接变颜色  
	8. 提供搜索功能  
 
 > 费茨法则：是一个人机互动以及人体工程学中人类活动的模型，它预测了快速移动到目标区域所需的时间是目标区域的距离和目标区域的大小的函数。
 
 - 用到了什么工具：
 	
	1. 界面设计Balsamiq  [bɔl'sæmik] Mockups，是一种软件工程中快速原型的建立软件，可以做为与用户交互的一个界面草图，一旦客户认可可以做为美工开发HTML的原型使用。 
	2. 数据库mysql，服务器apache，后台实现php，前台bootstrap框架，jquery库。
 
- 学到了什么东西？
	
	1. 小网站从前台到后台的开发建立过程及基本结构
	2. 增加动手实践能力，编程能力

###项目三（单周期mips cpu开发）

![Alt data-path][data-path]

 - 都做了什么

 - 学到了什么东西
 
 - 用到了哪些工具

###项目四（上创项目）

 - 领域：移动应用开发

 - 解决什么问题：1）自我学习规划  2）实时便捷记录  3）在线讨论
 
 - 主要有哪些方法：
 
 - 用到了什么工具：
  
	 Java Development Kit（JDK）、Eclipse, Android SDK(软件开发工具包:Software Development Kit）、Android Development Tools (ADT)

[1]: http://nikefd.elastos.org/2015/01/19/9/    "u-boot编译"
[2]: http://nikefd.elastos.org/2015/01/26/%E5%9C%A8u-boot%E5%9F%BA%E7%A1%80%E4%B8%8A%E5%9C%A8pandaboard%E4%B8%8A%E8%B7%91%E7%AE%80%E5%8D%95%E5%B0%8F%E7%A8%8B%E5%BA%8F%EF%BC%88experiment-2%EF%BC%89/  "u-boot上跑自己的小程序"
[data-path]: /image/data-path.png