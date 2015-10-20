---
layout: post
title:  "ios自动化测试入门之脚本编写"
date:   2015-10-19 20:27:05
categories: iOS-UI-automation
excerpt: iOS UI automation入门
---

* content
{:toc}

##写在前面

&ensp; 在上一篇中，我们介绍了ios ui automation的脚本录制回放功能，可以看到该方法的使用非常简单，它可以帮助我们了解instrument中脚本运行的原理以及基本的脚本语法，但是在实际使用过程中，我们无法仅仅利用录制回放功能来简单录制我们需要执行的过程然后直接运行脚本从而实现自动化，这是因为：

- **元素定位**：在使用脚本录制功能时，对某元素的访问会直接使用名称，如

```
target.frontMostApp().mainWindow().scrollViews()[0].collectionViews()[0].cells()["         [买2个送1个][凤全] 加粗实心铁条 创意心型多用途挂钩 1个顶7个可承重50斤以上 高温烤漆耐用10年以上"].tap();
```

&ensp;但是，这种与元素特有属性－名字相结合的写法无法应对应用的内容变动，当app中元素变化时，脚本将因为无法访问到元素而执行错误。

- **意外警告及弹窗**：在应用的实际使用过程中，常常会弹出各种各种的信息获取或者消息提示弹窗，而这些弹窗在脚本录制过程中，是无法覆盖完全的。而且可能发生的一种情况就是，录制过程中发生了弹窗，但是脚本再执行时不一定会遇到的弹窗，如只有首次安装才会遇到的位置获取弹窗等。

- **兼容性问题**：在不同的操作系统中，app的实现方式会有细微的差别，元素的层级结构和整个窗口的结构安排也会不同，这些都需要脚本的微调来适应兼容性问题。这些问题仅仅通过脚本录制回放功能是无法解决的。

&ensp;因此，实际使用过程中，脚本需要手工编写。下面我们介绍自动化脚本的编写以及执行过程。

---

##编写脚本

###编辑工具

&ensp;我们可以直接在instrument automation中的编辑页面进行自动化脚本的编辑，但是这种情况下代码格式需要手工控制，十分不友好。因此，我们可以选用sublime text3来编写js脚本，这也是前端人员使用最多的js编辑工具。

###模拟器 or 真机
&ensp;在iOS UI automation自动化测试入门之录制回放功能中，我们已经介绍了automation基本的使用流程，模拟器或者真机的区别在于target中机型的选择，当电脑连接真机后，target中便可以选择该设备，**需要注意**的一点是，在ios8中，如果想要执行自动化脚本，想要打开真机设备的ui automation的开关，其设置路径为：

设置－开发者－enable ui automation。

脚本运行中如果发生莫名其妙的错误，一定要首先检查这项开关的设置是否已经打开，不然会浪费很多功夫。

###元素访问
&ensp;每一个可以访问的UIKit控件都可以用一个javascript对象来描述，也就是一个UIAElement。我们可以使用以下两种方式来访问元素。

- **logElementTree()**

&ensp;使用target.logElementTree()的方法可以得到当前页面下所有元素层级。

- **Accessibility inspector**

&ensp;在模拟器中，可以通过路径：设置－general-accessibility打开accessibility inspector来查看当前页面中元素的属性，如元素的label，name，rect。

&ensp;查询到元素之后，就可以通过元素的名称、序号等属性来访问对应的元素。

###手势动作

&ensp;通过对页面中元素执行用户动作，就可以模拟真实的用户操作。一些操作的方法如下：

- **点击**

> 对具体的元素执行点击：

UIATarget.localTarget().frontMostApp().mainWindow().buttons()\[1\].tap();

> 对窗口某位置进行点击：

UIATarget.localTarget().frontMostApp().mainWindow().tap({x:100,y:30});

>双击方法：

doubleTap();

- **缩放**

> UIATarget.localTarget().pinchOpenFromToForDuration({x:20, y:200},{x:300, y:200},2); 

> UIATarget.localTarget().pinchCloseFromToForDuration({x:20, y:200}, {x:300, y:200},2);

- **拖拽或滑动** 

> UIATarget.localTarget().dragFromToForDuration({x:160, y:200},{x:160,y:400},1); 

> UIATarget.localTarget().flickFromTo({x:160, y:200},{x:160, y:400});

###高级交互

- 处理预期或者非预期的提示框（alert）

UIATarget.onAlert = function onAlert(alert){ 

    var title = alert.name(); 

    UIALogger.logWarning("Alert with title ’" + title + "’ encountered!"); 

    return false; // use default handler 

}


该方法返回false，会帮助我们自动销毁alertview的窗口。


UIATarget.onAlert = function onAlert(alert) { 

    var title = alert.name(); 

    UIALogger.logWarning("Alert with title ’" + title + "’ encountered!"); 

    if (title == "Add Something") { 

        alert.buttons()["Add"].tap(); 

        return true; // bypass default handler 
    } 
    return false; // use default handler 

}


&ensp;返回true会帮助我们处理一些我们预期会发生的alert，如获取用户位置信息。

- 多任务

&ensp;我们可能需要测试某些后台程序，或者将程序放入后台执行来观察一些数据信息，通过以下方法就可以模拟用户点击home操作，将程序放入后台执行，并在10秒之后重新打开程序。

>UIATarget.localTarget().deactivateAppForDuration(10);

- 屏幕方向

&ensp;通过setDeviceOrientation()方法可以设置设备的方向，自动化脚本运行中一般不会用到。

###脚本运行

&ensp;当编辑完脚本之后，我们可以导入我们的脚本文件，然后点击target下方的红色按钮（注意此时是target下的那个红色按钮）或者win+r的方式来运行我们的脚本。

###题外话

- 当不知道如何编写脚本时，不妨使用录制回放功能来进行查看，它可以作为很好的参考。
- 执行自动化脚本时，使用target.delay(3)方法可以帮助我们观测执行情况，并处理很多网络延迟，页面加载等时间问题。




