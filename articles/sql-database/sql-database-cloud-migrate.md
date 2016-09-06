---
ms.openlocfilehash: 41053c47be9913b489d83af4be9b7ca5f5fbf8f5
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="将数据库迁移到 SQL Azure 数据库"
   description="Microsoft Azure SQL 数据库，数据库部署、 迁移数据库导入数据库的数据库导出，迁移向导"
   services="sql-database"
   documentationCenter=""
   authors="carlrabeler"
   manager="jeffreyg"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-management"
   ms.date="09/02/2015"
   ms.author="carlrab"/>

# 将数据库迁移到 SQL Azure 数据库

Azure 的 SQL 数据库 V12 带来接近完成引擎兼容 SQL Server 2014年和更高版本。 在这种情况下，从内部部署的实例的 SQL Server 2005 或更高版本的大多数数据库迁移到 SQL Azure 数据库的任务是要简单得多。 许多数据库的迁移是简单架构和数据移动操作要求少，如果有的话，对架构更改和很少或没有重新进行工程处理的应用程序。 数据库需要进行更改，这些更改的范围更为局限。

按照设计，通过 Azure SQL 数据库 V12 不支持 SQL Server 的服务器范围的功能。 数据库和应用程序依赖于这些功能需要一些重建之前可以迁移。 同时 Azure SQL 数据库 V12 可以提高与本地 SQL Server 数据库的兼容性，仍需要迁移进行计划，并执行仔细，尤其是对于大型和复杂的数据库。

## 确定兼容性
若要确定是否符合 Azure SQL 数据库 V12 内部 SQL Server 数据库，可以使用选项 #1 下讨论这两种方法之一在迁移开始并查看架构验证例程检测到不兼容，或者您可以使用 Visual Studio 中的 SQL Server 数据工具下面的选项 #2 中所述来验证兼容性。 如果您在部署 SQL Server 数据库存在兼容性问题，可以使用 Visual Studio 或 SQL Server 管理 Studio 中的 SQL Server 数据工具来处理和解决兼容性问题。

## 迁移方法
有大量迁移到 Azure SQL 数据库 V12 兼容的内部部署 SQL Server 数据库的方法。

- 对于小型到中型的数据库，迁移兼容的 SQL Server 2005 或更高版本的数据库是简单，只需为在 SQL Server 管理 Studio 中，Microsoft Azure 数据库向导运行部署数据库提供您没有连接难题 （没有连接、 低带宽或超时的问题）。
- 适用于大型数据库或当您有连接难题中，您可以使用 SQL Server 管理 Studio 导出到 BACPAC 文件 （存储在本地或在 Azure 的 blob） 的数据和架构，然后将 BACPAC 文件导入到您的 SQL Azure 实例。 如果在 Azure blob 存储 BACPAC，您还可以导入从 Azure 门户中的 BACPAC 文件。 BACPAC 文件的详细信息，请参阅[数据层应用程序](https://msdn.microsoft.com/library/ee210546.aspx)。
- 对于较大的数据库，将获得最佳性能，通过分别将架构和数据的迁移。 可以提取到使用 SQL Server 管理 Studio 或 Visual Studio 数据库项目的架构，然后将部署的架构来创建 SQL Azure 数据库。 您可以提取使用 BCP 将数据，然后使用 BCP 导入到 SQL Azure 数据库使用并行流数据。 迁移大型、 复杂的数据库将耗费大量的时间，而不管所选择的方法。

### 选项 #1
***迁移数据库使用 SQL Server 管理 Studio 兼容 ***

SQL Server 管理 Studio 提供了两种方法可以将您在部署兼容的 SQL Server 数据库迁移到 SQL Azure 数据库。 可以使用部署到 Microsoft Azure SQL 数据库向导数据库或将数据库导出到一个 BACPAC 文件，其中导来创建新的 SQL Azure 数据库。  该向导验证 Azure SQL 数据库 V12 兼容性、 到 BACPAC 文件中提取的架构和数据，然后将其导入到指定的 SQL Azure 数据库实例。 若要使用此选项，请参阅[使用 SSMS](sql-database-migrate-ssms.md)。

### 选项 #2
***更新脱机使用 Visual Studio 的数据库架构，然后将部署与 SQL Server 管理 Studio***

如果您在部署 SQL Server 数据库不兼容或以确定其是否兼容，可将数据库架构导入 Visual Studio 数据库项目进行分析。 分析您为 SQL 数据库 V12 指定项目的目标平台，然后生成项目。 如果生成成功，则该数据库是兼容。 如果生成失败，则可以为 Visual Studio ("SSDT") 解决 SQL Server 数据工具中的错误。 一旦成功生成该项目，可以将其发布回为源数据库的一个副本，然后使用在 SSDT 数据比较功能将数据从源数据库复制到 Azure SQL V12 兼容数据库。 然后，此更新的数据库部署到 Azure SQL 数据库使用选项 #1。 如果仅限于架构的迁移是必需的可以直接到 SQL Azure 数据库直接从 Visual Studio 发布架构。 如果数据库架构要求不可以由迁移向导单独处理更多的更改，请使用此方法。 若要使用此选项，请参见[使用 Visual Studio](sql-database-migrate-visualstudio-ssdt.md)。

## 决定要使用的选项
- 如果您预计可以在不更改的情况下迁移的数据库应使用选项 1，快速而轻松地。  如果您不确定，首先导出仅限于架构的 BACPAC 从数据库选项 #1 中所述。 如果导出成功，没有错误，您可以使用选项 #1 要迁移的数据库使用的数据。  
- 如果导出选项 #1 的过程中遇到错误，请使用选项 2 号并纠正在 Visual Studio 使用迁移向导的组合和手动应用架构更改脱机数据库架构。 源数据库的一个副本是在 situ 中再更新，然后迁移到 Azure 使用选项 #1。

## 迁移工具
使用的工具包括 SQL Server 管理 Studio (SSMS) 而且在 Visual Studio （VS) SSDT，刀具作为 SQL Server 欠佳 Azure 的门户。

