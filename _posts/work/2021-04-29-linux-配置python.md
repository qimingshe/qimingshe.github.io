在 Ubuntu 中配置 Python 环境

https://www.jianshu.com/p/0c6eea11ef28



安装 Python2，打开 Terminal 输入
$ sudo apt install python2
检测安装版本
$ python2 -V
[站外图片上传中...(image-7ecbd2-1584549029115)]

{{< imgcap src="" title="" >}}

切换不同的 Python 版本
检查系统已安装的 Python 版本，打开 Terminal 输入
$ ls /usr/bin/python*
/usr/bin/python     /usr/bin/python3.6          /usr/bin/python3.8
/usr/bin/python2    /usr/bin/python3.6-config   /usr/bin/python3.8-config
/usr/bin/python2.7  /usr/bin/python3.6m         /usr/bin/python3-config
/usr/bin/python3    /usr/bin/python3.6m-config  /usr/bin/python-mkdebian
因为我是由 18.04 升级到 20.04，所以还有 Python 3.6 版本

检测是否已存在 Python 的配置方案
$ sudo update-alternatives --list python
update-alternatives: error: no alternatives for python
此时，显示没有配置方案

为 Python2 和 Python3 分别配置
$ sudo update-alternatives --install /usr/bin/python python /usr/bin/python2 1
$ sudo update-alternatives --install /usr/bin/python python /usr/bin/python3 2
确认是否配置成功
$ sudo update-alternatives --list python
/usr/bin/python2
/usr/bin/python3
更改默认 Python 版本
$ sudo update-alternatives --config python
有 2 个候选项可用于替换 python (提供 /usr/bin/python)。

  选择       路径            优先级  状态
------------------------------------------------------------
  0            /usr/bin/python3   2         自动模式
* 1            /usr/bin/python2   1         手动模式
  2            /usr/bin/python3   2         手动模式

要维持当前值[*]请按<回车键>，或者键入选择的编号：1
如果想默认 Python 2 选 1，想默认 Python 3 选2

检查当前 Python 默认版本
$ python -V
安装 Python 包管理器 PIP
pip is the package installer for Python. You can use pip to install packages from the Python Package Index and other indexes.

分别为 Python 2 和 Python 3 安装

% Python 2:
$ sudo apt install python-pip
% Python 3:
$ sudo apt install python3-pip
检测是否安装成功

$ pip --version
$ pip3 --version


Unix & Linux 平台安装 Python3:
以下为在 Unix & Linux 平台上安装 Python 的简单步骤：

打开WEB浏览器访问 https://www.python.org/downloads/source/
选择适用于 Unix/Linux 的源码压缩包。
下载及解压压缩包 Python-3.x.x.tgz，3.x.x 为你下载的对应版本号。
如果你需要自定义一些选项修改 Modules/Setup
以 Python3.6.1 版本为例：

# tar -zxvf Python-3.6.1.tgz
# cd Python-3.6.1
# ./configure
# make && make install


