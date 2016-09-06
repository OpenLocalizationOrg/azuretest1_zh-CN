---
ms.openlocfilehash: f2b5c0c07fb2d0089feccf2b80b2b194804ba848
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="从命令行管理移动服务 |Microsoft Azure" 
    description="了解如何创建、 部署和管理您使用命令行工具的 Azure 移动服务。" 
    services="mobile-services" 
    documentationCenter="Mobile" 
    authors="ggailey777" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="mobile-services" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="NA" 
    ms.devlang="multiple" 
    ms.topic="article" 
    ms.date="07/22/2015" 
    ms.author="glenga"/>

# 自动执行命令行工具的移动服务 

##概述

本主题演示如何使用 Azure 的命令行工具来自动创建和管理 Azure 移动服务。 本主题演示了如何安装和开始使用命令行工具和使用它们来执行键移动服务。
 
当合并到一个脚本或批处理文件，这些单个的命令将自动移动服务的创建、 验证和删除过程。 

本主题介绍选定的 Azure 的命令行工具所支持的常见管理任务。 有关详细信息，请参阅[Azure 的命令行工具文档][引用文档]。

##安装的 Azure 的命令行工具

下面的列表包含安装的命令行工具，具体取决于您的操作系统的信息︰

* **Windows**︰ 下载[Azure 命令行工具安装][windows 安装程序]。 打开下载的.msi 文件，然后按照提示完成安装步骤。

* **Mac**︰ 下载[Azure SDK 安装程序][mac 安装程序]。 打开下载的.pkg 文件，请按照提示完成安装步骤。

* **Linux**︰ 安装[Node.js][nodejs 组织]的最新版本 （请参见[安装 Node.js 通过程序包管理器][安装节点 linux]），然后运行以下命令︰

    npm 安装 azure cli-g

若要测试的安装，请键入`azure`在命令提示符下。 成功安装后，您将看到所有可用项的列表`azure`命令。

##如何下载并导入发布设置

若要开始，您必须首先下载并导入您的发布设置。 然后可以使用这些工具来创建和管理 Azure 服务。 下载您的发布设置，请使用`account download`命令︰

    azure account download

这将打开默认浏览器，并提示您登录到管理门户。 中，签名后您`.publishsettings`下载文件。 请注意此已保存文件的位置。

接下来，导入`.publishsettings`通过运行下面的命令文件替换`<path-to-settings-file>`的路径与您`.publishsettings`文件︰

    azure account import <path-to-settings-file>

您可以删除所有存储的信息<code>import</code>命令通过使用<code>account clear</code>命令︰

    azure account clear

若要查看选项列表`account`命令，使用`-help`选项︰

    azure account -help

导入后您发布设置，则应删除`.publishsettings`文件出于安全考虑。 有关详细信息，请参阅[如何安装 Mac 和 Linux 的 Azure 命令行工具]。 现在您可以开始创建和管理 Azure 移动服务从命令行或批处理文件中。  

##如何创建移动服务

您可以使用命令行工具来创建新的移动服务实例。 在创建移动服务时，也创建一个 SQL 数据库实例在一个新的服务器。 

以下命令将创建一个新的移动服务实例在您的订购，其中`<service-name>`是新的移动服务的名称`<server-admin>`的新服务器的登录名和`<server-password>`是新的登录密码︰

    azure mobile create <service-name> <server-admin> <server-password>

`mobile create`命令失败时已存在指定的移动服务。 在自动化脚本，您应该尝试删除移动服务，然后再尝试重新创建它。

##如何列出现有订阅移动服务

> [AZURE.NOTE] 在"列表"和"脚本"与相关的 CLI 命令仅使用 JavaScript 后端。 

以下命令返回 Azure 的预订中的所有移动服务列表︰

    azure mobile list

此命令还显示当前状态和每个移动服务的 URL。

##如何删除现有的移动服务

可以使用命令行工具来删除现有的移动服务，以及相关的 SQL 数据库和服务器。 下面的命令删除该移动的服务，其中`<service-name>`是要删除的移动服务名称︰

    azure mobile delete <service-name> -a -q

通过包括`-a`，`-q`参数，此命令还会删除 SQL 数据库和服务器服务使用的移动而不会显示一条提示。

