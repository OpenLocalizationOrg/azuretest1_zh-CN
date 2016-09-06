---
ms.openlocfilehash: 20d55b86ef98cb81a1ed93854396eb4a2342271c
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    title="Upgrade to the latest elastic database client library" 
    pageTitle="升级到最新的弹性数据库客户端库" 
    description="升级使用 PowerShell 和 C 的说明#" 
    metaKeywords="sharding,elastic scale, Azure SQL DB sharding" 
    services="sql-database" 
    documentationCenter="" 
    manager="jeffreyg" 
    authors="sidneyh"/>

<tags 
    ms.service="sql-database" 
    ms.workload="sql-database" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="05/17/2015" 
    ms.author="sidneyh" />

# 升级到最新的弹性数据库客户端库

通过[NuGet](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/)和 Visual Studio 中的 NuGetPackage 管理器界面弹性数据库客户端库的新版本不可用。 升级包含 bug 的修补程序和新功能的客户端库的支持。

## 升级步骤

升级时，必须先重新生成您的应用程序与新的库，以及更改现有 Shard 地图管理器元数据存储在 Azure SQL 数据库以支持新的功能。

遵循以下升级您的应用程序、 Shard 地图管理器数据库中，以及在每个 shard 上本地的 Shard 地图管理器元数据的规则。  按以下顺序执行升级步骤确保旧版本的客户端库不会再出现在您的环境中元数据对象被更新时，这意味着将不会在升级后创建的旧版本的元数据对象。   

**1.升级您的应用程序。** 在 Visual Studio，下载和引用最新的客户端库版本到所有开发项目使用库;然后重新生成和部署。 

 * 在 Visual Studio 解决方案中，选择**工具** --> **NuGet 程序包管理器** -->  **的 NuGet 程序包管理的解决方案**。 
 * 在左边的面板中，选择**更新**，，然后选择程序包**Azure SQL 数据库弹性扩展客户端库**在窗口中显示**更新**按钮。
    ![升级的 Nuget Pacakges][1]
 
 * 生成和部署。 

**2.升级您的脚本。** 如果您正在使用**PowerShell**脚本管理 shards，[下载最新的库版本](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/)，并将其复制到您执行脚本所在的目录。 

**3.将您拆分合并的服务升级。** 如果您使用弹性数据库拆分合并工具的合并来重新组织数据 sharded，[下载和部署该工具的最新版本](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Service.SplitMerge/)。 找不到该服务的详细的升级步骤[此处](sql-database-elastic-scale-overview-split-and-merge.md)。 

**4。 升级您 Shard 映射数据库管理器**。 升级在 SQL Azure 数据库中支持 Shard 映射的元数据。  有两种方法可以完成此操作，请使用 PowerShell 或 C#。 这两个选项如下所示。

***选项 1︰ 使用 PowerShell 升级元数据***

1. NuGet 从[这里](http://nuget.org/nuget.exe)下载最新版本的命令行实用程序并保存到某个文件夹。 

2. 打开命令提示窗口，定位到同一文件夹中，并发出命令︰
`nuget install Microsoft.Azure.SqlDatabase.ElasticScale.Client`

3. 导航到包含您只需下载，例如新的客户端 DLL 版本的子文件夹︰
`cd .\Microsoft.Azure.SqlDatabase.ElasticScale.Client.1.0.0\lib\net45`

4. 弹性的数据库客户端升级 scriptlet 从[脚本中心](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-Database-Elastic-6442e6a9)，并将其保存到同一文件夹中包含的 DLL。

5. 从该文件夹中，在命令提示符下运行"PowerShell.\upgrade.ps1"，按照提示进行操作。
 
***使用 C 选项 2︰ 升级元数据#***

另外，创建一个 Visual Studio 应用程序打开您 ShardMapManager，循环通过所有的 shards，并通过调用如本例所示的方法[UpgradeLocalStore](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.upgradelocalstore.aspx)和[UpgradeGlobalStore](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.upgradeglobalstore.aspx)执行元数据升级︰ 

    ShardMapManager smm =
       ShardMapManagerFactory.GetSqlShardMapManager
       (connStr, ShardMapManagerLoadPolicy.Lazy); 
    smm.UpgradeGlobalStore(); 
    
    foreach (ShardLocation loc in
     smm.GetDistinctShardLocations()) 
    {   
       smm.UpgradeLocalStore(loc); 
    } 

元数据升级这些技术可以多次应用而不会损坏。 例如，如果旧的客户端版本会无意中创建 shard 您已更新后，您可以再次运行升级跨所有 shards，以确保最新的元数据版本的整个基础架构存在。 

**注意︰** 客户端库的新版本发布至今继续使用先前版本的 Shard 地图管理器元数据在 Azure SQL 数据库，反之亦然。   但是，要利用的一些新的功能在最新的客户端中，元数据需要升级。   请注意，元数据升级不会影响任何用户数据或应用程序特定的数据，由 Shard 地图管理器创建和使用的唯一对象。  然后，应用程序继续运行通过上述升级序列。 

## 弹性的数据库客户端版本历史记录 

**版本 1.0-4 月 2015 2015年**

* 公开发行版本
* 对于日期时间类型为分片键添加的支持

**版 0.8--3 月 2015**

* 异步数据依赖于路由选择与新的 ShardMap.OpenConnectionForKeyAsync 方法添加支持。 
* 添加到 ShardMap 的公共 KeyType 属性 
* 添加支持 shards 数据库还原和灾难恢复方案的改进 

**版本 0.7--10 月 2014** 

初始预览版本 


[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]  


<!--Image references-->
[1]:./media/sql-database-elastic-scale-upgrade-client-library/nuget-upgrade.png
 