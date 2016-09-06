---
ms.openlocfilehash: 8b4364276e5bc9a76aa12c75a35fc365f10a6826
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    title="Getting started with elastic database query"
    pageTitle="要开始使用弹性数据库查询"
    description="如何使用弹性数据库查询"
    metaKeywords="azure sql database elastic queries"
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
    ms.date="06/23/2015"
    ms.author="sidneyh" />

# 要开始使用弹性数据库查询

SQL Azure 数据库的弹性数据库查询 （预览） 允许您运行跨多个数据库使用单个连接点的 T SQL 查询。 关于弹性数据库查询功能的详细信息，请参阅[功能概述页](sql-database-elastic-query-overview.md)。

本主题将扩展在[弹性数据库工具入门](sql-database-elastic-scale-get-started.md)中找到的示例。 完成后，您将︰ 学习如何配置和使用 Azure SQL 数据库来执行跨多个相关的数据库的查询。
## 先决条件

下载并运行[弹性数据库工具示例入门](sql-database-elastic-scale-get-started.md)。

## 创建 shard 地图管理器使用的示例应用程序

这里您将创建一个 shard 映射以及几个 shards，shards 到跟插入数据管理器。 如果碰巧他们中已经有 shards 安装程序使用 sharded 数据，可以跳过以下步骤并移到下一节。

1. 生成并运行**弹性数据库工具入门**示例应用程序。 直到步骤 7 部分[下载并运行示例应用程序](sql-database-elastic-scale-get-started.md#Getting-started-with-elastic-database-tools)中执行的步骤。 在第 7 步结束时，您将看到以下命令提示符︰

    ![命令提示符][1]

2.  在命令窗口中，键入"1"，然后按**Enter**。 这创建 shard 地图管理器中，并向服务器中添加两个 shards。 键入"3"，然后按**enter 键**。四次重复操作。 这会在您 shards 插入示例数据行。
3.  [Azure 预览门户](https://portal.azure.com)应显示 v12 服务器中的三个新的数据库︰

    ![Visual Studio 确认][2]

    在这种情况下，通过弹性数据库客户端库支持跨数据库查询。 例如，在命令窗口中使用选项 4。 多 shard 查询的结果始终是**UNION ALL**所有 shards 的结果。

    在下一节中，我们将创建用来支持跨 shards 的数据的丰富查询示例数据库终结点。

## 创建一个弹性的查询数据库

1. 打开[Azure 预览门户](https://portal.azure.com)，然后以用户身份登录。
2. 在 shard 安装程序所在的服务器中创建新的 SQL Azure 数据库。 命名数据库"ElasticDBQuery"。 定价层上，您必须选择一个津贴的提供。 弹性数据库查询目前仅在高级层上。

    ![Azure 门户和定价层][3]

    注意︰ 您可以使用现有的高级数据库。 如果能够这样做，它不能要上执行查询 shards 之一。 此数据库将用于创建弹性数据库查询的元数据对象。


## 创建数据库对象

### 数据库范围主要密钥和凭据

这些用于连接到 shard 地图管理器和 shards:

1. 在 Visual Studio 中打开 SQL Server 管理 Studio 或 SQL Server 数据工具。
2. 连接到 ElasticDBQuery 数据库，并执行以下 T SQL 命令︰

        CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<password>';

        CREATE DATABASE SCOPED CREDENTIAL ElasticDBQueryCred
        WITH IDENTITY = '<username>',
        SECRET = '<password>';

    "用户名"和"密码"应该[下载并运行示例应用程序](sql-database-elastic-scale-get-started.md#Getting-started-with-elastic-database-tools)[入门弹性数据库工具](sql-database-elastic-scale-get-started.md)中的步骤 6 中使用的登录信息相同。

### 外部数据源

若要创建外部数据源，请在 ElasticDBQuery 数据库上执行以下命令︰

    CREATE EXTERNAL DATA SOURCE MyElasticDBQueryDataSrc WITH
      (TYPE = SHARD_MAP_MANAGER,
      LOCATION = '<server_name>.database.windows.net',
      DATABASE_NAME = 'ElasticScaleStarterKit_ShardMapManagerDb',
      CREDENTIAL = ElasticDBQueryCred,
      SHARD_MAP_NAME = 'CustomerIDShardMap'
    ) ;

 "CustomerIDShardMap"是 shard 映射的名称，如果您创建的 shard 映射和 shard 地图管理器使用弹性数据库工具示例。 但是，如果此示例中使用自定义安装，它应是您在您的应用程序中选择 shard 的映射名称。

### 外部表

创建外部表相匹配 shards 在客户表，通过在 ElasticDBQuery 数据库上执行以下命令︰

    CREATE EXTERNAL TABLE [dbo].[Customers]
    ( [CustomerId] [int] NOT NULL,
      [Name] [nvarchar](256) NOT NULL,
      [RegionId] [int] NOT NULL)
    WITH
    ( DATA_SOURCE = MyElasticDBQueryDataSrc,
      DISTRIBUTION = SHARDED([CustomerId])
    ) ;

## 执行示例弹性数据库 T SQL 查询

一旦您定义了外部数据源，您现在可以使用完整的 T SQL 外部表的外部表。

在 ElasticDBQuery 数据库上执行以下查询︰

    select count(CustomerId) from [dbo].[Customers]

您会看到该查询从所有 shards 聚合结果并给出下面的输出︰

![输出详细信息][4]

## 向 Excel 中导入弹性数据库查询结果

 您可以导入到 Excel 文件的查询从结果。

1. 启动 Excel 2013。
2.  导航到**数据**功能区。
3.  单击**从其他来源**，然后单击**从 SQL Server 中**。

    ![从其他源 Excel 导入][5]
4.  在**数据连接向导**中，键入服务器名称和登录凭据。 然后单击**下一步**。
5.  在**选择包含所需的数据的数据库**对话框中，选择**ElasticDBQuery**数据库。
6.  在列表视图中选择**客户**表，然后单击**下一步**。 然后单击**完成**。
7.  在**导入数据**表单中，**选择您希望查看此工作簿中的数据**下, 选择**表**并单击**确定**。

存储在不同的 shards 中的**客户**表中的所有行来都填充在 Excel 工作表。

## 下一步行动
现在，您可以使用 Excel 的功能强大的数据功能。 可以与您的服务器名称、 数据库名称和凭据使用连接字符串连接到弹性查询数据库的 BI 和数据集成工具。 请确保 SQL Server 支持作为您的工具的数据源。 您可以参考弹性查询数据库，就像任何其他 SQL Server 数据库的外部表与您的工具将连接到的 SQL Server 表。

### Cost
没有使用弹性数据库查询功能无需额外付费。 但是，此时此功能仅适用于高级数据库作为终点，但 shards 可以是任何的服务层。

价格信息请参阅[SQL 数据库定价详细信息](http://azure.microsoft.com/pricing/details/sql-database/)。


[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-query-getting-started/cmd-prompt.png
[2]: ./media/sql-database-elastic-query-getting-started/portal.png
[3]: ./media/sql-database-elastic-query-getting-started/tiers.png
[4]: ./media/sql-database-elastic-query-getting-started/details.png
[5]: ./media/sql-database-elastic-query-getting-started/exel-sources.png
<!--anchors-->
