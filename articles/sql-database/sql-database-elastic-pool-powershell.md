---
ms.openlocfilehash: a46839b3c487cad74adb03ad21e016a5ef81847a
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="创建和管理使用 PowerShell 的 SQL 数据库弹性数据库池" 
   description="创建和管理使用 PowerShell 的 SQL Azure 数据库弹性数据库池" 
   services="sql-database" 
   documentationCenter="" 
   authors="stevestein" 
   manager="jeffreyg" 
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="powershell"
   ms.workload="data-management" 
   ms.date="08/25/2015"
   ms.author="adamkr; sstein"/>

# 创建和管理使用 PowerShell 的 SQL 数据库弹性数据库池

> [AZURE.SELECTOR]
- [Azure 门户](sql-database-elastic-pool-portal.md)
- [C#](sql-database-client-library.md)
- [PowerShell](sql-database-elastic-pool-powershell.md)


## 概述

这篇文章演示了如何创建和管理使用 PowerShell 的 SQL 数据库弹性数据库池。


使用 Azure PowerShell 创建弹性数据库池的各个步骤是分解，为澄清说明。 那些人只是想简单的命令列表，请参阅本文底部的**放在一起**的部分。

这篇文章将说明如何创建您创建并配置弹性数据库池除 Azure 订阅所需要的一切。 如果您需要订阅了 Azure 只需单击该页顶部的**免费试用版**，然后回来完成这篇文章。

> [AZURE.NOTE] 弹性的数据库池目前在预览中，并且只可与 SQL 数据库 V12 服务器。


## 先决条件

要使用 PowerShell 创建弹性数据库池，您需要具有 Azure PowerShell 安装并运行，并将其切换到资源管理器模式来访问 Azure 资源管理器 PowerShell Cmdlet。 

您可以通过下载和安装的 Azure PowerShell 模块运行[Microsoft Web 平台安装程序](http://go.microsoft.com/fwlink/p/?linkid=320376&clcid=0x409)。 有关详细信息，请参阅[如何安装和配置 Azure PowerShell](powershell-install-configure.md)。

用于创建和管理 Azure SQL 数据库和弹性数据库池的 cmdlet 都位于 Azure 资源管理器模块。 当您启动 Azure PowerShell 时，Azure 的模块中的 cmdlet 默认情况下导入。 若要切换到 Azure 资源管理器模块，请使用交换机 AzureMode cmdlet。

    Switch-AzureMode -Name AzureResourceManager

有关详细信息，请参阅[使用 Windows PowerShell 与资源管理器](powershell-azure-resource-manager.md)。


## 配置您的凭据，并选择您的订购

现在，您运行的 Azure 资源管理器模块可以访问所有必要的 cmdlet 来创建和配置一个弹性的数据库池。 首先必须建立到 Azure 帐户访问权限。 运行下面的命令，您将看到在屏幕符号以输入您的凭据。 使用相同的电子邮件和密码用于登录到 Azure 的门户。

    Add-AzureAccount

成功登录后您应该包括您登录的 Id 和您有权访问的 Azure 订阅的屏幕上看到的一些信息。


### 选择 Azure 订阅

选择订阅，您需要订阅 Id 或订阅名称 (**-SubscriptionName**)。 您可以将其从前面的步骤中，或如果您有多个订阅，则您可以运行**Get AzureSubscription** cmdlet 并从结果集中复制订阅所需的信息。 一旦您有您运行以下 cmdlet 的订阅︰

    Select-AzureSubscription -SubscriptionId 4cac86b0-1e56-bbbb-aaaa-000000000000


## 创建资源组、 服务器和防火墙规则

现在，您可以访问要对其运行的 cmdlet Azure 订阅以便下一步建立包含服务器的资源组创建弹性数据库池的位置。 您可以编辑下一个命令可以使用任何有效的位置选择。 运行**(Get AzureLocation | 位置对象 {$_。名称-eq"Microsoft.Sql/servers"}）。位置**以获取有效的位置的列表。

如果您已经有一个资源组，您可以转到下一步，或者您可以运行以下命令来创建新的资源组︰

    New-AzureResourceGroup -Name "resourcegroup1" -Location "West US"

### 创建一个服务器 

在 SQL Azure 数据库服务器内创建弹性数据库池。 如果您已经有一个服务器，您可以转到下一步，或者您可以运行以下命令来创建一个新的 V12 服务器。 替换为您的服务器的名称服务器的名称。 因此，则可能会出现以下错误的服务器名称已被使用，它必须是唯一的 Azure SQL 服务器。 此外值得注意的，此命令可能需要几分钟才能完成。 服务器已成功创建后，将显示服务器的详细信息并且 PowerShell 提示。 您可以编辑该命令以使用任何有效的位置选择。

    New-AzureSqlServer -ResourceGroupName "resourcegroup1" -ServerName "server1" -Location "West US" -ServerVersion "12.0"

当您运行此命令时打开一个窗口，要求输入**用户名**和**密码**。 这不是 Azure 凭据、 输入的用户名称和要创建的新服务器的管理员凭据的密码。  


### 配置服务器防火墙规则以允许对服务器的访问

建立防火墙规则来访问该服务器。 运行以下命令开始和结束 IP 地址替换为您的计算机的有效值。

如果您的服务器需要允许访问其他 Azure 服务，添加**-AllowAllAzureIPs**开关，将添加一个特殊的防火墙规则，允许所有 azure 通信访问服务器。

    New-AzureSqlServerFirewallRule -ResourceGroupName "resourcegroup1" -ServerName "server1" -FirewallRuleName "rule1" -StartIpAddress "192.168.0.198" -EndIpAddress "192.168.0.199"

有关详细信息，请参阅[Azure SQL 数据库防火墙](https://msdn.microsoft.com/library/azure/ee621782.aspx)。


## 创建一个弹性的数据库池和弹性数据库

现在您有一个资源组、 服务器和防火墙规则配置，因此您可以访问该服务器。 以下命令将创建弹性数据库池。 此命令创建了一个池，共享共 400 eDTUs。 池中的每个数据库保证始终有可用 10 eDTUs (DatabaseDtuMin)。 池中的每个数据库可以消耗 100 eDTUs (DatabaseDtuMax) 的最多。 有关详细的参数的说明，请参阅[SQL Azure 数据库弹性池](sql-database-elastic-pool.md)。 


    New-AzureSqlElasticPool -ResourceGroupName "resourcegroup1" -ServerName "server1" -ElasticPoolName "elasticpool1" -Edition "Standard" -Dtu 400 -DatabaseDtuMin 10 -DatabaseDtuMax 100


### 创建或添加到一个弹性的数据库池的弹性数据库

在上一步中创建的池为空，它具有无弹性的数据库。 以下部分显示了如何创建新的弹性数据库，在池，以及如何将现有的数据库添加到池中。

*在创建池之后还可以在池中，创建新的弹性数据库和一个池或移动现有的数据库使用事务处理 SQL。 有关详细信息，请参见[弹性数据库池引用的处理 SQL](sql-database-elastic-pool-reference.md#Transact-SQL)。*

### 创建新的弹性数据库内弹性数据库池

若要创建新的数据库，直接在池内，使用**New AzureSqlDatabase** cmdlet 并将**ElasticPoolName**参数设置。


    New-AzureSqlDatabase -ResourceGroupName "resourcegroup1" -ServerName "server1" -DatabaseName "database1" -ElasticPoolName "elasticpool1"



### 将现有数据库移到弹性数据库池

若要将现有的数据库移动到池，使用**一组 AzureSqlDatabase** cmdlet，将**ElasticPoolName**参数设置。 


演示中，来创建数据库未处于弹性数据库池。

    New-AzureSqlDatabase -ResourceGroupName "resourcegroup1" -ServerName "server1" -DatabaseName "database1" -Edition "Standard"

将现有的数据库移动到弹性数据库池。

    Set-AzureSqlDatabase -ResourceGroupName "resourcegroup1" -ServerName "server1" -DatabaseName "database1" -ElasticPoolName "elasticpool1"

## 更改的弹性数据库池的性能设置


    Set-AzureSqlElasticPool –ResourceGroupName “resourcegroup1” –ServerName “server1” –ElasticPoolName “elasticpool1” –Dtu 1200 –DatabaseDtuMax 100 –DatabaseDtuMin 50 


## 监视数据库的弹性和弹性数据库池

### 获得弹性数据库池操作的状态

您可以跟踪弹性数据库池操作包括创建和更新的状态。 

    Get-AzureSqlElasticPoolActivity –ResourceGroupName “resourcegroup1” –ServerName “server1” –ElasticPoolName “elasticpool1” 


### 获得弹性的数据库移入和移出弹性数据库池的状态

    Get-AzureSqlElasticPoolDatabaseActivity -ResourceGroupName "resourcegroup1" -ServerName "server1" -DatabaseName "database1" -ElasticPoolName "elasticpool1"

### 资源消耗指标获得弹性数据库池

测量数据，可检索的资源池限制百分比︰   

* 平均 CPU 利用率-cpu_percent 
* 平均的 IO 利用率-data_io_percent 
* 平均的日志利用率-log_write_percent 
* 平均内存利用率-memory_percent 
* 平均 eDTU 利用率 （如日志/IO/CPU 使用率的最大值）--DTU_percent 
* 最大并发用户请求 （工作人员） – max_concurrent_requests 数 
* 并发用户会话 — max_concurrent_sessions 的最大数量 
* 弹性池 – storage_in_megabytes 的总存储大小 


度量粒度/保留期︰

* 在 5 分钟的粒度将返回数据。  
* 数据保留期为 14 天。  


此 cmdlet 和 API 限制可以 1000年行 （大约在 5 分钟的粒度数据的 3 天） 的一次调用中检索的行数。 但可以使用不同的开始/结束时间间隔，以检索更多数据多次调用此命令 


检索的度量标准︰

    $metrics = (Get-Metrics -ResourceId /subscriptions/d7c1d29a-ad13-4033-877e-8cc11d27ebfd/resourceGroups/FabrikamData01/providers/Microsoft.Sql/servers/fabrikamsqldb02/elasticPools/franchisepool -TimeGrain ([TimeSpan]::FromMinutes(5)) -StartTime "4/18/2015" -EndTime "4/21/2015") 

通过重复调用并为其追加数据获取额外的天数︰

    $metrics = $metrics + (Get-Metrics -ResourceId /subscriptions/d7c1d29a-ad13-4033-877e-8cc11d27ebfd/resourceGroups/FabrikamData01/providers/Microsoft.Sql/servers/fabrikamsqldb02/elasticPools/franchisepool -TimeGrain ([TimeSpan]::FromMinutes(5)) -StartTime "4/21/2015" -EndTime "4/24/2015") 
 
设置表的格式︰

    $table = Format-MetricsAsTable $metrics 

导出到 CSV 文件︰

    foreach($e in $table) { Export-csv -Path c:\temp\metrics.csv -input $e -Append -NoTypeInformation} 

### 资源消耗指标获得弹性数据库

这些 Api 是当前 (V12) Api 用于监视资源使用情况的独立数据库，以下语义两者之间的区别与相同 

* 为此 API 检索的度量标准以百分比表示的每个 databaseDtuMax （或等效帽，如 CPU、 IO 等基础指标的） 设置为该弹性数据库池。 例如，任何这些度量标准的 50%利用率表示的特定资源占用位于 50%的数 DB 帽限制。 在父弹性数据库池的资源。 

获取标准︰ $metrics = (Get 度量资源 Id /subscriptions/d7c1d29a-ad13-4033-877e-8cc11d27ebfd/resourceGroups/FabrikamData01/providers/Microsoft.Sql/servers/fabrikamsqldb02/databases/myDB-TimeGrain ([时间跨度]:: FromMinutes(5)) 开始时间"18/4/2015"-结束时间"21/4/2015") 

如果需要通过重复调用并为其追加数据，获取额外的天数︰

    $metrics = $metrics + (Get-Metrics -ResourceId /subscriptions/d7c1d29a-ad13-4033-877e-8cc11d27ebfd/resourceGroups/FabrikamData01/providers/Microsoft.Sql/servers/fabrikamsqldb02/databases/myDB -TimeGrain ([TimeSpan]::FromMinutes(5)) -StartTime "4/21/2015" -EndTime "4/24/2015") 

设置表的格式︰

    $table = Format-MetricsAsTable $metrics 

导出到 CSV 文件︰

    foreach($e in $table) { Export-csv -Path c:\temp\metrics.csv -input $e -Append -NoTypeInformation}


## 放在一起


    Switch-AzureMode -Name AzureResourceManager
    Add-AzureAccount
    Select-AzureSubscription -SubscriptionId 4cac86b0-1e56-bbbb-aaaa-000000000000
    New-AzureResourceGroup -Name "resourcegroup1" -Location "West US"
    New-AzureSqlServer -ResourceGroupName "resourcegroup1" -ServerName "server1" -Location "West US" -ServerVersion "12.0"
    New-AzureSqlServerFirewallRule -ResourceGroupName "resourcegroup1" -ServerName "server1" -FirewallRuleName "rule1" -StartIpAddress "192.168.0.198" -EndIpAddress "192.168.0.199"
    New-AzureSqlElasticPool -ResourceGroupName "resourcegroup1" -ServerName "server1" -ElasticPoolName "elasticpool1" -Edition "Standard" -Dtu 400 -DatabaseDtuMin 10 -DatabaseDtuMax 100
    New-AzureSqlDatabase -ResourceGroupName "resourcegroup1" -ServerName "server1" -DatabaseName "database1" -ElasticPoolName "elasticpool1" -MaxSizeBytes 10GB
    Set-AzureSqlElasticPool –ResourceGroupName “resourcegroup1” –ServerName “server1” –ElasticPoolName “elasticpool1” –Dtu 1200 –DatabaseDtuMax 100 –DatabaseDtuMin 50 
    
    $metrics = (Get-Metrics -ResourceId /subscriptions/d7c1d29a-ad13-4033-877e-8cc11d27ebfd/resourceGroups/FabrikamData01/providers/Microsoft.Sql/servers/fabrikamsqldb02/elasticPools/franchisepool -TimeGrain ([TimeSpan]::FromMinutes(5)) -StartTime "4/18/2015" -EndTime "4/21/2015") 
    $metrics = $metrics + (Get-Metrics -ResourceId /subscriptions/d7c1d29a-ad13-4033-877e-8cc11d27ebfd/resourceGroups/FabrikamData01/providers/Microsoft.Sql/servers/fabrikamsqldb02/elasticPools/franchisepool -TimeGrain ([TimeSpan]::FromMinutes(5)) -StartTime "4/21/2015" -EndTime "4/24/2015")
    $table = Format-MetricsAsTable $metrics
    foreach($e in $table) { Export-csv -Path c:\temp\metrics.csv -input $e -Append -NoTypeInformation}

    $metrics = (Get-Metrics -ResourceId /subscriptions/d7c1d29a-ad13-4033-877e-8cc11d27ebfd/resourceGroups/FabrikamData01/providers/Microsoft.Sql/servers/fabrikamsqldb02/databases/myDB -TimeGrain ([TimeSpan]::FromMinutes(5)) -StartTime "4/18/2015" -EndTime "4/21/2015") 
    $metrics = $metrics + (Get-Metrics -ResourceId /subscriptions/d7c1d29a-ad13-4033-877e-8cc11d27ebfd/resourceGroups/FabrikamData01/providers/Microsoft.Sql/servers/fabrikamsqldb02/databases/myDB -TimeGrain ([TimeSpan]::FromMinutes(5)) -StartTime "4/21/2015" -EndTime "4/24/2015")
    $table = Format-MetricsAsTable $metrics
    foreach($e in $table) { Export-csv -Path c:\temp\metrics.csv -input $e -Append -NoTypeInformation}

## 下一步行动

创建后弹性数据库池，您可以通过创建弹性作业管理池中的弹性数据库。 弹性作业促进针对任意数量的数据库在池中运行 T SQL 脚本。 有关详细信息，请参阅[弹性数据库作业概述](sql-database-elastic-jobs-overview.md)。


## 弹性数据库引用

更多有关弹性数据库及弹性数据库池，包括 API 和错误的详细信息，请参阅[弹性数据库池引用](sql-database-elastic-pool-reference.md)。