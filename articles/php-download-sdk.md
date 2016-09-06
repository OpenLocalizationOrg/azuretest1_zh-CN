---
ms.openlocfilehash: b3035aee3bf83a28f9f79974ca1b1a5243cee0ef
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="下载 php 的 Azure SDK"
    description="了解如何下载并安装的 PHP 的 Azure SDK。"
    documentationCenter="php"
    services=""
    authors="tfitzmac"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="PHP"
    ms.topic="article"
    ms.date="08/31/2015"
    ms.author="tomfitz"/>

#下载 php 的 Azure SDK

## 概述

Azure SDK php 包含允许您开发、 部署和管理 Azure 的 PHP 应用程序的组件。 具体而言，php Azure SDK 包含下列内容︰

* **PHP 的 Azure 的客户端库**。 这些类库访问 Azure 的功能，如数据管理服务的提供的接口和云服务。  
* **Mac、 Linux、 和 (Azure CLI) Windows Azure 的命令行界面**。 这是一套用于部署和管理 Azure 服务，如 Azure 网站和 Azure 的虚拟机的命令。 在任何平台上，包括 Mac、 Linux、 Windows Azure CLI 工作。
* **Azure PowerShell (仅限 Windows)**。 这是一套用于部署和管理 Azure 服务，如云服务和虚拟机的 PowerShell cmdlet。
* **Azure 仿真程序 (仅限 Windows)**。 计算和存储的仿真程序是本地的云服务和数据管理服务，您可以测试应用程序本地的仿真程序。 仅在 Windows Azure 仿真程序运行。

以下各节介绍如何下载并安装上面描述的组件。

本主题中的说明假定您已安装了[PHP][安装 php] 。

> [AZURE.NOTE]
> 您必须具有 PHP 5.3 版或更高版本的 Azure 使用 PHP 客户端库。

##PHP 的 Azure 的客户端库

Azure PHP 客户端库访问 Azure 的功能，如数据管理服务的提供的接口和云服务，从任何操作系统。 这些库可以通过作曲家或梨树安装程序包管理器或手动。

有关如何使用 azure 的 PHP 客户端库的信息，请参阅[如何使用 Blob 服务][blob 服务]、[如何使用表服务][表服务]以及[如何使用队列服务][队列服务]。

###通过作曲家安装

1. [安装 Git][安装 git]。


    > [AZURE.NOTE]
    > 在 Windows 上，您还需要添加到 PATH 环境变量的可执行 git 中获取。

2. 创建名为**composer.json**的项目根目录中的文件并向其中添加下面的代码︰

        {
            "repositories": [
                {
                    "type": "pear",
                    "url": "http://pear.php.net"
                }
            ],
            "require": {
                "pear-pear.php.net/mail_mime" : "*",
                "pear-pear.php.net/http_request2" : "*",
                "pear-pear.php.net/mail_mimedecode" : "*",
                "microsoft/windowsazure": "*"
            }
        }

3. 下载**[composer.phar][作曲家 phar]**中的项目根。

4. 打开一个命令提示符并执行中的项目根

        php composer.phar install

###作为梨树包安装

要为梨树包安装 Azure PHP 客户端库，请执行以下步骤︰

1. [安装梨树][安装梨树]。
2. 设置 Azure 梨树通道︰

        pear channel-discover pear.windowsazure.com
3. 安装梨树程序包︰

        pear install pear.windowsazure.com/WindowsAzure-0.4.0

安装完成后，您可以从您的应用程序引用类库。

###手动安装

要下载并手动安装 Azure PHP 客户端库，请执行以下步骤︰

1. 下载.zip 存档包含从[GitHub][github 的 sdk 的 php]库。 或者，分叉存储库并克隆它到您的本地计算机。 （第二种选项需要 GitHub 帐户并让 Git 安装在本地）。

    > [AZURE.NOTE]
    > Azure PHP 客户端库的[HTTP_Request2](http://pear.php.net/package/HTTP_Request2)、 [Mail_mime](http://pear.php.net/package/Mail_mime)和[Mail_mimeDecode](http://pear.php.net/package/Mail_mimeDecode)梨树软件包具有相关性。 解决这些依赖项的推荐的方式是安装这些软件包使用[梨树程序包管理器](http://pear.php.net/manual/en/installation.php)

2. 复制`WindowsAzure`从您的应用程序下载到您的应用程序目录结构和引用类档案的目录。

##Azure PowerShell 和 Azure 仿真程序

Azure PowerShell 是一套用于部署和管理 Azure 服务 （如云服务和虚拟机） PowerShell cmdlet。 Azure 仿真程序是云服务和数据管理服务，您可以测试应用程序本地的仿真程序。 这些组件支持仅限 Windows。

安装 Azure PowerShell 和 Azure 仿真程序的推荐的方式是使用[Microsoft Web 平台安装程序][下载 wpi]。 请注意，您还可以安装其他开发组件，例如 PHP、 SQL Server、 SQL Server WebMatrix PHP，以及 Microsoft 驱动程序。

有关如何使用 Azure PowerShell 的信息，请参阅[如何使用 Azure PowerShell][powershell 工具]。

##Azure CLI

Azure CLI 可以套用于部署和管理 Azure 服务，如 Azure 网站和 Azure 的虚拟机的命令。 有关安装 Azure CLI 的信息，请参阅[安装 Azure CLI](xplat-cli-install.md)。




[安装 php]: http://www.php.net/manual/en/install.php
[作曲家 github]: https://github.com/composer/composer
[作曲家 phar]: http://getcomposer.org/composer.phar
[梨树净]: http://pear.php.net/
[http 的次数 2-包]: http://pear.php.net/package/HTTP_Request2
[-邮包 mimedecode]: http://pear.php.net/package/Mail_mimeDecode
[邮包的 mime]: http://pear.php.net/package/Mail_mime
[安装梨树]: http://pear.php.net/manual/en/installation.getting.php
[nodejs 组织]: http://nodejs.org/
[安装节点 linux]: https://github.com/joyent/node/wiki/Installing-Node.js-via-package-manager
[下载 wpi]: http://go.microsoft.com/fwlink/?LinkId=253447
[mac 安装程序]: http://go.microsoft.com/fwlink/?LinkId=252249
[blob 服务]: http://go.microsoft.com/fwlink/?LinkId=252714
[表服务]: http://go.microsoft.com/fwlink/?LinkId=252715
[队列服务]: http://go.microsoft.com/fwlink/?LinkId=252716
[azure cli]: http://go.microsoft.com/fwlink/?LinkId=252717
[powershell 工具]: http://go.microsoft.com/fwlink/?LinkId=252718
[php 的 sdk github]: http://go.microsoft.com/fwlink/?LinkId=252719
[安装 git]: http://git-scm.com/book/en/Getting-Started-Installing-Git

testbiubiubiu
