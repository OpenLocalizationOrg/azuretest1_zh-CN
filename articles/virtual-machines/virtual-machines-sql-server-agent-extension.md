---
ms.openlocfilehash: d838c7f0e264872ca5f31fe3b55f0ed5b45d61fb
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="SQL Server IaaS 代理扩展" 
    description="描述 SQL Server 代理程序扩展，它允许在 Azure 使用自动化功能，以及如何安装代理，如果不已经自动安装在云中运行 SQL Server 的虚拟机。" 
    services="virtual-machines" 
    documentationCenter="" 
    authors="jeffgoll" 
    manager="jeffreyg"
    editor=""/>

<tags
    ms.service="virtual-machines"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services" 
    ms.date="06/17/2015"
    ms.author="jeffreyg"/>

# SQL Server IaaS 代理扩展

此扩展使 SQL Server 在 Azure 虚拟机使用此文章，只能使用与安装此扩展中列出的某些服务。 对于 SQL Server 在 Azure 预览门户网站中的库图像自动安装此扩展。 它可以安装在任何 SQL Server VM 在 Azure 的 Azure VM 来宾已安装了代理。
 
## 先决条件
使用 Powershell cmdlet 的要求︰

- 最新 Azure 命令行 SDK[可从以下站点](http://azure.microsoft.com/downloads/)

若要在您的虚拟机上使用扩展的要求︰

- Azure VM 来宾代理
- Windows Server 2012，Windows Server 2012 R2，或更高版本
- SQL Server 2012，SQL Server 2014，或更高版本
 
## 可用的扩展服务

- **SQL 自动的备份**︰ 此服务自动化的 VM 中的 SQL Server 默认实例的计划备份的所有数据库。 若要查看有关此服务的详细信息，请参阅[SQL Server 在 Azure 的虚拟机为自动化备份](virtual-machines-sql-server-automated-backup.md)。
- **SQL 自动修补程序**︰ 该服务允许您配置一个维护窗口期间更新的 VM 可以进行，这样可以避免在工作负载高峰期间更新。 若要查看有关此服务的详细信息，请参阅[自动修补为 SQL Server 在 Azure 的虚拟机](virtual-machines-sql-server-automated-patching.md)。

## 添加使用 Powershell 的扩展
如果您配置 SQL Server VM 使用[Azure 预览门户网站](https://portal.azure.com/)，将自动安装该扩展。 对于 SQL Server 虚拟机调配使用[Azure 管理门户](https://manage.windowsazure.com)，或使自己 SQL 许可的虚拟机，您可以添加此扩展现有的虚拟机，使用下面的 Azure PowerShell cmdlet。

**一组 AzureVMSqlServerExtension**

### 语法

一组 AzureVMSqlServerExtension [-虚拟机] <IPersistentVM> [[-版本] <string>] [-AutoBackupSettings <AutoBackupSettings>] [-AutoPatchingSetttings <AutoPatchingSetttings>] [-确认] [-WhatIf] [<CommonParameters>]

> [AZURE.NOTE] 省略参数推荐的 – 版本。 如果没有它，默认为最新版本的扩展。

### 示例
    Get-AzureVM –ServiceName serviceName –Name vmName | Set-AzureVMSqlServerExtension –AutoBackupSettings $abs | Update-AzureVM**

## 检查扩展的状态
如果您想要检查此扩展和与之相关的服务的状态，您可以使用任一门户。 在您现有的虚拟机的详细信息，发现在**设置**下的**扩展**。 

您还可以使用下面的 Azure Powershell cmdlet。

**获得 AzureVMSqlServerExtension**

### 语法

获得 AzureVMSqlServerExtension [[-虚拟机] <IPersistentVM>] [[-版本] <string>] [<CommonParameters>]

> [AZURE.NOTE] 可以省略-Version 参数。 如果没有它，默认为最新版本的扩展。

### 示例
    Get-AzureVM –ServiceName "service" –Name "vmname" | Get-AzureVMSqlServerExtension

## 移除与 Powershell 扩展   
如果您想从您的虚拟机中删除此扩展，您可以使用下面的 Azure Powershell cmdlet。

**删除 AzureVMSqlServerExtension**

### 语法
虚拟机删除 AzureVMSqlServerExtension- <IPersistentVM> [<CommonParameters>] 
