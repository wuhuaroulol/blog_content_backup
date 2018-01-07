---
title: CentOS 7 安装python
date: 2016-09-16 19:37:50
tags: [centos,python]
categories: develop
---
笔者所用linxu发行版本为CentOS 7，所以系统自带的开发环境里的python版本为2.7.5，因为我需要python3，所以没办法，只好自己安装。
<!-- more -->
如果是开发老手或者专业人员，我觉得，想怎么弄就怎么弄。可惜我是个智障菜鸟，所以只能用比较简单的方法来安装。

有这样一个软件，叫pyenv，它是可以用来管理各种版本的python在你系统里面共存的，安装过它以后，你想使用哪个版本的python，只需要切换一下即可。关于它的介绍我引用一位前辈的经验：

### 安装 pyenv
在终端执行如下命令以安装 pyenv 及其插件：
<code>$ curl -L https://raw.githubusercontent.com/yyuu/pyenv-installer/master/bin/pyenv-installer | bash
</code>
安装完成后，根据提示将如下语句加入到 ~/.bashrc 中:
<code>export PYENV_ROOT="$HOME/.pyenv"
export PATH="$PYENV_ROOT/bin:$PATH"
eval "$(pyenv init -)"
eval "$(pyenv virtualenv-init -)" # 这句可以不加
</code>
然后重启终端即可。
### 安装 Python
#### 查看可安装的版本
<code>$ pyenv install –list</code>
该命令会列出可以用 pyenv 安装的 Python 版本，仅列举几个:

2.x.x # Python 2 版本
3.x.x # Python 3 版本
anaconda-4.0.0 # 支持 Python 2.6 和 2.7
anaconda3-4.0.0 # 支持 Python 3.3 和 3.4

其中形如 x.x.x 这样的只有版本号的为 Python 官方版本，其他的形如 xxxxx-x.x.x 这种既有名称又有版本后的属于 “衍生版” 或发行版。

#### 安装 Python 的依赖包
在安装 Python 时需要首先安装其依赖的其他软件包，已知的一些需要预先安装的库如下。 在 CentOS/RHEL/Fedora 下:
<code>sudo yum install readline readline-devel readline-static
sudo yum install openssl openssl-devel openssl-static
sudo yum install sqlite-devel
sudo yum install bzip2-devel bzip2-libs
</code>
在 Ubuntu下：
<code>sudo apt-get update
sudo apt-get install make build-essential libssl-dev zlib1g-dev
sudo apt-get install libbz2-dev libreadline-dev libsqlite3-dev wget curl
sudo apt-get install llvm libncurses5-dev libncursesw5-dev
</code>
#### 安装指定版本
使用如下命令即可安装 python 3.x.x：
<code>$ pyenv install 3.x.x</code>
该命令会从 github 上下载 python 的源代码，并解压到 /tmp 目录下，然后在 /tmp 中执行编译工作。

若依赖包没有安装，则会出现编译错误，需要在安装依赖包后重新执行该命令。

如果网络不太好，用 pyenv 下载会比较慢，可以先执行该命令，然后到 ~/.pyenv/cache 目录下查看要下载的文件的文件名，然后自己到官方网站下载，并将文件放在 ~/.pyenv/cache 目录下。

pyenv 会检查文件的完整性，若确认无误，则不会再重新下载。

对于科研环境，更推荐安装专为科学计算准备的 Anaconda 发行版：

<code>pyenv install anaconda-x.x.x</code>安装 Python 2.x 版本
<code>pyenv install anaconda3-x.x.x</code>安装 Python 3.x 版本；

#### 更新数据库
安装完成之后需要对数据库进行更新：
<code>$ pyenv rehash</code>
查看当前已安装的 python 版本
<code>$ pyenv versions
*system (set by /home/seisman/.pyenv/version)
3.x.x
</code>
其中的星号表示当前正在使用的是系统自带的 python。

#### 设置全局的 python 版本
<code>$ pyenv global 3.x.x</code>
当前全局的 python 版本已经变成了 3.x.x。也可以使用 pyenv local 或 pyenv shell 临时改变 python 版本。

注：

1.python各大版本之间可能是并不互相兼容的，所以如果有需求，请按照需求安装多个版本。

2.本文转载已通过联系授权，引用段来自https://seisman.info/python-pyenv.html