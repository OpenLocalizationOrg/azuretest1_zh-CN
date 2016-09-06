---
ms.openlocfilehash: d00ecd2ac2c23219203cecdff44e3746e70b0fbd
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="数据依赖于路由" 
    description="如何使用 ShardMapManager 的数据依赖于路由，弹性 Azure SQL 数据库的数据库功能" 
    services="sql-database" 
    documentationCenter="" 
    manager="jeffreyg" 
    authors="sidneyh" 
    editor=""/>

<tags 
    ms.service="sql-database" 
    ms.workload="sql-database" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/24/2015" 
    ms.author="sidneyh"/>

#数据依赖于路由

**ShardMapManager**类提供 ADO.NET 应用程序能够轻松地在 sharded 环境中直接数据库查询和到相应的物理数据库的命令。 这称为**数据相关路由**，使用 sharded 的数据库是一种基本模式。 每个特定查询或在应用程序中使用数据依赖于路由选择的交易记录将被限制访问单个数据库的每个请求。  

使用依赖于数据的路由，则不需要应用程序来跟踪不同的连接字符串或不同切片 sharded 环境中的数据相关联的数据库位置。 相反， [Shard 映射经理](sql-database-elastic-scale-shard-map-management.md)还要负责打开连接到正确的数据库可以分发时所需的、 基于 shard 地图和目标应用程序的请求的分片键的值中的数据。 （此项通常是*customer_id*、 *tenant_id*、 *date_key*或为基本参数的数据库请求的某些其他特定的标识符）。 

## 使用 ShardMapManager 数据依赖于路由应用程序中 

使用的应用程序依赖于数据的传送， **ShardMapManager**应被实例化，一旦每个应用程序域初始化，期间使用工厂拨打**GetSQLShardMapManager**。

    ShardMapManager smm = ShardMapManagerFactory.GetSqlShardMapManager(smmConnnectionString, 
                      ShardMapManagerLoadPolicy.Lazy);
    RangeShardMap<int> customerShardMap = smm.GetRangeShardMap<int>("customerMap"); 

在此示例中， **ShardMapManager** ，它包含特定**ShardMap**初始化。 

不操纵 shard 地图本身的应用程序，使用中的工厂方法来获取**ShardMapManager** （在上面的示例中， *smmConnectionString*） 的凭据应该只是只读权限对连接字符串所引用的**全局 Shard 地图**数据库的凭据。 这些凭据是通常不同于用来打开到 shard 地图管理器的连接凭据。 请参阅[中的弹性数据库客户端库使用凭据](sql-database-elastic-scale-manage-credentials.md)。 

## 调用数据依赖于路由 