> 请务必按照较早版本之间不兼容 Azure SQL 数据库 V12 安装客户端工具的最新版本。

### SQL Server 管理 Studio (SSMS)
可以用于 SSMS，兼容数据库部署到 SQL Azure 数据库直接或导出为 BACPAC，可然后导入，仍在使用 SSMS，以创建一个新的 Azure SQL 数据库的逻辑数据库的备份。  

[下载最新版本的 SSMS](https://msdn.microsoft.com/library/mt238290.aspx)  

### SQL Server 在 Visual Studio （SSDT VS) 刀具
SQL Server 在 Visual Studio 工具可以用于创建和管理数据库项目包含一组架构中每个对象的事务处理 SQL 文件。 从数据库或从脚本文件，则可以导入该项目。 项目创建后，可以指向 SQL Azure 数据库 v12;然后生成项目验证架构兼容性。 单击错误将打开相应的事务处理 SQL 文件，允许它被编辑并已更正错误。 一旦所有的错误固定项目可以发布，直接与要创建的空数据库的 SQL 数据库或回 （一份） 原始的 SQL Server 数据库，以更新其架构，它允许使用上述 SSMS 与其数据部署数据库。

使用[Visual Studio 的新 SQL Server 数据工具](https://msdn.microsoft.com/library/mt204009.aspx)Visual Studio 2013年更新 4 或更高版本。

## 比较
| 选项 #1 | 选项 #2 |
| ------------ | ------------ | ------------ |
| 兼容的数据库部署到 SQL Azure 数据库 |   更新数据库中的位置，然后将部署到 SQL Azure 数据库 |
|![SSMS](./media/sql-database-cloud-migrate/01SSMSDiagram.png)| ![脱机编辑](./media/sql-database-cloud-migrate/03VSSSDTDiagram.png) |
| 使用 SSMS | 使用 VS 和 SSMS |
|简单的过程需要该架构兼容。 迁移的架构不变。 | 架构导入在 Visual Studio 中的数据库项目。 使用 Visual Studio 并用于更新数据库中的 situ 的最终架构 SSDT 进行其他更新。 |
| 始终将部署或导出整个数据库。 | 迁移中包含的对象的完全控制。 |
| 如果有错误，请更改输出的任何规定，源架构必须不是兼容的。 | 对 Visual Studio 可用 SSDT 的完整功能。 脱机更改架构。 | 应用程序验证发生在 Azure 中。 当架构迁移而无需更改，应很小。 | 应用程序验证可以在 SQL Server 之前的数据库部署到 Azure。 |
| 简单、 方便地配置一个-或两-步过程。 | 更复杂的多步骤过程 （如果仅部署架构更容易）。 |
