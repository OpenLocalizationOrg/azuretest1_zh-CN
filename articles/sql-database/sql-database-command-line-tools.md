---
ms.openlocfilehash: 14b3ce0c8de670c6a11850c4231abe088b26b63c
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="管理与 PowerShell 的 SQL Azure 数据库资源" 
    description="SQL azure 数据库管理与 PowerShell。" 
    services="sql-database" 
    documentationCenter="" 
    authors="TigerMint" 
    manager="" 
    editor=""/>

<tags 
    ms.service="sql-database" 
    ms.workload="data-management" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/28/2015" 
    ms.author="vinsonyu"/>

# 管理与 PowerShell 的 SQL Azure 数据库资源


本主题提供了 PowerShell 命令来执行使用 Azure 资源管理器的 cmdlet 的许多 SQL Azure 数据库任务。


## 先决条件

要运行 PowerShell 的 cmdlet，您需要有 Azure PowerShell 安装并运行，并根据版本的不同，您可能需要将其切换到资源管理器模式来访问 Azure 资源管理器 PowerShell Cmdlet。 

您可以通过下载和安装的 Azure PowerShell 模块运行[Microsoft Web 平台安装程序](http://go.microsoft.com/fwlink/p/?linkid=320376&clcid=0x409)。 有关详细信息，请参阅[如何安装和配置 Azure PowerShell](../powershell-install-configure.md)。

用于创建和管理 Azure SQL 数据库的 cmdlet 都位于 Azure 资源管理器模块。 当您启动 Azure PowerShell 时，Azure 的模块中的 cmdlet 默认情况下导入。 若要切换到 Azure 资源管理器模块，请使用**交换机 AzureMode** cmdlet。

    Switch-AzureMode -Name AzureResourceManager

如果您收到一条警告说: 切换 AzureMode cmdlet 被否决，将来的版本中将删除 您可以忽略它并转至下一节。

有关详细信息，请参阅[使用 Windows PowerShell 与资源管理器](../powershell-azure-resource-manager.md)。



## 配置您的凭据

对 Azure 订阅运行 PowerShell cmdlet 您必须首先建立到 Azure 帐户的访问。 运行下面的命令，您将看到在屏幕符号以输入您的凭据。 使用相同的电子邮件和密码用于登录到 Azure 的门户。

    Add-AzureAccount

成功登录后您应该包括您登录的 Id 和您有权访问的 Azure 订阅的屏幕上看到的一些信息。


## 选择 Azure 订阅

若要选择您想要与您的订阅需要您订阅 Id (**-SubscriptionId**) 或订阅的名称 (**-SubscriptionName**)。 您可以将其从前面的步骤中，或如果您有多个订阅，则您可以运行**Get AzureSubscription** cmdlet 并从结果集中复制订阅所需的信息。

运行以下 cmdlet 订购信息来设置您的当前订购︰

    Select-AzureSubscription -SubscriptionId 4cac86b0-1e56-bbbb-aaaa-000000000000

以下命令将运行对订阅您刚刚选择上面。

## 创建资源组

创建将包含服务器的资源组。 您可以编辑下一个命令，可以使用任何有效的位置。 

为一份有效的 Azure SQL 数据库运行以下 cmdlet 服务器位置︰

    $AzureSQLLocations = Get-AzureLocation | Where-Object Name -Like "*SQL/Servers"
    $AzureSQLLocations.Locations

如果您已经有了可以跳一个资源组创建一个服务器，或您可以编辑并运行下面的命令来创建新的资源组︰

    New-AzureResourceGroup -Name "resourcegroupJapanWest" -Location "Japan West"

## 创建一个服务器 

若要创建新的 V12 服务器，请使用[New AzureSqlServer](https://msdn.microsoft.com/library/mt163526.aspx)命令。 替换为您的服务器的名称为 server12。 因此，则可能会出现以下错误的服务器名称已被使用，它必须是唯一的 Azure SQL 服务器。 此外值得注意的，此命令可能需要几分钟才能完成。 服务器已成功创建后，将显示服务器的详细信息并且 PowerShell 提示。 您可以编辑该命令以使用任何有效的位置。

    New-AzureSqlServer -ResourceGroupName "resourcegroupJapanWest" -ServerName "server12" -Location "Japan West" -ServerVersion "12.0"

当您运行此命令时打开一个窗口，要求输入**用户名**和**密码**。 这不是 Azure 凭据、 输入的用户名称和要创建的新服务器的管理员凭据的密码。

## 创建服务器防火墙规则

若要创建防火墙规则来访问该服务器，请使用[新建 AzureSqlServerFirewallRule](https://msdn.microsoft.com/library/mt125953.aspx)命令。 运行以下命令开始和结束 IP 地址替换为您的客户端有效的值。

如果您的服务器需要允许访问其他 Azure 服务，添加**-AllowAllAzureIPs**开关，将添加一个特殊的防火墙规则，允许所有 azure 通信访问服务器。

    New-AzureSqlServerFirewallRule -ResourceGroupName "resourcegroupJapanWest" -ServerName "server12" -FirewallRuleName "clientFirewallRule1" -StartIpAddress "192.168.0.198" -EndIpAddress "192.168.0.199"

有关详细信息，请参阅[Azure SQL 数据库防火墙](https://msdn.microsoft.com/library/azure/ee621782.aspx)。

## 创建 SQL 数据库

若要创建数据库，请使用[New AzureSqlDatabase](https://msdn.microsoft.com/library/mt125915.aspx)命令。 您需要创建一个数据库服务器。 下面的示例创建一个名为 TestDB12 的 SQL 数据库。 作为一个标准的 S1 数据库创建数据库。

    New-AzureSqlDatabase -ResourceGroupName "resourcegroupJapanWest" -ServerName "server12" -DatabaseName "TestDB12" -Edition Standard -RequestedServiceObjectiveName "S1"


## 更改 SQL 数据库的性能级别

您可以随数据库上下[集 AzureSqlDatabase](https://msdn.microsoft.com/library/mt125814.aspx)命令。 下面的示例扩展一个名为 TestDB12 从其当前性能级别标准 S3 级别的 SQL 数据库。

    Set-AzureSqlDatabase -ResourceGroupName "resourcegroupJapanWest" -ServerName "server12" -DatabaseName "TestDB12" -Edition Standard -RequestedServiceObjectiveName "S3"


## 删除 SQL 数据库

您可以删除[删除 AzureSqlDatabase](https://msdn.microsoft.com/library/mt125977.aspx)命令的 SQL 数据库。 下面的示例删除名为 TestDB12 的 SQL 数据库。

    Remove-AzureSqlDatabase -ResourceGroupName "resourcegroupJapanWest" -ServerName "server12" -DatabaseName "TestDB12"

## 删除服务器

您还可以删除[删除 AzureSqlServer](https://msdn.microsoft.com/library/mt125891.aspx)命令的服务器。 下面的示例删除名为 server12 的服务器。

    Remove-AzureSqlServer -ResourceGroupName "resourcegroupJapanWest" -ServerName "server12"



如果您将创建这些 SQL Azure 资源或相似的您可以︰ 

- 另存为 PowerShell 脚本文件 (*.ps1)
- 另存为 Azure 自动化 runbook Azure 管理门户的自动化部分中 

## 下一步行动

将命令合并和自动化。 例如，替换引号，包括一切 < 和 > 字符，您可以创建服务器、 防火墙规则和数据库的值︰


    New-AzureResourceGroup -Name "<resourceGroupName>" -Location "<Location>"
    New-AzureSqlServer -ResourceGroupName "<resourceGroupName>" -ServerName "<serverName>" -Location "<Location>" -ServerVersion "12.0"
    New-AzureSqlServerFirewallRule -ResourceGroupName "<resourceGroupName>" -ServerName "<serverName>" -FirewallRuleName "<firewallRuleName>" -StartIpAddress "<192.168.0.198>" -EndIpAddress "<192.168.0.199>"
    New-AzureSqlDatabase -ResourceGroupName "<resourceGroupName>" -ServerName "<serverName>" -DatabaseName "<databaseName>" -Edition <Standard> -RequestedServiceObjectiveName "<S1>"

## 相关的信息

- [SQL azure 数据库资源管理器的 Cmdlet](https://msdn.microsoft.com/library/mt163521.aspx)
- [SQL azure 数据库服务管理 Cmdlet](https://msdn.microsoft.com/library/dn546726.aspx)
 