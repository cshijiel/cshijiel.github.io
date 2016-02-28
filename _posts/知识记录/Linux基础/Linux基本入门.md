title: "Linux基本入门"
date: 2015-04-07 00:20:36
tags: 
- Linux
categories: Linux基础
---
### 选择一个Linux的发行版本
学习Linux的第一件事情，就是要选择一个Linux的发行版本，在虚拟机或者物理机安装都可以了，初学者最好用虚拟机。初学Linux的第一件事情，就是看到众多的Linux分支而头晕，这到底有什么区别呢，为啥Linux不是只有一个版本，而是有很多个版本呢？其实是这样的，Linux其实是一个操作系统内核，但是一个操作系统除了内核，还有用户操作界面，应用软件，例如我们使用的windows，也有windows内核，出了windows内核，还有windows的图形界面，windows的office等应用软件。而Linux是一个免费开源的内核，每个厂家都可以去Linux内核官网http://www.kernel.org/下载内核，然后去订制自己的图形界面和应用软件，所以会出现很多Linux分支，但是内核都是一样的。

目前Linux只要有几个分支:redhat,ubuntu,debian,suse。很多其他linux发行版本是这几个分支的衍生版本，例如国内的红旗，centos都是redhat的衍生版本。

**在服务器领域，个人觉得redhat现在做的最好，桌面领域是ubuntu最好，而我们学习Linux的最大目的是学习Linux的服务器领域，所以我推荐`redhat`版本。**

学校里的linux课本都比较陈旧，大部分是Redhat Linux 9的教程，但是Redhat Linux 9由于硬盘驱动关系，是无法在现在的物理机上安装的，包括本人，也受过大学课本的误导(坑爹的教科书)。

Redhat Linux 9之后，redhat公司不在维护Redhat的开源版本，于是直接发行他的商业版本Redhat Enterprise Linux 2，目前已经有Redhat Enterprise Linux 6，但是6的稳定性还不清楚，个人推荐使用Redhat Enterprise Linux 5，请自行去网上下载Redhat Enterprise Linux 5。

Redhat Enterprise Linux虽然说是商业版本，但是只要你安装的时候，确定你不输入序列号，你还是可以正常使用，只是不能在redhat官网更新软件而已，然后，这里就要提下CentOS了，由于Redhat Enterprise Linux是商业版本，于是CentOS这个组织就和redhat公司买了源代码，并重新编译，免费开放出来，免费让用户可以在centos官网更新软件，包括使用Redhat Enterprise Linux的系统也可以在centos的官网更新软件。大家也可能有疑问，既然centos和redhat都是一模一样，除了名字不一样，为啥不选择centos。其实没任何区别，Centos 5.5就和Redhat Enterprise Linux 5.5是一模一样的，你可以选择centos去安装，去拿redhat的教程学习。


### linux常用命令
- 超级权限：`sudo`
- 拷贝 `cp N.java /home/M.java`
- 删除 `rm [-rf] file.file`
- 关机 `shutdown now`
- 挂载磁盘 `mount 设备 挂载位置`
- Yum-`yum [-y] install`  `yum makecache` 等

- cat tac
- vi file.file

### Linux设置网络
。。。

### Linux 设置权限
    chmod u+x filename

### Linux解压文件
    tar xvf filename

### Vim常见命令

vi两种模式，按i进入`insert`模式，按`Esc`返回到普通模式。

- `hjkl` 左上下右
- :`wq[!]` 保存并退出(`quit`)

