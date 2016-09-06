---
ms.openlocfilehash: 09ac9ad40ff5e3be722bba1e70b0fd63e913b767
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="Linux 在虚拟机上安装灯泡堆栈"
    description="了解如何安装灯泡堆栈在 Azure 中的 Linux 虚拟机 (VM)。 您可以安装 CentOS Ubuntu 上。"
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



#在 Azure 中的 Linux 虚拟机上安装灯泡堆栈

灯泡堆栈由以下各元素组成︰

- **L**inux 的操作系统
- **A**pache-Web 服务器
- **M**ySQL-数据库服务器
- **P**HP 的编程语言


##在 Ubuntu 上安装

您将需要安装以下软件包︰

- `apache2`
- `mysql-server`
- `php5`
- `php5-mysql`

运行后`apt-get update`更新包的本地列表，然后可以安装这些软件包都通过一个`apt-get install`命令︰

    # sudo apt-get update
    # sudo apt-get install apache2 mysql-server php5 php5-mysql

运行上述命令之后将提示您安装这些软件包和其他依赖项的数量。  请按 y，然后按 Enter 继续，然后按照任何其他提示为 MySQL 设置管理员密码。

这将安装 PHP 使用 MySQL 所需最小需要的 PHP 扩展。 运行下面的命令以查看其他可用的软件包形式的 PHP 扩展︰

    # apt-cache search php5


##在 CentOS 和 Oracle Linux 上安装

您将需要安装以下软件包︰

- `httpd`
- `mysql`
- `mysql-server`
- `php`
- `php-mysql`

您可以使用单一安装这些软件包`yum install`命令︰

    # sudo yum install httpd mysql mysql-server php php-mysql

运行上述命令之后将提示您安装这些软件包和其他依赖项的数量。  请按 y，然后按 Enter 继续。

这将安装 PHP 使用 MySQL 所需最小需要的 PHP 扩展。 运行下面的命令以查看其他可用的软件包形式的 PHP 扩展︰

    # yum search php


## 在 SUSE Linux 企业服务器上安装

您将需要安装以下软件包︰

- apache2
- mysql
- apache2 mod_php53
- php53-mysql

您可以使用单一安装这些软件包`zypper install`命令︰

    # sudo zypper install apache2 mysql apache2-mod_php53 php53-mysql

运行上述命令之后将提示您安装这些软件包和其他依赖项的数量。  请按 y，然后按 Enter 继续。

这将安装 PHP 使用 MySQL 所需最小需要的 PHP 扩展。 运行下面的命令以查看其他可用的软件包形式的 PHP 扩展︰

    # zypper search php


设置
----------

1. 设置**Apache**

    - 运行以下命令来确保 Apache web 服务器已启动︰

        - Ubuntu 和 SLES: `sudo service apache2 restart`

        - CentOS 与 Oracle: `sudo service httpd restart`

    - 在默认端口 80 上侦听 apache。 您可能需要打开远程访问 Apache 服务器的终结点。  请其[配置终结点](virtual-machines-set-up-endpoints.md)的详细说明，参阅文档。

    - 现在，您可以检查以查看 Apache 是运行，提供的内容。 您浏览器指向`http://[MYSERVICE].cloudapp.net`，其中**[MYSERVICE]**是虚拟机所在的云服务的名称。 您可能会遇到被默认 web 页，只是指出一些分布在"它工作 ！"。 在其他人可能会看到带有指向其他文档和内容的 Apache 服务器配置更完整的网页。

2. 设置**MySQL**

    - 请注意此步骤不是必需在 Ubuntu，提示您指定 MySQL`root`安装 mysql 服务器软件包时的密码。

    - 上其它版本，通过运行以下命令设置为 MySQL 的超级用户口令︰

            # mysqladmin -u root -p password yourpassword

    - 然后可以管理使用 MySQL`mysql`或`mysqladmin`实用程序。


##其他阅读材料

假设您希望自动执行这些步骤以应用程序部署到远程 Linux 虚拟机？ 你可以使用 Linux CustomScript 扩展名。 请参见[使用 Linux Azure CustomScript 扩展一个灯应用程序部署](virtual-machines-linux-script-lamp.md)。

还有很多其他资源设置在 Ubuntu 上灯堆栈。

- [https://help.ubuntu.com/community/ApacheMySQLPHP](https://help.ubuntu.com/community/ApacheMySQLPHP)
 