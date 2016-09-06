---
ms.openlocfilehash: 3e2d44fccb079a7ae8a21625487b65bc2aca53c0
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="创建一个 NoSQL DocumentDB 数据库 |Microsoft Azure" 
    description="了解如何创建使用 Azure DocumentDB，为 JSON 文档 NoSQL 数据库的联机服务门户托管的数据库。 得到今天的免费试用版。" 
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
    ms.date="06/26/2015" 
    ms.author="mimig"/>

# 创建使用 Azure 预览门户 DocumentDB 数据库

若要使用 Microsoft Azure DocumentDB，必须具有一个[DocumentDB 帐户](documentdb-create-account.md)、 数据库、 集合和文档。  本主题介绍如何在 Microsoft Azure 预览门户网站中创建的 DocumentDB 数据库。 

不需要使用预览门户创建数据库，您还可以创建使用[DocumentDB Sdk](https://msdn.microsoft.com/library/azure/dn781482.aspx)。 C# 的代码示例演示如何使用 DocumentDB.NET SDK 创建数据库，请参阅[Program.cs](https://github.com/Azure/azure-documentdb-net/blob/master/samples/code-samples/DatabaseManagement/Program.cs)和数据库管理在项目文件中，在[GitHub.com](https://github.com) [azure documentdb 网络](https://github.com/Azure/azure-documentdb-net)存储库中可用。 

1.  在[Azure 预览门户网站](https://portal.azure.com/)，请单击**浏览所有**。

2.  在**浏览**刀片式服务器，请单击**DocumentDB 帐户**。

3.  在**DocumentDB 帐户**刀片式服务器，选择要在其中添加 DocumentDB 数据库的帐户。 如果没有列出任何帐户，您需要[创建一个 DocumentDB 帐户](documentdb-create-account.md)。
    
    ![屏幕抓图突出显示浏览按钮，浏览刀片上的 DocumentDB 帐户和 DocumentDB 帐户刀片式服务器上的 DocumentDB 帐户](./media/documentdb-create-database/docdb-database-creation-1-3.png)

4. 在**DocumentDB 帐户**刀片式服务器，单击**添加数据库**。

5. 在**数据库添加**刀片式服务器，输入新的数据库 ID。 当验证名称时，一个绿色的复选标记将出现在 ID 框中。

6. 单击**确定**在屏幕的底部来创建新的数据库。 

    ![屏幕抓图 DocumentDB 帐户刀片式服务器上的添加数据库按钮，添加数据库刀片式服务器和确定按钮的 ID 框中突出显示](./media/documentdb-create-database/docdb-database-creation-4-6.png)

8. **DocumentDB 帐户**刀片式服务器上**的数据库**镜头现在出现新的数据库。
 
    ![DocumentDB 帐户刀片式服务器中的新数据库的屏幕抓图](./media/documentdb-create-database/docdb-database-creation-7.png)

## 下一步行动

既然您已经有一个 DocumentDB 数据库下, 一步是[创建一个集合](documentdb-create-collection.md)。

创建集合后，可以[将文档添加](../documentdb-view-json-document-explorer.md)到使用 DocumentDB 数据迁移工具集合[导入文档](documentdb-import-data.md)的预览门户中使用文档资源管理器或使用一个[DocumentDB Sdk](https://msdn.microsoft.com/library/azure/dn781482.aspx)执行 CRUD 操作。 DocumentDB 具有.NET、 Java、 Python，Node.js 和 JavaScript API Sdk。 有关.NET 代码示例演示如何创建、 删除、 更新和删除文档，请参阅[Program.cs](https://github.com/Azure/azure-documentdb-net/blob/master/samples/code-samples/DocumentManagement/Program.cs) GitHub.com 在 azure documentdb 网络存储库中的文档管理项目。  

集合中的文档后，可以使用[DocumentDB SQL](documentdb-sql-query.md) [执行查询](documentdb-sql-query.md#executing-queries)到针对文档使用[查询浏览器](documentdb-query-collections-query-explorer.md)中预览门户、 [REST API](https://msdn.microsoft.com/library/azure/dn781481.aspx)，或[Sdk](https://msdn.microsoft.com/library/azure/dn781482.aspx)之一。 

测试
