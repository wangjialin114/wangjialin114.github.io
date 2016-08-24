---
layout:     post
title:      "Ubuntu16.04系统安利篇"
subtitle:   "Loneliness Makes Man Stronger!"
date:       2016-07-20
author:     "WangJialin"
tags:
    - linux
    
---
- [1. 华中科技大学校园网认证](#mentohust)
- [2. ubuntu下良心软件](#software)
- [3. Firefox插件安装](#firefox)
- [4. Linux系统备份](#backup)
- [5. 调整分区大小](#adjust_partition)



如今DeepLearning炒的非常火，突然一下子，如日中天。虽然我本人一向对这种大吹特吹的东西有种天生的抵触感，不过还是打算抱着学习的态度了解下。找到了自然语言处理和深度学习的交叉领域作为学习的切入点，正好stanford也有这门课（链接：http://cs224d.stanford.edu/），没有视频资源，不过资料，作业之类的很全。这门课用的深度学习平台试tensorflow，而tensorflow的GPU版本却只支持Linux版本，没办法，所以又来折腾linux了。以前玩过一下，感觉实在太麻烦，这次打算咬着头皮玩，和它死磕。这几天，也真的是快被逼疯了。听说ubuntu12.04是最经典，最稳定的版本,最开始也是选择的12.04,可是安装12.04和14.04无数遍，各种问题，最后没办法转向刚发布的16.04，说实话，内心不是很看好，毕竟发布没有多久。不过这款新的系统真的是给了我很多的惊喜。
可是ubuntu对于校园网来说，有个很大的问题就是校园网的认证。毕竟有线网的速度快很多。比较有名的校园网认证客户端有锐捷客户端。
华中科技大学的校园网采用的是锐捷认证，官方版本没有linux下面的，后来有大神自己写了个mentohust用于linux认证。


----------


<a name="mentohust"></a>

## 1. 华中科技大学校园网认证

### 1.1 (deb包)下载地址：

MentoHUST V0.3.4 for Ubuntu i386  与 MentoHUST V0.3.4 for Ubuntu amd64 下载
免费下载地址在 http://linux.linuxidc.com/
用户名与密码都是www.linuxidc.com
具体下载目录在 /2013年资料/1月/20日/Ubuntu下使用MentoHUST代替锐捷认证上网

### 1.2 安装配置

安装命令： `sudo dpkg -i deb包`
华中科技大学采用的dhcp配置动态IP的方式。
**配置有线网连接：**

 - **选择有线网的网卡**：

![img](/img/2016_latter_half_year/ethernet-device.png)

 - **选用IPV4连接：**
![img](/img/2016_latter_half_year/ipv4_connect.png)

### 1.3 使用

 - **运行命令：**
 
```
sudo mentohust -uusername -p123456 -a1 -d2 -b2 -v4.10 -w
```

其中，参数-b若设置为1或2，则当关闭该终端或使用Ctrl+C时不会断开连接；参数-w表示将配置信息默认保存至文件/etc/mentohust.conf，当需要再次认证时，仅需要键入命令`sudo mentohust`即可。
认证过程：
![img](/img/2016_latter_half_year/mentohust_login.png)
对于静态IP用户，需要先在Network Connections中配置好IP、网关、掩码、DNS等信息，再参照帮助信息设置。
 - **退出认证：**

```
sudo mentohust -k
```

或

```
pkill mentohust
```


<a name="software"></a>

## 2. ubuntu下良心软件

 - **网易云音乐**（http://music.163.com/#/download）
 - **有道词典**
 - **Thunderbird邮件**
 - **tmux分屏**
![img](/img/2016_latter_half_year/tmux.png)
 - **sublime Text + Jedi(python auto_completation package)**:打造轻量级IDE

<a name="firefox"></a>

## 3. Firefox浏览器插件安装

ubuntu下浏览器默认的都不支持flash，所以有时候会看不了视频。今天安装这个折腾了一下子，不知道为什么，即使按照官网的步骤来还是没有成功，最后是下面这条简短的命令救了我。

##FlashPlayer安装：

```
$ sudo apt-get install flashplugin-nonfree
```

### 更新：

```
$ sudo update-flashplugin-nonfree --install
```

### 卸载：

```
$ sudo update-flashplugin-nonfree --uninstall
$ sudo apt-get remove flashplugin-nonfree
```

## 网页缩放插件Default Full Zoom Level

由于默认的firefox网页太小，看着实在难受，安装这个插件可以设置适合自己的默认的缩放比例。

<a name="backup"></a>

## 4. Linux系统的备份

在ubuntu16.04中，有备份图形化界面选项，
![img](/img/2016_latter_half_year/linux_bk.png)
![img](/img/2016_latter_half_year/linux_bk_2.png)
当然，也可以用命令进行备份：


 - 使用sudo命令
 - 备份：

 - `#cd /`
 
 - `# tar cvpzf backup.tgz --exclude=/proc --exclude=/lost+found --exclude=/backup.tgz --exclude=/mnt --exclude=/sys /`

注意，exclude前面是两条短划线， 同时，一定要记住将备份文件本身排除在备份的文件之外，不然有可能造成循环备份，不停的备份其本身。
在备份命令结束时你可能会看到这样一个提示：’tar: Error exit delayed from previous errors’，**"tar: 由于前次错误，将以上次的错误状态退出"**多数情况下你可以忽略它。
 - 使用下面的命令来恢复系统：

```
# tar xvpfz backup.tgz -C /
```

 - 注意：上面的命令会用档案文件中的文件覆盖分区上的所有文件。

执行恢复命令之前请再确认一下你所键入的命令是不是你想要的，执行恢复命令可能需要一段不短的时间。

恢复命令结束时，你的工作还没完成，别忘了重新创建那些在备份时被排除在外的目录：

```
# mkdir proc
# mkdir lost+found
# mkdir mnt
# mkdir sys`
```

等等

ubuntu现在在图形化界面上面越做越nice，个人感觉是朝着mac的风格去的。大家有兴趣的可以安装ubuntu16.04Desktop版本试试，感觉真心不错。大家一直吐嘈图形化的这面应该以后不是个问题，我觉得最大的问题是各种应用软件太少了，另外游戏这方面更是捉襟见肘。不过如果作为一个开发者，选用ubuntu比较适合。一些重要的必备的各种软件基本上不缺。

<a name="adjust_partition"> </a>

## 5. 调整Linux磁盘分区大小

利用[Gparted分区编辑器制作LiveCD用于重新调整分区](http://gparted.org/livecd.php)

# 持续更新中
参考链接：http://www.linuxidc.com/Linux/2013-01/78222.htm

