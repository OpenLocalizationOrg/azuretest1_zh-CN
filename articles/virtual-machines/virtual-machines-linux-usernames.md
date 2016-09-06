---
ms.openlocfilehash: 3fbff8bbc15553944a9551c3d26f1707352ce9d0
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="选择在 Azure 上的 linux 用户名称" 
    description="了解如何选择在 Azure 中的 Linux 虚拟机的用户名称。" 
    services="virtual-machines" 
    documentationCenter="" 
    authors="szarkos" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="virtual-machines" 
    ms.workload="infrastructure-services" 
    ms.tgt_pltfrm="vm-linux" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/29/2015" 
    ms.author="szark"/>



#选择在 Azure 上的 linux 用户名称#

当您配置 Linux 虚拟机在 Azure 上必须指定非根用户，您以后可用于登录到虚拟机的名称。 您可以选择该新用户的名称，或通过管理门户设置如果您可以接受默认名称"azureuser"。

在大多数情况下此用户将不会存在于基图像和设置过程中创建。 如果用户存在基本的虚拟机映像，然后 Azure Linux 代理只配置的密码 （和/或 SSH 密钥） 用户根据 VM 创建时指定的信息。

**不过**，Linux 定义一组用户名称不应使用。 设置过程将**失败**如果您尝试配置 Linux 虚拟机时使用现有系统用户，是指具有 UID 0-99 的用户。 一个典型的例子是`root`用户，有 UID 0。

 - 另请参见︰ [Linux 标准基-用户 ID 的范围](http://refspecs.linuxfoundation.org/LSB_4.1.0/LSB-Core-generic/LSB-Core-generic/uidrange.html)

下面是用户名称，则应避免使用 Linux 虚拟机资源调配时。 我们建议您**不要使用这些用户名**，否则虚拟机资源调配过程可能会失败。


## openSUSE
- abrt
- adm
- 音频
- 纸盒
- 纸盒
- 光驱
- cgred
- 守护程序
- dbus
- 拨出
- 调节
- 磁盘
- 软盘
- ftp
- 游戏
- gopher
- haldaemon
- 暂停
- kmem
- 锁定
- lp
- 邮件
- 手册
- 记忆
- nfsnobody
- 没有人
- ntp
- 运算符
- oprofile
- postdrop
- 后缀
- qpidd
- 根
- rpc
- rpcuser
- saslauth
- 关机
- slocate
- sshd
- stapdev
- stapusr
- 同步
- sys
- 磁带
- 测试
- tcpdump
- tty
- 用户
- utempter
- utmp
- uucp
- vcsa
- 视频
- 滚轮


## SLES
- 音频
- 纸盒
- 光驱
- 控制台
- 守护程序
- 拨出
- 磁盘
- 软盘
- ftp
- ftp
- 游戏
- haldaemon
- kmem
- lp
- lp
- 邮件
- maildrop
- 手册
- messagebus
- 调制解调器
- 新闻
- 新闻
- 没有人
- nogroup
- polkituser
- 后缀
- 公用
- 根
- 阴影
- sshd
- sys
- 测试
- 受信任的
- tty
- 用户
- utmp
- uucp
- uuidd
- 视频
- 滚轮
- www
- wwwrun
- xok

 
## CentOS
- abrt
- adm
- 音频
- 纸盒
- 光驱
- cgred
- 守护程序
- dbus
- 拨出
- 调节
- 磁盘
- 软盘
- ftp
- ftp
- 游戏
- gopher
- haldaemon
- 暂停
- kmem
- 锁定
- lp
- 邮件
- 手册
- 记忆
- nfsnobody
- 没有人
- ntp
- 运算符
- oprofile
- postdrop
- 后缀
- qpidd
- 根
- rpc
- rpcuser
- saslauth
- 关机
- slocate
- sshd
- stapdev
- stapusr
- 同步
- sys
- 磁带
- 测试
- tcpdump
- tty
- 用户
- utempter
- utmp
- uucp
- vcsa
- 视频
- 滚轮


## Ubuntu
- adm
- 管理
- 音频
- 备份
- 纸盒
- 光驱
- crontab
- 守护程序
- 拨出
- 调节
- 磁盘
- fax
- 软盘
- 保险丝
- 游戏
- gnats
- irc
- kmem
- 横向
- libuuid
- 列表
- lp
- 邮件
- 手册
- messagebus
- mlocate
- netdev
- 新闻
- 没有人
- nogroup
- 运算符
- plugdev
- 代理服务器
- 根
- sasl
- 阴影
- src
- ssh
- sshd
- 人员
- sudo
- 同步
- sys
- 系统日志
- 磁带
- tty
- 用户
- utmp
- uucp
- 视频
- 声音
- whoopsie
- www 数据

 