---
ms.openlocfilehash: 93a43e0d089876775eb4b79b8a979fd2c4fc202e
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="SQL 数据仓库中的 cmdlet 入门"
   description="暂停和重新启动 SQL 数据仓库使用 PowerShell cmdlet"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sidneyh"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/28/2015"
   ms.author="twounder;sidneyh;barbkess"/>

# Azure 数据仓库 cmdlet 和 REST Api 入门

可以使用 Azure PowerShell cmdlet 或 REST Api 来管理 SQL 数据仓库。 

为**SQL Azure 数据库**定义的命令还用于**SQL 数据仓库**。 当前列表，请参阅[SQL Azure 的 Cmdlet](https://msdn.microsoft.com/library/azure/dn546726.aspx)。 这些 cmdlet**挂起 AzureSqlDatabase**和**恢复 AzureSqlDatabase** （下图） 是为 SQL 数据仓库设计的附件。

同样，REST Api，可用于**SQL Azure 数据库**还可以用于**SQL 数据仓库**实例。 当前列表，请参阅[Azure SQL 数据库的操作](https://msdn.microsoft.com/library/azure/dn505719.aspx)。

## 获取并运行 Azure PowerShell cmdlet

1. 若要下载 Azure PowerShell 模块，运行[Microsoft Web 平台安装程序](http://go.microsoft.com/fwlink/p/?linkid=320376&clcid=0x409)。 
2. 要运行的模块，请在启动窗口键入**Microsoft Azure PowerShell**。
3. 如果您尚未您的帐户添加到计算机，请运行以下 cmdlet。 (有关详细信息，请参阅[如何安装和配置 Azure PowerShell]():

        Add-AzureAccount
3. 与此 cmdlet 将模式切换︰

        Switch-AzureMode AzureResourceManager

## 挂起 AzureSqlDatabase
### 示例 1︰ 暂停数据库服务器上的名称

本示例暂停一个名为"Database02"，在一个名为"Server01"。 服务器上承载数据库 服务器是在 Azure 的资源组名为"ResourceGroup1"。

    Suspend-AzureSqlDatabase –ResourceGroupName "ResourceGroup11" –ServerName
    "Server01" –DatabaseName "Database02"

### 示例 2︰ 暂停数据库对象

本示例检索一个名为"Database02"从服务器名为"Server01"包含在资源组名为"ResourceGroup1"。 该管道**挂起 AzureSqlDatabase**检索到的对象。 因此，数据库已暂停。

    $database = Get-AzureSqlDatabase –ResourceGroupName "ResourceGroup11" –ServerName "Server01" –DatabaseName "Database02"
    $resultDatabase = $database | Suspend-AzureSqlDatabase

## 恢复-AzureSqlDatabase

### 示例 1︰ 按名称服务器上恢复数据库

本示例恢复名为"Database02"，在一个名为"Server01"。 服务器上承载的数据库的操作 服务器包含在资源组名为"ResourceGroup1"。

    Resume-AzureSqlDatabase –ResourceGroupName "ResourceGroup11" –ServerName "Server01" –DatabaseName "Database02"

### 示例 2︰ 恢复数据库对象

本示例检索一个名为"Database02"从服务器名为"Server01"，包含在资源组名为"ResourceGroup1"。 该对象被输送到**恢复 AzureSqlDatabase**。 

    $database = Get-AzureSqlDatabase –ResourceGroupName "ResourceGroup11" –ServerName "Server01" –DatabaseName "Database02"
    $resultDatabase = $database | Resume-AzureSqlDatabase

## 获得 AzureSqlDatabaseRestorePoints

此 cmdlet 列出 SQL Azure 数据库的备份还原点。 使用还原点还原数据库。
返回的对象的属性，如下所示。

属性|说明
---|---
RestorePointType|独立/连续。 离散的还原点描述可能点-中-的时间，可以将 SQL Azure 数据库还原到。 连续还原点描述最早可能点-中-时间的 SQL Azure 数据库可以恢复到。 可以恢复到任意时间点之后最早的数据库。
EarliestRestoreDate|最早恢复时间 (填充时 restorePointType = 连续)
RestorePointCreationDate |备份快照时间 (填充时 restorePointType = 离散)

### 示例 1︰ 在服务器上的名称来检索数据库的还原点
本示例检索名为"Database02"的数据库的还原点从服务器名为"Server01，"包含在资源组名为"ResourceGroup1"。

    $restorePoints = Get-AzureSqlDatabaseRestorePoints –ResourceGroupName "ResourceGroup11" –ServerName "Server01" –DatabaseName "Database02"



### 示例 2︰ 恢复数据库对象

本示例检索名为"Database02"的数据库从一个服务器名为"Server01，"包含在资源组名为"ResourceGroup1"。 数据库对象被输送到**获得 AzureSqlDatabase**，和结果是数据库的还原点。

    $database = Get-AzureSqlDatabase –ResourceGroupName "ResourceGroup11" –ServerName "Server01" –DatabaseName "Database02"
    $restorePoints = $database | Get-AzureSqlDatabaseRestorePoints



> [AZURE.NOTE] 请注意，如果您的服务器为 foo.database.windows.net，使用"foo"为 powershell cmdlet 中的服务器名称。


## 下一步行动
引用的详细信息，请参阅[SQL 数据仓库引用概述][]。

<!--Image references-->

<!--Article references-->
[SQL 数据仓库引用概述]: sql-data-warehouse-overview-reference.md
[如何安装和配置 Azure PowerShell]: powershell-install-configure.md

<!--MSDN references-->


<!--Other Web references-->
[gog]: http://google.com/
[yah]: http://search.yahoo.com/
[msn]: http://search.msn.com/
