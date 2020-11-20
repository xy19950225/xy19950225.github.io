---
title: "Jupyter Notebook 配置"
date: 2020-11-13T16:30:05+08:00
lastmod: 2020-11-13T16:30:05+08:00
draft: false

tags: ["Anaconda", "Jupyter Notebook"]
categories: ["文档"]
hiddenFromHomePage: false

featuredImage: "/images/theme-documentation-content/featured-image.jpg"
featuredImagePreview: ""

toc: true
autoCollapseToc: true
math: true
lightgallery: true
linkToMarkdown: true
share:
  enable: true
comment: true

---

了解如何简单配置  Jupyter Notebook。

<!--more-->

## Jupyter 配置文件

配置文件，顾名思义就是可以修改Jupyter各种配置的文件。想要修改Jupyter那些默认的配置选项，就需要在配置文件`jupyter_notebook_config.py`中修改相应配置选项的属性。

这个配置文件一开始并不存在，需要手动生成。方式很简单，在命令行输入`jupyter notebook --generate-config`并执行，配置文件就创建好了，它的位置是在`C:\Users\Administrator\.jupyter\`中。

![img](https://i.loli.net/2020/11/19/TkgMvDp8f74CtBj.png)

然后我们去c盘主目录下打开.jupyter文件夹，就能找到配置文件：`jupyter_notebook_config.py`![img](https://filescdn.proginn.com/41b6e91d48628f78e66d900d12c3bc1d/1064e411a64ea204231a45f4e48e7543.webp)配置文件是关键，后面都要用到的。

## 1. 更改默认工作目录

一般情况下，Jupyter的默认工作目录为`C:\Users\Administrator\`，这样很不清爽，而且不便于管理项目，所以常需要在其他盘建立一个独立的Jupyter工作目录文件。![img](https://filescdn.proginn.com/71071936ed2c5394d8388054450aea2b/05a7d7d5f7ae7befa02125b7218b95d9.webp)

前面提到配置文件`jupyter_notebook_config.py`，工作目录就在这个里面修改。

1. 用记事本打开配置文件`jupyter_notebook_config.py`；
2. Crtl + F组合键找到`c.NotebookApp.notebook_dir`元素，删掉前面的注释`#`；
3. 在后面的单引号里输入要设置的目录路径（注意双斜杠），保存关闭；例如：`c.NotebookApp.notebook_dir = "E:\\jupyter_notebook"`
4. 修改快捷键，在win开始菜单中找到`jupyter notebook`快捷图标，右击选择属性，删除目标值最后的 “%USERPROFILE%/”，点击确定退出。

