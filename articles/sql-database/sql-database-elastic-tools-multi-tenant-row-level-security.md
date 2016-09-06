---
ms.openlocfilehash: 693ec9e6314eace43c42bb23e7796e55f58c7105
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    title="Multi-tenant applications with elastic database tools and row-level security" 
    pageTitle="具有弹性数据库工具和行级别安全性的多租户应用程序" 
    description="了解如何使用和行级别安全性的弹性数据库工具来构建具有高可伸缩性的数据层应用程序支持多租户 shards Azure SQL 数据库上。" 
    metaKeywords="azure sql database elastic tools multi tenant row level security rls" 
    services="sql-database" documentationCenter=""  
    manager="jeffreyg" 
    authors="tmullaney"/>

<tags 
    ms.service="sql-database" 
    ms.workload="sql-database" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/19/2015" 
    ms.author="thmullan;torsteng;sidneyh" />

# 具有弹性数据库工具和行级别安全性的多租户应用程序 

[弹性数据库工具](sql-database-elastic-scale-get-started.md)和[行级别安全性 (RLS)](https://msdn.microsoft.com/library/dn765131)提供一组功能强大的灵活、 高效地扩展数据层的多租户应用程序与 SQL Azure 数据库的功能。 本文说明了如何结合使用这些技术来构建应用程序具有支持多租户 shards，使用**ADO.NET SqlClient**和/或**实体框架**的高度可扩展的数据层。 

* **弹性的数据库工具**使开发人员能够扩展通过业界标准分片做法使用.NET 库和 Azure 服务模板的一组应用程序的数据层。 管理与使用弹性数据库客户端库 shards 帮助自动化和简化许多通常与分片的基础结构性任务。 

* **行级别安全性**使开发人员能够将数据存储为多个承租人在同一数据库中使用安全策略来过滤不属于组织执行查询的行。 集中访问逻辑与 RLS 内部数据库中，而不是在应用程序中，简化了维护并根据应用程序的代码库，减少了错误的风险增大。 RLS 需要最新的[SQL Azure 数据库更新 (V12)](sql-database-preview-whats-new.md)。 

结合使用这些功能，应用程序可以受益于成本节约和效率收益为同一 shard 数据库中多个租户存储数据。 同时，应用程序仍有提供独立、 单租户 shards 为"特优"承租人需要更严格的性能保证，因为多租户 shards 并不能保证承租人之间的相等资源分配的灵活性。  

简单地说，弹性的数据库客户端库的[数据依赖于路由](sql-database-elastic-scale-data-dependent-routing.md)Api 自动连接承租人正确 shard 数据库包含其分片键 (通常"TenantId")。 建立连接后，可确保 RLS 安全策略数据库中的承租人仅可以访问包含其 TenantId 的行。 假定所有的表包含的 TenantId 列，以指示哪些行属于每个租户。 

![博客应用程序体系结构][1]

## 下载样例项目

### 先决条件
* 使用 Visual Studio （2012年或更高版本） 
* 创建三个 Azure SQL 数据库 
* 下载样例项目︰[弹性 Azure SQL 的多租户 Shards 数据库工具](http://go.microsoft.com/?linkid=9888163)
  * **Program.cs**在开始数据库的信息填充 

此项目将扩展述[弹性 Azure SQL 的实体框架集成的数据库工具](sql-database-elastic-scale-use-entity-framework-applications-visual-studio.md)添加对于多租户 shard 的数据库支持。 它构建了四个承租人与上图中所示的两个 shard 多租户数据库创建博客和文章，一个简单的控制台应用程序。 

生成并运行该应用程序。 这将引导弹性数据库工具的 shard 地图管理器，并运行以下测试︰ 

1. 使用实体框架和 LINQ，创建新的博客，然后为每个租户显示所有博客
2. 使用 ADO.NET SqlClient，显示所有博客的租户
3. 尝试插入错误的租户，以验证错误引发的博客  

请注意，由于 RLS 尚未启用 shard 数据库中，每个测试中发现问题︰ 承租人能够看到博客，不属于自己，并且应用程序不会阻止插入错误的租户的博客。 本文的其余部分介绍如何解决这些问题，通过强制执行与 RLS 的租户隔离。 有两个步骤︰ 

1. **应用层**︰ 修改应用程序代码始终将 CONTEXT_INFO 设置为当前的 TenantId，在打开连接后。 示例项目已经这样。 
2. **数据层**︰ 在基于 CONTEXT_INFO 的值的筛选行的每个 shard 数据库中创建 RLS 安全策略。 您将需要为每个 shard 数据库执行此操作，否则不会筛选多租户 shards 中的行。 


## 步骤 1） 应用程序层︰ 将 CONTEXT_INFO 设置为 TenantId

连接到使用弹性数据库客户端库的数据依赖于路由 Api，应用程序仍需要告诉数据库 shard 数据库后的 TenantId 使用该连接以便 RLS 安全策略可以过滤掉属于其他承租人的行。 将此信息传递的推荐的方法是将[CONTEXT_INFO](https://msdn.microsoft.com/library/ms180125)设置为该连接当前的 TenantId。 请注意，在 Azure SQL 数据库中，CONTEXT_INFO 预先填入特定于会话的 GUID，以便您之前，*必须*设置 CONTEXT_INFO 到正确的 TenantId 执行一个新的连接，以确保没有任何行都不小心泄漏的任何查询。

### 实体框架

对于使用实体框架应用程序，最简单的方法是将 CONTEXT_INFO 设置[数据依赖于路由使用 EF DbContext](sql-database-elastic-scale-use-entity-framework-applications-visual-studio.md/#data-dependent-routing-using-ef-dbcontext)中所述的 ElasticScaleContext 重写内。 之后返回通过数据相关路由通过代理连接，只需创建并执行将 CONTEXT_INFO 设置为 shardingKey (TenantId) 为该连接指定 SqlCommand。 这种方法，只需一次编写代码来设置 CONTEXT_INFO。 

```
// ElasticScaleContext.cs 
// ... 
// C'tor for data dependent routing. This call will open a validated connection routed to the proper 
// shard by the shard map manager. Note that the base class c'tor call will fail for an open connection 
// if migrations need to be done and SQL credentials are used. This is the reason for the  
// separation of c'tors into the DDR case (this c'tor) and the internal c'tor for new shards. 
public ElasticScaleContext(ShardMap shardMap, T shardingKey, string connectionStr)
    : base(OpenDDRConnection(shardMap, shardingKey, connectionStr), true /* contextOwnsConnection */)
{
}

public static SqlConnection OpenDDRConnection(ShardMap shardMap, T shardingKey, string connectionStr)
{
    // No initialization
    Database.SetInitializer<ElasticScaleContext<T>>(null);

    // Ask shard map to broker a validated connection for the given key
    SqlConnection conn = null;
    try
    {
        conn = shardMap.OpenConnectionForKey(shardingKey, connectionStr, ConnectionOptions.Validate);

        // Set CONTEXT_INFO to shardingKey to enable Row-Level Security filtering
        SqlCommand cmd = conn.CreateCommand();
        cmd.CommandText = @"SET CONTEXT_INFO @shardingKey";
        cmd.Parameters.AddWithValue("@shardingKey", shardingKey);
        cmd.ExecuteNonQuery();

        return conn;
    }
    catch (Exception)
    {
        if (conn != null)
        {
            conn.Dispose();
        }

        throw;
    }
} 
// ... 
```

现在 CONTEXT_INFO 将自动设置为指定的 TenantId 每当调用 ElasticScaleContext 时︰ 

```
// Program.cs 
SqlDatabaseUtils.SqlRetryPolicy.ExecuteAction(() => 
{   
    using (var db = new ElasticScaleContext<int>(sharding.ShardMap, tenantId, connStrBldr.ConnectionString))   
    {     
        var query = from b in db.Blogs
                    orderby b.Name
                    select b;
        
        Console.WriteLine("All blogs for TenantId {0}:", tenantId);     
        foreach (var item in query)     
        {       
            Console.WriteLine(item.Name);     
        }   
    } 
}); 
```

### ADO.NET SqlClient 

对于使用 ADO.NET SqlClient 的应用程序，建议使用的方法是创建自动将 CONTEXT_INFO 设置为正确的 TenantId，返回一个连接前的 ShardMap.OpenConnectionForKey() 周围的包装函数。 若要确保始终正确地设置 CONTEXT_INFO，您应该只打开使用此包装函数连接。 

```
// Program.cs
// ...

// Wrapper function for ShardMap.OpenConnectionForKey() that automatically sets CONTEXT_INFO to the correct
// tenantId before returning a connection. As a best practice, you should only open connections using this 
// method to ensure that CONTEXT_INFO is always set before executing a query.
public static SqlConnection OpenConnectionForTenant(ShardMap shardMap, int tenantId, string connectionStr)
{
    SqlConnection conn = null;
    try
    {
        // Ask shard map to broker a validated connection for the given key
        conn = shardMap.OpenConnectionForKey(tenantId, connectionStr, ConnectionOptions.Validate);

        // Set CONTEXT_INFO to shardingKey to enable Row-Level Security filtering
        SqlCommand cmd = conn.CreateCommand();
        cmd.CommandText = @"SET CONTEXT_INFO @shardingKey";
        cmd.Parameters.AddWithValue("@shardingKey", tenantId);
        cmd.ExecuteNonQuery();

        return conn;
    }
    catch (Exception)
    {
        if (conn != null)
        {
            conn.Dispose();
        }

        throw;
    }
}

// ...

// Example query via ADO.NET SqlClient
// If row-level security is enabled, only Tenant 4's blogs will be listed
SqlDatabaseUtils.SqlRetryPolicy.ExecuteAction(() =>
{
    using (SqlConnection conn = OpenConnectionForTenant(sharding.ShardMap, tenantId4, connStrBldr.ConnectionString))
    {
        SqlCommand cmd = conn.CreateCommand();
        cmd.CommandText = @"SELECT * FROM Blogs";

        Console.WriteLine("--\nAll blogs for TenantId {0} (using ADO.NET SqlClient):", tenantId4);
        SqlDataReader reader = cmd.ExecuteReader();
        while (reader.Read())
        {
            Console.WriteLine("{0}", reader["Name"]);
        }
    }
});

```

## 第 2 步） 数据层︰ 创建行级别的安全策略和约束 

### 创建安全策略筛选选择、 更新和删除查询 

现在，应用程序将查询之前将 CONTEXT_INFO 设置为当前的 TenantId，RLS 安全策略可以筛选查询，并排除行具有不同的 TenantId。  

RLS 采用 T SQL︰ 用户定义的谓词函数定义的筛选逻辑和安全策略将此函数绑定到任意数量的表。 对于此项目，将在谓词函数只是验证连接到数据库，应用程序 （而不是某一其他 SQL 用户） 和 CONTEXT_INFO 的值匹配给定行的 TenantId。 满足这些条件行，将通过筛选器允许的选择中，更新查询和删除查询。 如果未设置 CONTEXT_INFO，则将不返回任何行。 

若要启用 RLS，在使用 Visual Studio (SSDT)、 SSMS 或 PowerShell 脚本包含在项目中的所有 shards 上执行下面的 T SQL （或如果您使用的[弹性数据库作业](sql-database-elastic-jobs-overview.md)，可以使用它来自动执行的所有 shards 在此 T SQL 执行）︰ 

```
CREATE SCHEMA rls -- separate schema to organize RLS objects 
GO

CREATE FUNCTION rls.fn_tenantAccessPredicate(@TenantId int)     
    RETURNS TABLE     
    WITH SCHEMABINDING
AS
    RETURN SELECT 1 AS fn_accessResult          
        WHERE DATABASE_PRINCIPAL_ID() = DATABASE_PRINCIPAL_ID('dbo') -- the user in your application’s connection string (dbo is only for demo purposes!)         
        AND CONVERT(int, CONVERT(varbinary(4), CONTEXT_INFO())) = @TenantId -- @TenantId (int) is 4 bytes 
GO

CREATE SECURITY POLICY rls.tenantAccessPolicy
    ADD FILTER PREDICATE rls.fn_tenantAccessPredicate(TenantId) ON dbo.Blogs,
    ADD FILTER PREDICATE rls.fn_tenantAccessPredicate(TenantId) ON dbo.Posts
GO 
```

> [AZURE.TIP] 对于需要含有上百个表上添加谓词函数的复杂项目，可以使用自动生成的架构中的所有表上添加一个谓词的安全策略的帮助器存储过程。 [应用到所有表 – 帮助器脚本 （博客） 的行级别安全性](http://blogs.msdn.com/b/sqlsecurity/archive/2015/03/31/apply-row-level-security-to-all-tables-helper-script)，请参阅。  

如果您以后添加的新表，只需更改安全策略并添加新表上的筛选器谓词︰ 

```
ALTER SECURITY POLICY rls.tenantAccessPolicy     
    ADD FILTER PREDICATE rls.fn_tenantAccessPredicate(TenantId) ON dbo.MyNewTable 
GO 
```

现在，如果您运行示例应用程序再次，承租人不能查看不属于自己的行。 

### 添加 check 约束来阻止错误的租户插入和更新

目前，RLS 安全策略不会防止应用程序发生意外错误 TenantId，用于插入行或更新可见行的 TenantId 是一个新的值。 对于某些应用程序，如只读报告应用程序，这不是一个问题。 但是，由于此应用程序允许承租人要插入新的博客，是物有所值来创建违反筛选器谓词将引发错误，如果应用程序代码错误地尝试插入或更新行此类附加保护。  如中所述[行级别安全性︰ 阻止未经授权的插入 （博客）](http://blogs.msdn.com/b/sqlsecurity/archive/2015/03/23/row-level-security-blocking-unauthorized-inserts)，推荐的解决方案是创建实施相同的 RLS 筛选器谓词为插入和更新操作的每个表的 check 约束。 

若要添加 check 约束，执行上利用 SSMS、 SSDT，或包括 PowerShell 脚本 （或弹性数据库作业） 如上面所述的所有 shards 以下的 T SQL: 

```
-- Create a scalar version of the predicate function for use in check constraints 
CREATE FUNCTION rls.fn_tenantAccessPredicateScalar(@TenantId int)     
    RETURNS bit 
AS     
    BEGIN     
        IF EXISTS( SELECT 1 FROM rls.fn_tenantAccessPredicate(@TenantId) )         
            RETURN 1     
        RETURN 0 
    END 
GO 

-- Add the function as a check constraint on all sharded tables 
ALTER TABLE Blogs     
    WITH NOCHECK -- don't check data already in table     
    ADD CONSTRAINT chk_blocking_Blogs -- needs a unique name     
    CHECK( rls.fn_tenantAccessPredicateScalar(TenantId) = 1 ) 
GO

ALTER TABLE Posts     
    WITH NOCHECK     
    ADD CONSTRAINT chk_blocking_Posts     
    CHECK( rls.fn_tenantAccessPredicateScalar(TenantId) = 1 ) 
GO 
```

现在，应用程序不能插入行属于承租人不是当前连接到 shard 的数据库。 同样，应用程序无法更新可见行具有不同的 TenantId。 如果应用程序尝试执行两种操作，则会引发 DbUpdateException。 


### 添加默认约束来自动填充插入 TenantId 

除了使用检查约束来阻止错误的租户插入，您可以将每个表中插入行时自动填充 TenantId 与 CONTEXT_INFO 的当前值的默认约束。 例如︰ 

```
-- Create default constraints to auto-populate TenantId with the value of CONTEXT_INFO for inserts 
ALTER TABLE Blogs     
    ADD CONSTRAINT df_TenantId_Blogs      
    DEFAULT CONVERT(int, CONVERT(varbinary(4), CONTEXT_INFO())) FOR TenantId 
GO

ALTER TABLE Posts     
    ADD CONSTRAINT df_TenantId_Posts      
    DEFAULT CONVERT(int, CONVERT(varbinary(4), CONTEXT_INFO())) FOR TenantId 
GO 
```

现在，应用程序不需要在插入行时指定 TenantId: 

```
SqlDatabaseUtils.SqlRetryPolicy.ExecuteAction(() => 
{   
    using (var db = new ElasticScaleContext<int>(sharding.ShardMap, tenantId, connStrBldr.ConnectionString))
    {
        var blog = new Blog { Name = name }; // default constraint sets TenantId automatically     
        db.Blogs.Add(blog);     
        db.SaveChanges();   
    } 
}); 
```

> [AZURE.NOTE] 如果您使用实体框架项目的默认约束，建议，不包括 TenantId 列在 EF 数据模型中。 这是因为实体框架查询自动提供默认值将重写使用 CONTEXT_INFO T SQL 中创建默认约束。 若要使用默认约束示例项目中，例如，您应删除 TenantId 从 DataClasses.cs （和在程序包管理器控制台运行的添加迁移） 并使用 T SQL，以确保该字段仅存在于数据库表。 这样一来，EF 将自动提供正确的默认值时插入数据。 

### （可选）启用"超级用户"访问所有行
某些应用程序可能需要创建"超级用户"可以访问所有的行，例如，若要启用跨所有租户上所有 shards，报告或执行 shards 涉及的数据库之间移动租户行拆分/合并操作。 若要启用此功能，应每个 shard 数据库中创建新的 SQL 用户 （在此示例中的"超级"）。 然后更改安全策略允许此用户访问所有行的新谓词函数︰

```
-- New predicate function that adds superuser logic
CREATE FUNCTION rls.fn_tenantAccessPredicateWithSuperUser(@TenantId int)
    RETURNS TABLE
    WITH SCHEMABINDING
AS
    RETURN SELECT 1 AS fn_accessResult 
        WHERE 
        (
            DATABASE_PRINCIPAL_ID() = DATABASE_PRINCIPAL_ID('dbo') -- note, should not be dbo!
            AND CONVERT(int, CONVERT(varbinary(4), CONTEXT_INFO())) = @TenantId
        ) 
        OR
        (
            DATABASE_PRINCIPAL_ID() = DATABASE_PRINCIPAL_ID('superuser')
        )
GO

-- Atomically swap in the new predicate function on each table
ALTER SECURITY POLICY rls.tenantAccessPolicy
    ALTER FILTER PREDICATE rls.fn_tenantAccessPredicateWithSuperUser(TenantId) ON dbo.Blogs,
    ALTER FILTER PREDICATE rls.fn_tenantAccessPredicateWithSuperUser(TenantId) ON dbo.Posts
GO
```


### Maintenance 

* **添加新的 shards**︰ 必须在任何新的 shards 执行 T SQL 脚本启用 RLS （并添加 check 约束），否则不会过滤这些 shards 查询。

* **添加新表**︰ 必须将筛选器谓词添加到所有 shards 上的安全策略中，每当创建一个新表，否则不会筛选新的表上的查询。 这可以自动使用 DDL 触发器[应用行级别的安全性，自动为新创建的表 （博客）](http://blogs.msdn.com/b/sqlsecurity/archive/2015/05/22/apply-row-level-security-automatically-to-newly-created-tables.aspx)中所述。


## 摘要 

弹性数据库工具和行级别安全性可以同时支持这两种多租户应用程序的数据层扩展到使用和单租户 shards。 多租户 shards 可用于更有效地存储数据 （特别是在情况下，大量的承租人只有几行的数据），而单租户可以使用 shards 来支持具有更严格的性能和隔离要求的最优承租人。  有关详细信息，请参阅 MSDN 上的[弹性数据库工具的文档结构图](sql-database-elastic-scale-documentation-map.md)或[行级安全性的参考](https://msdn.microsoft.com/library/dn765131)。 


[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-tools-multi-tenant-row-level-security/blogging-app.png
<!--anchors-->

 
