## 拉钩教育Linux基础

#### Linux的发行版本

- Red Hat Linux   红帽  （全世界应用最广泛的Linux）
- Fedora Linux 
- Slackware Linux
- Ubuntu Linux
- Debian Linux
- SuSE Linux
- 中标麒麟
- 深度DEEPIN

#### Linux的文件目录结构

> **目录结构**

- Bin：全称binary，含义是二进制。该目录中存储的都是一些二进制文件，文件都是可以被运行的。

- Dev：该目录中主要存放的是外接设备，例如盘、其他的光盘等。在其中的外接设备是不能直接被使用的，需要***\*挂载（类似windows下的分配盘符）\****。

- Etc：该目录主要存储一些配置文件。

- Home：表示“家”，表示***\*除了root用户以外其他用户的家目录\****，类似于windows下的User/用户目录。

- Proc：process，表示进程，该目录中存储的是Linux运行时候的进程。

- Root：该目录是root用户自己的家目录。
- Sbin：全称super binary，该目录也是存储一些可以被执行的二进制文件，但是必须得有super权限的用户才能执行。
- Tmp：表示“临时”的，当系统运行时候产生的临时文件会在这个目录存着。
- Usr：存放的是用户自己安装的软件。类似于windows下的program files。
- Var：存放的程序/系统的日志文件的目录。
- Mnt：当外接设备需要挂载的时候，就需要挂载到mnt目录下。

#### 基础指令

- ##### ls指令

***用法1：#ls***

含义：列出当前工作目录下的所有文件/文件夹的名称