![img](https://filescdn.proginn.com/3bf28446e6b47b046f7e60320ba1e5f2/329594e45d9fa1d138e38dc20c2c779d.webp)

经过这四个步骤，工作目录就修改好了，这时候不管你是通过快捷键还是命令行进入Jupyter Notebook，都能看到最新设置的目录，干净清爽。

![img](https://i.loli.net/2020/11/19/9Mfo51rwYTvPSQL.jpg)

## 2.更改默认浏览器

很多小伙伴有自己的浏览器偏好，希望Jupyter运行在经常使用的那个浏览器上。

更改Jupyter默认浏览器也比较简单，以设置chrome浏览器为例：

1. 找到chrome.exe文件的安装路径，复制该路径。例如：`u'C:\\Program Files (x86)\\Google\\Chrome\\Application\\chrome.exe'`查找方式？右键chorme图标，打开文件所在位置，如下图：

![img](https://filescdn.proginn.com/91847da82c8b97c6c79078846222a620/b603afcd741e4ca896853aa0adc72f4c.webp)

2. 用记事本打开配置文件`jupyter_notebook_config.py`；
3. Crtl + F组合键找到`c.NotebookApp.browser`元素；
4.  在找到记录的下方添加以下代码（注意替换为你的chrome.exe路径）：

```
import webbrowser
webbrowser.register('chrome', None, webbrowser.GenericBrowser(u'C:\\Program Files (x86)\\Google\\Chrome\\Application\\chrome.exe'))
c.NotebookApp.browser = 'chrome'
```

保存文件。这样就大功告成了，重新启动Jupyter，就会在新设定的浏览器上运行。

## 3. 设置登录密码

假如你对自己的Jupyter目录很敏感，不想让别人轻易使用，那么可以设置登录密码。步骤如下：

1. 用记事本打开配置文件`jupyter_notebook_config.py`；
2. Crtl + F组合键找到`c.NotebookApp.allow_password_change`元素，修改为：`NotebookApp.allow_password_change=False`，并且删掉前面的注释`#`，保存文件；
3. 回到windows命令行，运行`jupyter notebook password`，按照提示输入新密码（注意这里的密码是不显示的）；

![img](https://filescdn.proginn.com/2e3b6c0b7a6f7848a4fa684f473dac06/596372de93a583ec3d458a29fe1391c7.webp)

1. 可以看到上一步生成了一个json文件，保存在`.jupyter`文件夹里，和配置文件一个位置。这个json文件保存了密码生成的一段哈希值。找到该文件并打开，复制这段哈希值。![img](https://filescdn.proginn.com/bd90a3ecbb37dd1936b3ac881637dcc1/6f6e1ea04a8295416b7d5879943c2aaa.webp)
2. 再一次打开配置文件`jupyter_notebook_config.py`；
3. Crtl + F组合键找到`c.NotebookApp.password`元素，将前面的哈希值添加到后面，并且删掉前面的注释`#`，保存文件；示例：`c.NotebookApp.password = u'sha1:67c9e60bb8b6:9ffede0825894254b2e042ea597d771089e11aed'`
4. 到这里全部设置好了，重启Jupyter，就可以输入新密码登录。![img](https://i.loli.net/2020/11/19/HRlFv7B4wGk3c1S.png)

## 4.安装扩展插件

Jupyter让很多人喜欢的原因在于它提供了丰富的插件，包括显示代码执行时间、生成目录、显示变量名、代码块折叠等各种让你舒适的功能。

使用插件前，必须要安装扩展nbextensions。

全程在命令行安装，步骤如下:

1. 安装nbextensions 执行`pip install jupyter_contrib_nbextensions`;
2. 安装javascript and css files 执行`jupyter contrib nbextension install --user`;
3. 安装configurator 执行`pip install jupyter_nbextensions_configurator`
4. 重启 Jupyter Notebook， 能看到nbextension 标签

![img](https://i.loli.net/2020/11/19/3qtiMGoSPVTzkba.png)



## Conda 常用命令



## 1. 获取版本号

```python
conda --version
conda -V
```

## 2. 获取帮助

```python
conda --help
conda -h
```



查看某一命令的帮助，如update命令及remove命令

```python
conda --help update
conda --help remove
```

## 3. 更新管理

更新conda，保持conda最新：

```python
conda update conda
```

更新anaconda

```python
conda update anaconda
```

更新Python

```python
conda update python
```

假设当前环境是python3.6，conda会将python升级为3.6x系列的当前最新版本

## 4. 环境管理

创建新环境

```python
conda create -n '环境名' python='版本号'
```

卸载环境

```python
conda remove -n '环境名' --all
```

查看所有环境

```python
conda info -e
```

激活指定环境

```python
source activate '环境名'
```

退出当前环境

```python
source deactivate 
```



## 5. 包管理

安装包

```python
conda install '包的名字'
conda install '包的名字'='版本号'  eg: conda install tensorflow=1.10
```

更新包

```python
conda update '包的名字'
#更新所有包
conda update --all
```

搜索包(目的是查看可获得的版本)

```python
conda search '包的名字' eg: conda search tensorflow
```

列出当前环境所有包

```python
conda list
```

查看指定环境所有包

```python
conda list -n '环境名'
```

删除环境中的某个包

```python
conda remove -n '环境名' '包的名字'
```

## 踩坑

## 问题01 Jupyter Notebook：kernel error 已解决

### 问题描述

Jupyter Notebook出现`kernel error`
 当时用Anaconda安装多个版本的Python的时候，时常由于安装和卸载多次Python导致Juoyter notebook不可用。常常导致如下结果



```ruby
File”//anaconda/lib/python2.7/site-packages/jupyter_client/manager.py”, line 190, in _launch_kernel 
return launch_kernel(kernel_cmd, **kw) 
File “//anaconda/lib/python2.7/site-packages/jupyter_client/launcher.py”, line 123, in launch_kernel 
proc = Popen(cmd, **kwargs) 
File “//anaconda/lib/python2.7/subprocess.py”, line 710, in init 
errread, errwrite) 
File “//anaconda/lib/python2.7/subprocess.py”, line 1335, in _execute_child 
raise child_exception 
OSError: [Errno 2] No such file or director
```

### 解决方案

- 运行`python -m ipykernel install --user`重新安装内核
- 如果有多个内核，先运行`conda create -n python2 python=2`，为Python2.7设置Anaconda变量，在Anacoda下使用`activate pyhton2`切换python环境
- 重启jupyter notebook即可

### 小技巧

查看安装的内核和位置

```
jupyter kernelspec list
```

进入安装内核目录打开 **kernel.json** 文件，查看Python编译器的路径

## 问题02

## 参考

1. Getting started with conda https://conda.io/projects/conda/en/latest/user-guide/getting-started.html
2. Command reference https://docs.conda.io/projects/conda/en/latest/commands.html
3. Anaconda 镜像使用帮助 https://mirrors.tuna.tsinghua.edu.cn/help/anaconda/
4. Conda常用命令整理 https://zhengyujie.cn/2257.html
5. Conda 常用命令的整理 https://morooi.cn/2019/anaconda/
6. 