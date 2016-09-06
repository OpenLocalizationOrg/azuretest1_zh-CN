---
ms.openlocfilehash: 6e481355e5ce7803c804ea2fcfdd9f8ccb95b65a
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="创建一个 DocumentDB 数据库 |Microsoft Azure" 
    description="了解如何创建使用 Azure DocumentDB，JSON 使用托管 NoSQL 文档数据库联机服务门户网站的集合。 得到今天的免费试用版。" 
    services="documentdb" 
    authors="mimig1" 
    manager="jhubbard" 
    editor="monicar" 
    documentationCenter=""/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/08/2015" 
    ms.author="mimig"/>

# 创建使用 Azure 预览门户 DocumentDB 集合

若要使用 Microsoft Azure DocumentDB，必须有一个[DocumentDB 帐户](documentdb-create-account.md)、[数据库](documentdb-create-database.md)、 集合和文档。 本主题介绍如何在 Azure 预览门户网站中创建的 DocumentDB 集合。 

不需要使用预览门户创建集合，您还可以创建使用[DocumentDB Sdk](https://msdn.microsoft.com/library/azure/dn781482.aspx)。 C# 的代码示例演示如何使用 DocumentDB.NET SDK 创建集合，请参阅在 CollectionManagement 项目中， [GitHub.com](https://github.com)在[azure documentdb 网络](https://github.com/Azure/azure-documentdb-net)存储库中可用的[Program.cs](https://github.com/Azure/azure-documentdb-net/blob/master/samples/code-samples/CollectionManagement/Program.cs)文件。

1.  在[Azure 预览门户网站](https://portal.azure.com/)，请单击**浏览所有**。

2.  在**浏览**刀片式服务器，请单击**DocumentDB 帐户**。

3.  在**DocumentDB 帐户**刀片式服务器，选择包含要在其中添加集合数据库的帐户。 如果没有列出任何帐户，您需要[创建一个 DocumentDB 帐户](documentdb-create-account.md)。
    
    ![屏幕抓图突出显示浏览按钮，浏览刀片上的 DocumentDB 帐户和 DocumentDB 帐户刀片式服务器上的 DocumentDB 帐户](./media/documentdb-create-collection/docdb-database-creation-1-3.png)

4. 在**数据库**刀片式服务器，请单击**添加集合**。

5. 在**集合中添加**刀片式服务器，输入您的新收藏 ID。 当验证名称时，一个绿色的复选标记将出现在 ID 框中。

6. 选择定价层的新集合。 您创建的每个集合是一个收费的实体。 有关可用的性能级别的详细信息，请参阅[DocumentDB 中的性能级别](documentdb-performance-levels.md)。

7. 选择下列**索引的策略**之一。 

    - **默认值**。 是相等对执行查询字符串和数字使用 ORDER BY、 范围和相等的查询时，此策略是最好的。  该策略具有较低的索引存储开销比**范围**。
    - **哈希**。 正在执行数字和字符串相等的查询时，此策略是最佳的。  该策略具有最低索引存储开销。
    - **范围**。 此策略最好在您 ORDER BY，范围和相等的查询上使用数字和字符串。  该策略具有更高的索引存储开销比**默认**或**哈希**。

    有关索引创建策略的详细信息，请参阅[DocumentDB 索引创建策略](documentdb-indexing-policies.md)。

8. 单击**确定**在屏幕的底部以创建新的收藏集。 

    ![屏幕抓图突出显示数据库刀片式服务器上的添加收藏按钮、 ID 框中的添加集合刀片式服务器，和确定按钮](./media/documentdb-create-collection/docdb-collection-creation-4-7.png)

9. 现在新的收藏集将出现在**集合****数据库**刀片上镜头。
 
    ![DocumentDB 帐户刀片式服务器中的新数据库的屏幕抓图](./media/documentdb-create-collection/docdb-collection-creation-8.png)

## 下一步行动

既然您已经有一个集合下, 一步是添加文档或将文档导入集合。 向集合中添加文档时，您有几个选择︰

- 可以通过使用预览门户中的文档资源管理器[中添加文档](../documentdb-view-json-document-explorer.md)。
- 通过使用 DocumentDB 数据迁移工具，该工具使您能够将 JSON 和 CSV 文件导入，以及 SQL Server，MongoDB，Azure 表存储和其他 DocumentDB 集合中的数据可以使用[导入的文档和数据](documentdb-import-data.md)。 
- 也可以通过[DocumentDB Sdk](https://msdn.microsoft.com/library/azure/dn781482.aspx)中添加文档。 DocumentDB 具有.NET、 Java、 Python，Node.js 和 JavaScript API Sdk。 [GitHub.com](https://github.com)，在[azure documentdb 网络](https://github.com/Azure/azure-documentdb-net)存储库中可用的文档管理项目中的[Program.cs](https://github.com/Azure/azure-documentdb-net/blob/master/samples/code-samples/DocumentManagement/Program.cs)文件演示使用 DocumentDB.NET SDK 文档上的 CRUD 操作。

集合中的文档后，可以使用[DocumentDB SQL](documentdb-sql-query.md) [执行查询](documentdb-sql-query.md#executing-queries)到针对文档使用[查询浏览器](documentdb-query-collections-query-explorer.md)中预览门户、 [REST API](https://msdn.microsoft.com/library/azure/dn781481.aspx)，或[Sdk](https://msdn.microsoft.com/library/azure/dn781482.aspx)之一。 

测试
