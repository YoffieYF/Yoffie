## Flask搭建一个本地web站点

### 简单介绍
Flask是一个轻量级的搭建站点的框架。Flask可以快速的构建起：移动应用API（网络数据请求接口）。它使用的开发语言是python，因此如果想要更好的使用Flask，必须要了解python这门编程语言。(注：此教程需要在Mac OS X下进行)
<br>
<br>
### 安装
安装Flask非常简单，只需要安装一个虚拟机：virtualenv
```
$ sudo easy_install virtualenv
```
### 项目目录(新建项目)
virtualenv 安装完毕后，你可以立即打开终端 然后创建你自己的开发环境。我通常创建一个项目文件夹，并在其下创建一个“venv”文件夹
```
$ mkdir myproject
$ cd myproject
$ virtualenv venv
New python executable in venv/bin/python
Installing distribute............done.
```
### 启动虚拟机
无论何时你想在某个项目上工作，只需要激活相应的环境。
在 OS X 和 Linux 上，执行如下操作:
```
$ . venv/bin/activate
```
下面的操作适用 Windows:
```
$ venv\scripts\activate
```
### 安装Flask
```
pip install Flask
```