方法**ShardMap.OpenConnectionForKey 密钥、 连接字符串 (connectionOptions）**返回 ADO.Net 连接到基于**key**参数的值的适当数据库发出命令的准备。 Shard 信息被缓存在应用程序**ShardMapManager**，因此这些请求通常不涉及数据库查找针对**全局 Shard 映射**数据库。 

* **Key**参数用作查找键到 shard 映射来确定适当的数据库请求。 

* **连接字符串**用于传递只有所需的连接的用户凭据。 没有数据库名称或服务器名称都包括在此*连接字符串*，因为该方法将确定的数据库和服务器使用**ShardMap**。 

* **ConnectionOptions**枚举用于指示是否验证发生的时间或不提供打开的连接时。 **ConnectionOptions.Validate**建议。 在环境中可能会更改 shard 映射行可能移动到其它数据库拆分或合并操作的结果，验证可以确保数据库中基于关键字值缓存的查询仍然正确。 验证涉及到目标系统上的本地 shard 映射一个简短查询之前该连接传送到应用程序数据库 （不到全球的 shard 映射）。 

如果对本地 shard 映射验证失败 （指示缓存不正确），Shard 地图管理器将查询全局 shard 地图获得查找正确的新值，更新缓存，并获取并返回相应的数据库连接。 

只有在这个时候， **ConnectionOptions.None** （未验证） 是可以接受的在线应用程序时不需要进行 shard 映射更改时发生。 在这种情况下，可假定的缓存的值始终是正确的并可以安全地跳到目标数据库的额外往返验证调用。 这可能会减少事务延迟和数据库通信。 **ConnectionOptions**也可以设置通过一个值以指示是否需要进行分片更改配置文件中或不在一段时间。  

这是代码的一个 Shard 地图管理器用于执行数据相关路由基于整数键**客户 id**，使用名为**customerShardMap**的**ShardMap**对象的值的示例。  

## 示例︰ 数据依赖于路由 

    int customerId = 12345; 
    int newPersonId = 4321; 

    // Connection to the shard for that customer ID
    using (SqlConnection conn = customerShardMap.OpenConnectionForKey(customerId, 
        Configuration.GetCredentialsConnectionString(), ConnectionOptions.Validate)) 
    { 
        // Execute a simple command 
        SqlCommand cmd = conn.CreateCommand(); 
        cmd.CommandText = @"UPDATE Sales.Customer 
                            SET PersonID = @newPersonID 
                            WHERE CustomerID = @customerID"; 

        cmd.Parameters.AddWithValue("@customerID", customerId); 
        cmd.Parameters.AddWithValue("@newPersonID", newPersonId); 
        cmd.ExecuteNonQuery(); 
    }  

使用跟该连接对象， **OpenConnectionForKey**方法对**open （）**调用，而不能为**SqlConnection**，使用构造函数的声明，并为正确的数据库提供了一个新的已打开连接。 利用这种方式连接仍然充分利用 ADO.Net 连接池。 只要交易记录和请求可以满足一个 shard 的一次，这应该是已经在使用 ADO.Net 应用程序中所需的唯一修改。 

如果您的应用程序利用异步使用 ADO.Net 编程方法**OpenConnectionForKeyAsync**还有。  其行为相当依赖于数据的路由的 ADO。Net 的**Connection.OpenAsync**方法。

## 瞬时故障处理与集成在一起 

在开发数据访问应用程序在云中的最佳做法是确保应用程序中，通过捕获了连接到或查询数据库中的瞬时故障和操作被试在引发错误之前的几次。 在[瞬时性故障处理](http://msdn.microsoft.com/en-us/library/dn440719\(v=pandp.60\).aspx)讨论了瞬态错误处理对云应用程序。 
 
瞬时故障处理可与数据相关的路由模式自然共存。 主要的要求是重试包括**using**块而获得的数据依赖于路由连接整个数据访问请求。 无法重写上面的示例中，如下 （注意突出显示更改）。 

### 示例-具有瞬时性故障处理路由取决于数据 

<pre><code>int customerId = 12345; 
int newPersonId = 4321; 

<span style="background-color:  #FFFF00">Configuration.SqlRetryPolicy.ExecuteAction(() =&gt; </span> 
<span style="background-color:  #FFFF00">    { </span>
        // Connection to the shard for that customer ID 
        using (SqlConnection conn = customerShardMap.OpenConnectionForKey(customerId,  
        Configuration.GetCredentialsConnectionString(), ConnectionOptions.Validate)) 
        { 
            // Execute a simple command 
            SqlCommand cmd = conn.CreateCommand(); 

            cmd.CommandText = @&quot;UPDATE Sales.Customer 
                            SET PersonID = @newPersonID 
                            WHERE CustomerID = @customerID&quot;; 

            cmd.Parameters.AddWithValue(&quot;@customerID&quot;, customerId); 
            cmd.Parameters.AddWithValue(&quot;@newPersonID&quot;, newPersonId); 
            cmd.ExecuteNonQuery(); 

            Console.WriteLine(&quot;Update completed&quot;); 
        } 
<span style="background-color:  #FFFF00">    }); </span> 
</code></pre>


生成弹性数据库示例应用程序时，将自动下载包有必要实现瞬时性故障处理。 包都还分别在[企业库 — 瞬时性故障处理应用程序块](http://www.nuget.org/packages/EnterpriseLibrary.TransientFaultHandling/)。 使用版本 6.0 或更高版本。 

## 事务一致性 

事务属性保证所有操作的 shard 的本地。 例如，交易记录提交到依赖于数据的路由连接目标 shard 的范围内执行。 这次，有没有能力供登记到事务的多个连接，因此没有在 shards 之间执行的操作的任何事务处理保证。  

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]
 