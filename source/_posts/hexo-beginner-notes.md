---
title: 如何使用Hexo建立个人博客(新手参考笔记)
date: 2017-06-16 20:56:45
tags: [hexo,blog]
categories: program
---
有朋友问我如何用hexo建立一个和我这个一样的个人小博客，其实很简单。

首先，你需要在你的本地主机上安装hexo。

这个其实用不着再百度查找各类所谓的教程，因为那些只是他人机器的过程和问题记录，只具有一些参考价值而已。

你只需要打开hexo的官方文档，按照文档的指示一步一步老老实实的做下去即可，非常简单。

但朋友还是懒得看，好吧，我把最简洁的步骤放在这里。
<!-- more -->
## hexo的安装
第一步，你想要使用hexo的话，你需要已经安装好node.js和git这两个程序，如果没有，那么按照下面的步骤走一遍。

### 安装node.js：
linux平台：
<code>wget -q0- https://raw.github.com/creationix/nvm/master/install.sh | sh
nvm install stable
</code>
win平台：

请点击[此处][1]获取下载链接

这样就安装好了node.js。

### 下面安装git：
rh系：
<code>yum install git</code>
debian系:
<code>apt-get install git</code>
win平台：
请点击[此处][2]获取下载链接

下载之后可以进行一些基础设置，比如设定用户名和邮箱，命令为：

<code>git config –global user.name “用户名”</code>
<code>git config –global user.email “邮箱”</code>

当然，设定用户名这一步可以不做。

如此，我们就好了准备工作。可以进行安装hexo了。

### 安装hexo：
linux下，直接敲入：
<code>npm install -g hexo-cli</code>
即可。

win平台比较好的安装方法是点击开始按钮，输入”git bash”查找到git bash，运行它，敲入这行命令即可。

## hexo的使用
### hexo的使用-init
现在我们就可以用hexo来建立我们的博客了。

执行
<code>hexo init 你想放博客的文件夹名字(本来是没有这个文件路径的，是由hexo新建)</code>
这样就完成了，就是这样的简单。

然后你就需要切换到博客文件路径下进行各种操作了。

首先打开_config.yml这个文件，在这个文件中，你首先要更改一下博客的基本信息，让它真正变成你的记事本。
<code># Site
title: 你的博客标题
subtitle: 你的博客副标题
description: 关键字
author: 作者名字
language: 语言区域
timezone: 时间区域
\# URL
\## If your site is put in a subdirectory, set url as 'http://yoursite.com/child' and root as '/child/'
url: 你的域名
root: 你的博客根路径
permalink: 固定链接的自定义显示要素
</code>
改好这些信息之后就基本可以了。

### hexo的使用-文章的书写和发布
那么我们现在就来使用hexo来写一篇文章，并且把我们的小博客在本地运行起来看看效果。

请先处于你的博客根目录，然后执行
<code>hexo new 你的文章名</code>
这样，我们就创建好了文章的md(markdown)文件了。

这个文件位于source/_post路径下，你可以用你喜欢的markdown编辑器来填写这个文件的内容。

写好之后保存。

那么现在我们需要把它生成静态文件。

切回根目录，执行
<code>hexo g</code>
就可以了。这个命令是hexo generate的简化。

然后我们本地运行以下看看效果。

根目录，执行
<code>hexo s --debug</code>
打开你的浏览器，在地址栏键入localhost:4000即可访问了。这里加上debug参数是为了本地调试。

### hexo的使用-github
我知道很多人都喜欢在github分享自己的笔记，那么这里就介绍如何把本地的项目和github”连接”在一起。

当然，你需要拥有一个github帐号，并且新建一个github page的项目。

注意，这个项目的命名是有严格的规范的，必须是 你的用户名.github.io 才行。

创建好这个项目之后，我们就可以进行”连接”了。

首先，你需要切到博客根目录，安装hexo-deployer-git。

执行
<code>npm install hexo-deployer-git --save</code>
这样，就装好了。

之后，打开_config.yml文件。
<code># Deployment
\## Docs: https://hexo.io/docs/deployment.html
deploy:
type: git
repo: https://github.com/你的用户名/你的用户名.github.io.git
branch: master
</code>
按照如上配置填写好自己的github配置(这里用https而不是ssh是主要考虑普通用户的操作难度，用https会有一个输入用户名和密码的过程，比较简单直观。当然了，用ssh很省事，但如果你已经会用ssh协议了，那我觉得也用不着这篇博文了，是吧 :) )。

配置好了之后，只需要在根目录执行
<code>hexo deploy</code>
就可以了。

### hexo的使用-更新的简单步骤
1.新建
<code>hexo new 文件名</code>
2.编辑刚才新建好的博文md文件
3.生成静态文件
<code>hexo g</code>
4.本地测试(括号内为可选参数)
<code>hexo s (--debug)</code>
5.同步github
<code>hexo deploy</code>
### hexo的使用-主题
更改主题很简单，下载好主题放入theme路径下。

然后打开_config.yml文件，找到
<code># Extensions
\## Plugins: https://hexo.io/plugins/
\## Themes: https://hexo.io/themes/
theme: 主题名字
</code>
修改好之后就可以了，plugins也是同理，文档注释也写的很清楚了。

### hexo的使用-个人域名
如果你的域名并不想使用 你的用户名.github.io ，那么需要你在source路径下新建一个CNAME文件。

注意，没有后缀，这个文件就叫CNAME。

然后用文本编辑器打开，填写你的个人域名，保存。

之后还需要设置好自己的域名解析，即可使用自己的域名作为博客域名了。

### 最后

如果对本文有任何问题请联系我。


  [1]: https://nodejs.org/dist/v8.9.4/node-v8.9.4-x64.msi
  [2]: https://git-scm.com/download/win