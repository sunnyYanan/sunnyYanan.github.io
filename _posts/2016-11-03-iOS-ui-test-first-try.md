---
layout: post
title:  iOS ui testing初探
categories: iOS测试
tag: ui testing
---

* content
{:toc}

背景
---------------
&emsp;&emsp;xcode7发布后，引入了ui testing这一个新的ui测试框架，原有的ui automation慢慢地也不再维护了。在xcode 8中官方直接抛弃了ui automation（启动instrument后会发现，这个入口被完全拿掉了）。这就使得我们原有的基于ui automation搭建起来的ui自动化持续集成方式在iOS10上变得不可用，本地xcode如果升级xcode8后，脚本也不能维护编写了。
由于现在iOS10系统的占有量已经超过了50%，因此需要将原有的UI自动化思路迁移到UI testing上面。

入门
----------
- 新建uitesting
	- 如果已有一个工程，只需要新建一个工程对应的UI target,路径🈶️2种
	1. 切换到test模块，点击左下角的＋，New UI Test Target
	2. File － New － Target － Test － iOS UI Testing Bundle
	- 如果是新建工程，则可以自动生成一个uitesting的target，这是一个类似于单测的存在

- 脚本录制
	- 新建target之后，会默认带有一个.m或者.swift文件，取决于建立target时，选择的是oc 或swift，不过凭借苹果的这个尿性，还是建议用swift写。
	- 点击该.m／.swift文件，在文件下方有一个红色的按钮，点击该按钮即可进行脚本的录制

- 脚本运行
	- 录制或者编写代码后，可commond+u运行测试代码。在test模块可查看运行结果


遇到的问题
----------
1、录制完成后直接运行脚本，提示：

```
No architectures to compile for (ONLY_ACTIVE_ARCH=YES, active arch=x86_64, VALID_ARCHS=i386)
```

&emsp;解决方法：在build setting中的Valid Architectures加入arm64即可