> [AZURE.NOTE] 如果没有指定<code>-q</code>参数沿<code>-a</code>或<code>-d</code>，执行将暂停，并提示您选择您的 SQL 数据库的删除选项。 仅使用<code>-a</code>参数时没有其他服务使用的数据库或服务器;否则使用<code>-d</code>参数仅删除属于移动服务被删除的数据。

##如何在移动服务中创建表

下面的命令创建一个表中指定的移动服务，其中`<service-name>`是移动服务的名称和`<table-name>`为要创建的表的名称︰

    azure mobile table create <service-name> <table-name>

此创建一个新表的默认权限， `application`，表操作︰ `insert`， `read`， `update`，和`delete`。 

下面的命令创建一个新表的公共`read`权限，但`delete`仅对管理员授予的权限︰

    azure mobile table create <service-name> <table-name> -p read=public,delete=admin

下表显示了在[Azure 管理门户]的权限值比较的脚本的权限值。

|编写脚本的值 |管理门户值 ||========|========|
|`public`|每个人都 ||`application`（默认值） |任何人使用的应用程序键 ||`user`|只有经过身份验证的用户 ||`admin`|仅脚本和管理员 |

`mobile table create`命令失败时指定的表已存在。 在自动化脚本，您应该尝试删除某个表，然后再尝试重新创建它。

##如何向现有的表列表中移动服务

以下命令返回的所有表的列表中移动的服务，在`<service-name>`的移动服务名称︰

    azure mobile table list <service-name>

此命令还显示每个表上的索引的数目和数据行数当前表中。

##如何从移动服务中删除一个现有表

下面的命令删除表从移动服务中，其中`<service-name>`是移动服务的名称和`<table-name>`是要删除的表的名称︰

    azure mobile table delete <service-name> <table-name> -q

自动化脚本中使用`-q`参数，并且不显示确认提示，阻止执行删除表。

##如何注册脚本保存到表操作

以下命令上载并注册到一个表中，操作的函数，`<service-name>`是移动服务的名称`<table-name>`的表中，名称和`<operation>`是表操作，它可以是`read`， `insert`， `update`，或`delete`:

    azure mobile script upload <service-name> table/<table-name>.<operation>.js

请注意，此操作将从本地计算机 (.js) JavaScript 文件上载。 文件的名称必须包含的表和操作名称，并且它必须位于`table`相对于执行了该命令的位置的子文件夹。 例如，以下操作会上载并注册一个新`insert`脚本，属于`TodoItems`表︰

    azure mobile script upload todolist table/todoitems.insert.js

脚本文件中的函数声明还必须匹配的已注册的表操作。 这意味着对于`insert`脚本，上载脚本包含一个函数具有下列签名︰

    function insert(item, user, request) {
        ...
    } 

有关注册脚本的详细信息，请参阅[移动服务服务器脚本引用]。

<!-- Anchors. -->
[下载并安装的命令行工具]: #install
[下载并导入发布设置]: #import
[创建新的移动服务]: #create-service
[获取主密钥]: #get-master-key
[创建新表]: #create-table
[注册一个新的表脚本]: #register-script
[删除一个现有表]: #delete-table
[删除现有的移动服务]: #delete-service
[测试移动服务]: #test-service
[列表中移动服务]: #list-services
[列表中的表]: #list-tables
[下一步行动]: #next-steps

<!-- Images. -->











<!-- URLs. -->
[移动服务服务器脚本引用]: http://go.microsoft.com/fwlink/p?LinkId=262293

[Azure 的管理门户]: https://manage.windowsazure.com/
[nodejs 组织]: http://nodejs.org/
[安装节点 linux]: https://github.com/joyent/node/wiki/Installing-Node.js-via-package-manager

[mac 安装程序]: http://go.microsoft.com/fwlink/p?LinkId=252249
[windows 安装程序]: http://go.microsoft.com/fwlink/p?LinkID=275464
[参考文档]: http://azure.microsoft.com/documentation/articles/virtual-machines-command-line-tools/#Commands_to_manage_mobile_services
[如何为 Mac 和 Linux 安装 Azure 命令行工具]: http://go.microsoft.com/fwlink/p/?LinkId=275795

 
测试
