---
ms.openlocfilehash: ffc8ad6dc2252bb5109a86ac335c468d4e572242
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="部署使用 Azure CustomScript 扩展名的 Linux 应用程序"
    description="了解如何使用 Azure CustomScript 扩展部署 Linux 虚拟机上的应用程序。"
    editor="tysonn"
    manager="timlt"
    documentationCenter=""
    services="virtual-machines"
    authors="gbowerman"/>

<tags
    ms.service="virtual-machines"
    ms.workload="multiple"
    ms.tgt_pltfrm="linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/23/2015"
    ms.author="guybo"/>

#将用于 Linux 使用 Azure CustomScript 扩展一个灯应用程序部署#

Linux Microsoft Azure CustomScript 扩展提供通过运行任意代码 （例如，Python 和 Bash） VM 支持任何脚本语言中编写自定义您的虚拟机 (Vm)。 这提供了一种非常灵活的方式自动执行应用程序部署到多台计算机。

您可以部署使用 Azure 门户、 Windows PowerShell 或 Azure 命令行界面 (Azure CLI) CustomScript 扩展名。

在本文中我们将简单灯将应用部署到使用 Azure CLI 的 Ubuntu。

## 先决条件

对于下一步的示例中，首先创建两个 Azure 虚拟机运行 Ubuntu 14.04。 虚拟机称为*脚本 vm*和*灯泡 vm*。 当您创建虚拟机，请使用唯一的名称。 一个用于运行 CLI 命令和一个用于部署灯的应用程序。

您还需要 Azure 存储帐户和密钥来访问它 （您可以从 Azure 门户会得到）。

如果您需要帮助在 Azure 上创建 Linux 虚拟机是指[创建虚拟机运行 Linux](virtual-machines-linux-tutorial.md)。

安装命令假定 Ubuntu，但您可以修改任何受支持的 Linux distro 的安装。

需要有安装具有有效的连接到 Azure 的 Azure CLI 脚本 vm 虚拟机。 有关与此的帮助请参阅[安装和配置 Azure 命令行界面](../xplat-cli.md)。

## 将上载脚本

在此示例中 CustomScript 扩展远程 VM 安装灯泡堆栈并创建一个 PHP 页面上运行脚本。 从任意位置访问该脚本以便我们将为 Azure 的 blob 将其上载。

### 脚本概述

下一个脚本示例安装 Ubuntu （包括设置无提示安装 MySQL 的） 灯堆栈，写一个简单的 PHP 文件并启动 Apache。

    #!/bin/bash
    # set up a silent install of MySQL
    dbpass="mySQLPassw0rd"

    export DEBIAN_FRONTEND=noninteractive
    echo mysql-server-5.6 mysql-server/root_password password $dbpass | debconf-set-selections
    echo mysql-server-5.6 mysql-server/root_password_again password $dbpass | debconf-set-selections

    # install the LAMP stack
    apt-get -y install apache2 mysql-server php5 php5-mysql  

    # write some PHP
    echo \<center\>\<h1\>My Demo App\</h1\>\<br/\>\</center\> > /var/www/html/phpinfo.php
    echo \<\?php phpinfo\(\)\; \?\> >> /var/www/html/phpinfo.php

    # restart Apache
    apachectl restart

### 将上载脚本

将脚本保存为文本文件，例如为*lamp_install.sh*，然后再将其上载到 Azure 存储。 你可以很容易地使用 Azure CLI。 下面的示例将文件上载到的存储容器，名为"脚本"。 如果不存在该容器需要先创建它。

    azure storage blob upload -a <yourStorageAccountName> -k <yourStorageKey> --container scripts ./install_lamp.sh

介绍如何从 Azure 存储下载脚本的 JSON 文件创建。 另存为*public_config.json* （与您的存储帐户的名称替换"mystorage"）︰

    {"fileUris":["https://mystorage.blob.core.windows.net/scripts/install_lamp.sh"], "commandToExecute":"sh install_lamp.sh" }


## 部署扩展

现在可以使用下一个命令将部署到远程 VM 使用 Azure CLI 的 Linux CustomScript 扩展名。

    azure vm extension set -c "./public_config.json" lamp-vm CustomScriptForLinux Microsoft.OSTCExtensions 1.*

在前一个命令下载并称为*灯 vm*虚拟机上运行*lamp_install.sh*脚本。

由于该应用程序包括一个 web 服务器，请记住要使用下一个命令打开远程 VM 上的 HTTP 监听端口。

    azure vm endpoint create -n Apache -o tcp lamp-vm 80 80

## 监视和故障排除

您可以通过查看远程 VM 上日志文件的自定义脚本的运行如何更好地进行检查。 SSH 到*灯泡 vm*和尾部下一个命令与日志文件。

    cd /var/log/azure/Microsoft.OSTCExtensions.CustomScriptForLinux/1.3.0.0/
    tail -f extension.log

运行 CustomScript 扩展之后，您可以浏览到创建信息的 PHP 页。 本文中的示例的 PHP 页面是*http://lamp-vm.cloudapp.net/phpinfo.php*。

## 其他资源

可以使用相同的基本步骤来部署更复杂的应用程序。 在此示例中的安装脚本保存为 Azure 存储中的公钥 blob。 更安全的选项是为具有[安全访问权限签名](https://msdn.microsoft.com/library/azure/ee395415.aspx)(SAS) 安全 blob 存储安装脚本。

下文列出了 Azure CLI、 Linux 和 CustomScript 扩展名的其他资源。

[自动执行 Linux 虚拟机使用 CustomScript 扩展名的自定义任务](http://azure.microsoft.com/blog/2014/08/20/automate-linux-vm-customization-tasks-using-customscript-extension/)

[Azure Linux 扩展 (GitHub)](https://github.com/Azure/azure-linux-extensions)

[Linux 和开源在 Azure 上计算](virtual-machines-linux-opensource.md)
