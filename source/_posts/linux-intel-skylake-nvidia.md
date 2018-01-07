---
title: linux对intel skylake+nvidia笔记本的硬件支持个人笔记
date: 2017-02-28 21:30:32
tags: [centos,linux,skylake,intel,nvidia]
categories: linux
---
在过年回家之前，因为要利用假期写一点东西，所以买了个新本子。

因为考虑到我并不需求显示和媒体处理，所以就选择了高U低显的奇葩配置。

我的硬件状况大概是i7 6700hq和nvidia 940mx。

不过想法是好的，现实是蛋疼的，因为我发现一个问题，就是无论哪个linux发行版，都无法正常的进行图形化安装。
<!-- more -->
有卡死在安装界面的，比如debian系的ubuntu、debian等。

有卡死在安装过程中的，比如rh系的centos、fedora等。

还有压根就只能文本安装的，比如openSUSE等。

弄的我是血泪一片，蛋碎一地。

经过两周的狂怼，总算是解决了这个问题。

首先，我还是选择了我个人较为喜欢使用的centos 7系统(因为我是个linux新手，也基本是只用来写文字，所以这个发行版挺稳当的……)，进入trouble shooting选项之后自定义了启动参数，并且以基本显示模式进行启动，即可正常安装。

这个问题究竟是什么原因呢？怎么说，如果是其它设备可能会很简单，不过我个人的这个本子，那真是蛋疼的不行。

首先，我们需要解决的是目前的linux发行版(ubuntu 16.04、centos 7 1611、fedora 25等同时期版本)对intel的skylake cpu的支持问题。这个问题花费了我一整个周末的时间来进行查找相关资料，但遗憾的是，国内的大部分资料都是没用/无意义的。在各大linux发行版的官方论坛，我看到了很多关于此问题的发问帖，甚至早在2015年就有人提出了相关问题，但我很纳闷为何到了今天还是存在这个问题，而且没有一个明确的解决方法。底下的回复基本是各执一词，打印的日志也是五花八门，让我头疼不已。很多人说可以尝试更新内核，因为新的内核版本对新硬件支持更好，结果就是我从3.10一路尝试到了4.10，但是没有一个能够在正常模式里进入x的，我也是醉了。直到我在arch linux的wiki上检索到了这样一条信息：

<code>This can be fixed by disabling execlist support which was changed to default on with kernel 4.0. Add the following kernel parameter:i915.enable_execlists=0</code>

我当时看到这篇wiki的时候我感受到了光你们造吗……虽然它说的是在4.0版本内核以前会存在问题，但是……

我就加了这么一条，结果，it works！

这里我必须要说，arch linux的wiki真TM的是个宝。

解决了这个问题之后，虽然可以正常进入图形界面了，而且显卡识别也正确了，但是风扇狂转，发热巨大，基本上我开机了两分多钟就按电源了，再那么烧下去估计本子就废了。

所以我们接下来要先解决风扇和发热问题。

经过我的个人瞎JB诊断(新手+智障)，我得出，之所以会出现这种情况，是因为开源驱动nouveau的问题。

首先先来这三张页面看一下基础：nvidia-archwiki nouveau-archwiki nvidia optimus-archwiki

对的，没有错，我们现在需要一些东西来“控制显卡”。

首先我安装了这些东西： lm_sensors、tlp、tlp-rdw acpid、bumblebee

不过跳过tlp的脚本控制，一般你只需要安装bumblebee就可以。

如果你不怎么需要用独显，那么只需要在安装好bumblebee之后把自己的用户加入到bumblebee后reboot就可以了。

如果使用独显，那么请参考上方archwiki的三个链接及你的发行版的论坛支持和wiki进行安装并配置(这里不写是因为linux下的显卡驱动一直是个大问题，尤其是n卡，几乎就是一个设备一个样，基本上没什么参考价值)。

安装好了之后，ok，如果你和我一样是1080p的屏幕，那么你需要设置一下dpi适配，不然你会瞎。

如果想禁用触摸版，那么有两种方法。

第一个是装一个xorg-x11-apps，然后xinput –list，看看你触摸版是哪个，xinput set-int-prop num(列表里的数字编号) “Device Enabled” 8 0/1(关/开)即可。

第二个是开机去bios，直接关掉。