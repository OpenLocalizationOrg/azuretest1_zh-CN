---
ms.openlocfilehash: b0c4731c5d079e71de13bed622cd53890be38b74
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---

<properties
        pageTitle="使用 Linux 诊断扩展监视 Linux 虚拟机的性能和诊断数据 |Microsoft Azure"
        description="了解如何使用 Linux 诊断扩展监视 Linux 虚拟机的性能和诊断数据。"
        services="virtual-machines"
        documentationCenter=""
    authors="NingKuang"
        manager="timlt"
        editor=""
    tags=""/>

<tags
        ms.service="virtual-machines"
        ms.workload="infrastructure-services"
        ms.tgt_pltfrm="vm-linux"
        ms.devlang="na"
        ms.topic="article"
        ms.date="07/20/2015"
        ms.author="Ning"/>


# 使用 Linux 诊断扩展监视 Linux 虚拟机的性能和诊断数据

## 简介

Linux 诊断扩展名可帮助用户监视 Linux 虚拟机在 Microsoft Azure 上运行具有以下功能︰

- 收集并将 Linux 虚拟机的系统性能、 诊断和系统日志数据上载到用户的存储表。
- 使用户可自定义的数据标准将被收集并上载。
- 使用户能够指定的日志文件上传到指定的存储表。

2.0 版中，有关的数据包括︰

- 所有的 Linux Rsyslog loggings，包括系统、 安全性和应用程序日志。
- 所有在本[文档](https://scx.codeplex.com/wikipage?title=xplatproviders")中指定的系统数据。
- 用户指定的日志文件。

## 如何启用扩展
可以通过[Azure 的门户](https://ms.portal.azure.com/#)，Azure PowerShell 或 Azure CLI 脚本启用扩展。

查看和配置系统和性能数据直接从 Azure 的门户，请按照下列[步骤](http://azure.microsoft.com/blog/2014/09/02/windows-azure-virtual-machine-monitoring-with-wad-extension/ "向 Windows 网络日志的 URL")。


本文重点介绍启用和配置通过 Azure CLI 命令的扩展。这使您可以阅读并直接查看存储表中的数据。


## 先决条件
- Microsoft Azure Linux 代理版本 2.0.6 或更高版本。
请注意，大多数 Azure VM Linux 库图像包含版本 2.0.6 或更高版本。 您可以运行**WAAgent-版本**确认在虚拟机中安装的版本。 如果虚拟机正在运行的版本早于 2.0.6 您可以按照这些[指导](https://github.com/Azure/WALinuxAgent "说明")要对其进行更新。
- [Azure CLI](./xplat-cli.md)。 按照[本指南](./xplat-cli-install.md)来设置您的计算机上的 Azure CLI 环境。 Azure CLI 安装后，可以从您的命令行界面 （大扫除，终端，通过命令提示符） 使用**azure**命令以访问 Azure CLI 命令。 例如，运行**azure vm 扩展集--帮助**运行**azure 登录**来登录到 Azure 中，运行**azure 虚拟机列表**列出可以在 Azure 的所有虚拟机的详细用法。
- 要存储数据的存储帐户。 您将需要以前创建的存储帐户名称和访问键来将数据上载到您的存储。


## 使用 Azure CLI 命令启用 Linux 诊断扩展

###  方案 1。 启用默认数据集扩展
2.0 或更高版本中，默认将收集的数据包括︰

- 所有 Rsyslog 信息 （包括系统、 安全性和应用程序日志）。  
- 核心基础系统数据，注意完整的数据集是本[文档](https://scx.codeplex.com/wikipage?title=xplatproviders)中所述。
如果您想要启用额外的数据，继续进行方案 2 和 3 中的步骤。

第 1 步。 创建一个名为 PrivateConfig.json，使用以下内容文件。

    {
        "storageAccountName":"the storage account to receive data",
        "storageAccountKey":"the key of the account"
    }

第 2 步。 运行**azure vm 扩展设置 vm 名称 LinuxDiagnostic Microsoft.OSTCExtensions 2。* — 专用配置路径 PrivateConfig.json**。


###   方案 2。 自定义性能监视指标  
本部分介绍如何自定义性能和诊断的数据表格。

第 1 步。 创建名为 PrivateConfig.json，在接下来的示例中显示的内容。 指定要收集的特定数据。

有关所有受支持的提供程序和变量，引用此[文档](https://scx.codeplex.com/wikipage?title=xplatproviders)。 可以有多个查询，并将它们存储在多个表，通过向脚本追加更多的查询。

默认情况下，总是收集 Rsyslog 数据。

    {
        "storageAccountName":"storage account to receive data",
        "storageAccountKey":"key of the account",
        "perfCfg":[
            {"query":"SELECT PercentAvailableMemory, AvailableMemory, UsedMemory ,PercentUsedSwap FROM SCX_MemoryStatisticalInformation","table":"LinuxMemory"
            }
          ]
    }


第 2 步。 运行**azure vm 扩展设置 vm 名称 LinuxDiagnostic Microsoft.OSTCExtensions 2。* — 专用配置路径 PrivateConfig.json**。


###   方案 3。 日志文件上传
本部分介绍如何收集和特定日志文件上传到您的存储帐户。
您需要指定您的日志文件，路径和指定的表名来存储您的日志。 通过向脚本中添加多个文件/目录项可以有多个日志文件。

第 1 步。 创建一个名为 PrivateConfig.json，使用以下内容文件。

    {
        "storageAccountName":"the storage account to receive data",
        "storageAccountKey":"key of the account",
        "fileCfg":[
            {"file":"/var/log/mysql.err",
             "table":"mysqlerr"
            }
          ]
    }


第 2 步。 运行**azure vm 扩展设置 vm 名称 LinuxDiagnostic Microsoft.OSTCExtensions 2。* — 专用配置路径 PrivateConfig.json**。


###   方案 4。 禁用 Linux 监视器扩展
第 1 步。 创建一个名为 PrivateConfig.json，使用以下内容文件。

    {
        "storageAccountName":"the storage account to receive data",
        "storageAccountKey":"the key of the account",
        “perfCfg”:[],
        “enableSyslog”:”False”
    }


第 2 步。 运行**azure vm 扩展设置 vm 名称 LinuxDiagnostic Microsoft.OSTCExtensions 2。* — 专用配置路径 PrivateConfig.json**。


## 查看您的数据
在 Azure 存储表中存储的性能和诊断数据。 请查看[本文](storage-ruby-how-to-use-table-storage.md)以了解如何访问使用 Azure CLI 脚本存储表中的数据。

此外，您可以使用下列工具用户界面访问数据︰

1.  使用 Visual Studio 的服务器资源管理器。 导航到您的存储帐户。 虚拟机运行大约 5 分钟后，您应该看到四种默认表:"LinuxCpu"、"LinuxDisk"、"LinuxMemory"和"Linuxsyslog"。 双击要查看的数据的表名。
2.  使用[Azure 存储浏览器](https://azurestorageexplorer.codeplex.com/ "Azure 存储浏览器")来访问数据。

![image](./media/virtual-machines-linux-diagnostic-extension/no1.png)

如果已启用的 fileCfg 或 perfCfg 指定在方案 2 和 3 中，您可以使用以前的工具查看非默认数据。



## 已知的问题
- 版本 2.0，只能通过脚本访问 Rsyslog 信息和客户指定的日志文件。
- 为 2.0 版中，如果您启用了 Linux 诊断扩展通过脚本中的第一次，然后您不能查看的数据从 Azure 的门户。 如果先启用从门户扩展，然后脚本仍然有效。
