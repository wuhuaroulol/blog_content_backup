---
title: Linux零碎经验整理
date: 2017-06-16 20:56:45
tags: [linux,centos,software]
categories: develop
---
## 无线网络相关
在使用Linux的过程中，有过一段神奇的经历。

那是我刚换x550本子的时候，我发现无线网络偶尔会抽风。

具体表现为连接了一会儿之后莫名其妙的断开连接，但是桌面指示却还是显示为已连接状态。

一开始我以为是Network Manager和Network的冲突所导致。为此我查阅了Aarchlinux的wiki去简略学习了一下这个东西，链接是NetworkManager。

但折腾了半天之后发现并不是这个问题，于是我换了个思路。
<!-- more -->
首先我打印了我的网卡信息和驱动信息，看了看感觉并没什么问题
。
那难道是等待回复的时间过短导致认为我已经超时？我改了配置之后发现也不是这么回事。

无奈我去社区提问，发现和我有相似经历的人是有很多的。

最后一哥们回复说，他是这么解决这个问题的。

<code>sudo modprobe -r rtl8723be</code>

然后在/etc/modeprobe.d/这个路径下新建一个配置文件rtl8723be.conf

<code>sudo touch /etc/modprobe.d/rtl8723be.conf</code>

用文本编辑器打开它，添加

<code>options rtl8723be ips=0 fwlps=0 swenc=1</code>

我参考了他的方法发现真的就解决了这个问题。

这里再放上一个ubuntu论坛的rtl8723be无线网卡问题的帖子作为参考资料：[ubuntu14.04 amd64 无线网卡rtl8723be安装与问题资料整理][1]

## 播放器相关
本人用的是centos，桌面是集成的gnome，所以默认已经装上了一个播放器叫Videos(没错它就叫这个)。

因为最近需要看一些教程视频，所以得弄下播放器，我一看环境里已经自带了一个，就寻思直接用吧。

体验结果是，无fuck说。

这玩意简直不是体验差那么简单了，默认什么格式都不支持，需要现搜索解码。

这也就算了，大哥，搜索之后各种安装不了是什么意思。

于是我选择了我比较喜欢的vlc。

但是问题来了，这货(Videos)无论我怎么设置，默认打开方式还是它。

诶哟脾气还挺大啊。

我寻思卸载了吧，结果我猜了半天的名字也没猜对这货真实包名。

我就很气。

百度了半天也没找到个所以然(垃圾百度)，换谷歌，在ubuntu论坛又找到了答案(ubuntu桌面问题真的挺全的，毕竟桌面大户)。

<code>sudo apt-get purge totem totem-plugins</code>

我去，你叫totem啊……

赶紧卸载了，真的是闹不住。

参考资料：[Videos，How do I remove the Gnome’s video applications?][2]


  [1]: http://forum.ubuntu.org.cn/viewtopic.php?t=462588
  [2]: https://askubuntu.com/questions/768487/how-do-i-remove-the-gnomes-video-applications