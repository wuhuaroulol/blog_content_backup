---
title: CentOS 7 配置 dnf 及其插件
date: 2017-10-11 20:25:43
tags: [linux,centos,dnf,dnf-plugins-core]
categories: linux
---
**注意：本文所述dnf不是某网络游戏(严肃)。**

笔者最近安装了一些神奇的东西，于是发现有时候dnf管理要好于yum一些，所以就想给centos7弄上。

但是奇怪的是在中文搜索结果中，清一色的都是添加仓库，然后直接yum install dnf即可，我很好奇他们在1611之后的版本是如何按照这个步骤添加的dnf，因为仓库里已经没有这个包了。
<!-- more -->
于是还是谷歌一番，不出所料，果然有跟我一样纳闷的哥们。

> It is so weird, I have a centos7 VM box. Try to install dnf because it
> is a dependency of another package.
> 
> Most instructions on internet is like this:
> 
> <code>sudo yum install epel-release </code>
> <code>sudo yum install dnf </code>
> But nothing installs.
> 
> <code>No package dnf available Reedcentos-7-4-can-not-install-dnffrom-epel</code>

from: [Reed centos-7-4-can-not-install-dnffrom-epel][1]

我一看这帖，跟我情况一样，就是没有。

我看了看答案，还是这个安装脚本解决了我的问题：

> This will install dnf-0.6.4 on CentOS/7.
> 
> <code>curl -OL https://raw.githubusercontent.com/tamama/repository/master/sh/tamama/node-provisioning/tamama-centos/7.3/setup.sh</code>
> 
> <code>sudo sh setup.sh</code>
> 

from: [tamama centos-7-4-can-not-install-dnffrom-epel][2]

OK，dnf的问题解决了，但是要想使用dnf copr这个命令，还是需要再安装dnf-plugins-core才行。

继续谷歌。

<code>su root</code>

然后用root账户执行：

<code>wget -P /etc/yum.repos.d/ https://copr.fedoraproject.org/coprs/jkastner/dnf-plugins-core/repo/epel-7/jkastner-dnf-plugins-core-epel-7.repo && yum install dnf-plugins-core</code>

到此，搞定。

点[这里][3]去往DNF文档。


  [1]: https://serverfault.com/questions/874471/centos-7-4-can-not-install-dnffrom-epel
  [2]: https://serverfault.com/questions/874471/centos-7-4-can-not-install-dnffrom-epel
  [3]: http://dnf.readthedocs.io/en/latest/index.html