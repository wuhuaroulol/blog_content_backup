---
title: CentOS 7(1708) Intel+Nvidia双显卡笔记本安装Nvidia驱动并用Bumblebee控制独显
date: 2018-01-19 06:34:34
tags: [linux,centos,hardware,driver]
categories: linux
---
标题名字有点长，因为我并不想把分两篇文章来讲这个事。

想写这样一篇博文，主要是因为中文环境中关于这个问题的资料实在太少，而且很多的文章有过多的坑，让我的几个朋友苦不堪言，于是应邀在我的博客挂一篇指南。

关于笔者环境之前文章已有说明，不再赘述。

那么我们就直奔主题了。
<!-- more -->
<strong>警告：确定继续阅读前，请保证自己有良好的英文阅读能力及linux使用经验(本篇为rh系指南，但也可作为其它系参考)，并且拥有良好的耐心和学习态度，比较整块的时间进行实践。</strong>

## 关于驱动的一些概要说明
首先，在linux环境下，显卡的驱动是有几种选择的。

但就如之前文章所述，如果是普通用户，那么就不要自己碰这方面了，安安稳稳的使用发行版集成的开源驱动即可。

如果确定需要自行安装nvidia独立显卡驱动，那么我也并不推荐去nvidia官方进行下载安装，因为这种安装方式会有一个问题，就是维护比较麻烦。举个例子就是可能你系统升级一次之后，你就需要重新安装，非常麻烦。

对于centos，我推荐的方式是通过ELrepo源进行安装。

## Nvidia驱动的安装

### 添加ELRepo源
这个步骤非常简单，复制下面的链接，粘贴到浏览器地址栏打开：

<code>http://elrepo.org/tiki/tiki-index.php</code>

然后按照页面中所写的步骤进行操作即可。

### 安装nvidia-detect
这是一个用来检测并确认自己设备显卡的信息及其所适合的驱动版本的程序。

打开终端，执行：

<code>sudo yum install nvidia-detect</code>

即可。

### 检测型号
打开终端，执行：

<code>nvidia-detect -v</code>

即可。

### 更新内核及内核开发文件
打开终端，执行：

<code>sudo yum update kernel kernel-devel</code>

即可。

### 确认当前是否为nouveau驱动
打开终端，执行：

<code>lsmod|grep nouveau</code>

如果有输出相关信息，那么就是了。

### 禁用nouveau驱动
我们想要顺利安装nvidia驱动，那么必须先禁用nouveau驱动，还需要把它踢进黑名单中。

用文本编辑器打开 /lib/modprobe.d/dist-blacklist.conf 这个文件，在其中加入nouveau：

<code>blacklist nouveau options nouveau modeset=0</code>

### 更改并重新生成grub2
用文本编辑器打开 /etc/default/grub 文件，在其中的：

<code>GRUB_CMDLINE_LINUX="rd.lvm.lv=vg_centos/lv_root rd.lvm.lv=vg_centos/lv_swap rhgb quiet"</code>

quiet后面加入rdblacklist=nouveau ，保存。

打开终端，执行：

<code>sudo grub2-mkconfig -o /boot/grub2/grub.cfg</code>

重新生成即可。

### 重建initramfs image
我们首先把现有的移动到其它路径下以作为留手备份：

打开终端执行：

<code>sudo mv /boot/initramfs-$(uname -r).img /你喜欢的路径</code>

然后重建它，执行：

<code>sudo dracut /boot/initramfs-$(uname -r).img $(uname -r)</code>

即可。

### 设置启动方式为文本模式
因为我们需要执行显卡驱动的安装，所以不能进入图形界面。

打开终端，执行：

<code>systemctl set-default multi-user.target</code>

然后reboot就行了。

### 查看nouveau是否被禁用
重启后会直接进入文本模式，需要用用户名和密码登陆一下终端。

然后执行：

<code>lsmod|grep nouveau</code>

如果没有输出，那么就证明已经禁用了。

### 安装nvidia驱动
如果是比较新的显卡，比如9xx，那么直接执行：

<code>sudo yum install kmod-nvidia</code>

即可。

如果不是比较新的，那么需要按照nnvidia-detect -v 命令所输出的版本进行安装。

到此，nvidia显卡驱动安装结束。

## Bumblebee的安装及配置
<strong>首先，你应该知晓这是一个什么东西，做什么用的，为什么选用它。</strong>

你首先需要去archlinux的wiki中了解以下bumblebee的相关知识，下面是链接：

<code>https://wiki.archlinux.org/index.php/bumblebee</code>

### Bumblebee的安装
终端中执行：

<code>sudo yum install bumblebee</code>

即可。

### 添加用户到Bumblebee的组
这一步可能并不是必要的，但如果安装后/重启后发现并没有自动把用户加入Bumblebee组中，那么需要执行：

<code>usermod -a uname -G bumblebee</code>

这样就添加完成了。

### 配置Bumblebee 及 将控制面板加入用户菜单
<strong>不要盲目复制任何网络上的配置文件，请认真阅读注释及根据自身情况进行配置文档书写。</strong>

配置文档的配置，你可以根据：<code>http://elrepo.org/tiki/bumblebee</code> 此处的配置进行参考配置。

### 切回图形模式默认启动
终端执行：

<code>systemctl set-default graphical.target</code>

重启即可。

至此，所有步骤完成，已可正常使用。

<strong>注：对于所有命令中不理解的地方请自行查阅资料，本文是以长期使用linux作为工作环境的使用者角度书写，很多基础知识不予赘述。</strong>
