---
ms.openlocfilehash: 4b08d6aee961c8418b8ff718d534b298d0a2c5a0
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="如何在 Azure 上安装 MySQL "
    description="了解如何在 Azure 中的 Linux 虚拟机 (VM) 上安装 MySQL 栈。 您可以安装 Ubuntu 或 RedHat 系列操作系统上。"
    services="virtual-machines"
    documentationCenter=""
    authors="SuperScottz"
    manager="timlt"
    editor=""/>

<tags
    ms.service="virtual-machines"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/10/2015"
    ms.author="mingzhan"/>


#如何在 Azure 上安装 MySQL


在本文中，您将学习如何安装和配置 MySQL 运行 Linux Azure 的虚拟机上。


> [AZURE.NOTE] 您必须已经为了完成本教程中运行 Linux 的 Microsoft Azure 虚拟机。 若要创建和设置 Linux 虚拟机使用[Azure Linux 虚拟机教程](virtual-machines-linux-tutorial.md)，请参阅`mysqlnode`作为虚拟机名称和`azureuser`为用户在继续操作之前。

[在这种情况下，请使用 3306 端口为 MySQL 端口。  


##在虚拟机上安装 MySQL
连接到 Linux 通过 putty 创建的虚拟机。 如果这是首次使用 Azure Linux 虚拟机，请参阅如何使用 putty 连接到 Linux 虚拟机[下面](virtual-machines-linux-use-ssh-key.md)。

我们将使用存储库软件包安装 MySQL5.6 作为本文中的示例。 实际上，MySQL5.6 比 MySQL5.5 的性能有更多改进。  详细信息[在此处](http://www.mysqlperformanceblog.com/2013/02/18/is-mysql-5-6-slower-than-mysql-5-5/)。


###如何在 Ubuntu 上安装 MySQL5.6
我们将使用 Linux 虚拟机从 Azure 的 Ubuntu 此处。 

- 步骤 1︰ 安装 MySQL 服务器 5.6 切换到`root`用户︰ 

            #[azureuser@mysqlnode:~]sudo su -

    安装 mysql 服务器 5.6:

            #[root@mysqlnode ~]# apt-get update
            #[root@mysqlnode ~]# apt-get -y install mysql-server-5.6

    在安装期间，您将看到对话框窗口 poping 达要求您设置 MySQL root 密码，并且您需要在此处设置的密码。

    ![image](./media/virtual-machines-linux-install-mysql/virtual-machines-linux-install-mysql-p1.png)

    
    输入密码进行确认。

    ![image](./media/virtual-machines-linux-install-mysql/virtual-machines-linux-install-mysql-p2.png)
 
- 步骤 2︰ 登录 MySQL 服务器

    在 MySQL 服务器安装完成后，将自动启动 MySQL 服务。 您可以登录 MySQL 服务器`root`用户。
    使用以下登录和输入密码的命令。

             #[root@mysqlnode ~]# mysql -uroot -p

- 步骤 3︰ 管理正在运行 MySQL 服务

    （a） 获得 MySQL 服务的状态

             #[root@mysqlnode ~]# service mysql status

    （b） 启动 MySQL 服务

             #[root@mysqlnode ~]# service mysql start

    （c） 停止 MySQL 服务

             #[root@mysqlnode ~]# service mysql stop

    （d） 重新启动 MySQL 服务

             #[root@mysqlnode ~]# service mysql restart


###如何安装 MySQL 在 CentOS，Oracle Linux 类似 Red Hat 操作系统家族
我们将在此处使用 CentOS 或 Oracle Linux 具有 Linux 虚拟机。
 
- 步骤 1︰ 添加 MySQL Yum 库切换到`root`用户︰ 

            #[azureuser@mysqlnode:~]sudo su -

    下载并安装 MySQL 版本程序包︰ 

            #[root@mysqlnode ~]# wget http://repo.mysql.com/mysql-community-release-el6-5.noarch.rpm 
            #[root@mysqlnode ~]# yum localinstall -y mysql-community-release-el6-5.noarch.rpm 

- 步骤 2︰ 编辑文件以启用 MySQL 资料库下载 MySQL5.6 包下。
 
            #[root@mysqlnode ~]# vim /etc/yum.repos.d/mysql-community.repo

    更新至以下获得此文件的每个值︰

        \# *Enable to use MySQL 5.6*

        [mysql56-community]
        name=MySQL 5.6 Community Server

        baseurl=http://repo.mysql.com/yum/mysql-5.6-community/el/6/$basearch/

        enabled=1

        gpgcheck=1

        gpgkey=file:/etc/pki/rpm-gpg/RPM-GPG-KEY-mysql

- 步骤 3︰ 安装 MySQL 从 MySQL 安装 MySQL 资料库︰

           #[root@mysqlnode ~]#yum install mysql-community-server 

    将安装 MySQL RPM 包和所有相关的包。

- 步骤 4︰ 管理正在运行 MySQL 服务

    （a） 检查 MySQL 服务器的服务状态︰
   
           #[root@mysqlnode ~]#service mysqld status

    （b） 检查默认端口的 MySQL 服务器是否正在运行︰

           #[root@mysqlnode ~]#netstat  –tunlp|grep 3306


    （c） 启动 MySQL 服务器︰

           #[root@mysqlnode ~]#service mysqld start

    （d） 停止 MySQL 服务器︰

           #[root@mysqlnode ~]#service mysqld stop

    （e） 设置 MySQL 时启动系统引导向上︰

           #[root@mysqlnode ~]#chkconfig mysqld on


###在 SUSE Linux 上安装 MySQL 的方式
我们将使用 Linux 虚拟机与 OpenSUSE 此处。

- 步骤 1︰ 下载并安装 MySQL 服务器

    切换到`root`用户通过以下命令︰  

           #sudo su -

    下载并安装 MySQL 包︰

           #[root@mysqlnode ~]# zypper update

           #[root@mysqlnode ~]# zypper install mysql-server mysql-devel mysql

- 步骤 2︰ 管理正在运行 MySQL 服务

    （a） 检查 MySQL 服务器的状态︰

           #[root@mysqlnode ~]# rcmysql status

    （b） 检查是否 MySQL 服务器的默认端口︰

           #[root@mysqlnode ~]# netstat  –tunlp|grep 3306


    （c） 启动 MySQL 服务器︰

           #[root@mysqlnode ~]# rcmysql start

    （d） 停止 MySQL 服务器︰

           #[root@mysqlnode ~]# rcmysql stop

    （e） 设置 MySQL 时启动系统引导向上︰

           #[root@mysqlnode ~]# insserv mysql

###下一步
查找更多的使用情况和有关 MySQL[此处](https://www.mysql.com/)的信息。