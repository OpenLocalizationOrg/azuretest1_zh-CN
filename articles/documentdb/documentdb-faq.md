---
ms.openlocfilehash: ecde673c695bd645b1cfadf943545c5dd53cbb95
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="关于 DocumentDB 常见问题 |Microsoft Azure" 
    description="关于 Azure DocumentDB，NoSQL 文档数据库服务相关的常见问题的解答。" 
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
    ms.date="09/01/2015" 
    ms.author="mimig"/>


#DocumentDB 有关的常见问题

## Microsoft Azure DocumentDB 基础知识

### 什么是 Microsoft Azure DocumentDB？ 
Microsoft Azure DocumentDB 是高度可扩展 NoSQL 文档数据库作为-服务提供丰富查询架构的数据，有助于提供可配置的和可靠的性能、 和实现快速发展，通过托管平台强大的后盾和 Microsoft Azure 的到达。 DocumentDB 适合 web 和移动应用程序的解决方案时才可预测的吞吐量、 低延迟和架构的数据模型时的关键要求。 DocumentDB 提供了架构的灵活性和丰富索引通过本机的 JSON 数据模型，并且支持多文档事务与集成的 JavaScript。  
  
有关配置和使用此服务的说明，请参阅[DocumentDB 文档页中找到](http://azure.microsoft.com/documentation/services/documentdb/)。

### 哪种类型是数据库的 DocumentDB？
DocumentDB 是 NoSQL 文档面向的数据库，以 JSON 格式存储数据。  DocumentDB 支持嵌套、 自助 contained 数据结构可通过丰富的 DocumentDB [SQL 查询语法](documentdb-sql-query.md)查询。 DocumentDB 提供了高性能事务处理服务器端 JavaScript 通过[存储过程、 触发器和用户定义的函数](documentdb-programming.md)。 该数据库还支持具有相关联[的性能级别](documentdb-performance-levels.md)的开发人员可调式的一致性级别。
 
### DocumentDB 数据库有像关系数据库 (RDBMS) 的表吗？
否，DocumentDB 的 JSON 文档集合中存储的数据。  DocumentDB 资源信息，请参阅[DocumentDB 资源模型和概念](documentdb-resources.md)。 

### DocumentDB 数据库支持架构的数据吗？
是的 DocumentDB 允许应用程序存储任意 JSON 文档架构定义或提示。 数据可立即用于查询通过 DocumentDB SQL 查询界面。   

### DocumentDB 是否支持 ACID 事务？
是的 DocumentDB 支持跨文档表示为 JavaScript 存储过程和触发器的交易记录。 交易记录应用范围限定为单个集合和执行与 ACID 语义或无隔离与其他并发执行的代码和用户请求。  如果通过服务器端执行的 JavaScript 应用程序代码，则将引发异常，则回滚整个事务。 

### 对于 DocumentDB 的典型用例是什么？  
DocumentDB 是新的 web 和移动应用程序的理想选择刻度、 性能和对架构的数据查询的能力非常重要。 DocumentDB 适合于快速开发和支持应用程序数据模型的连续迭代。 应用程序管理用户生成的内容和数据是[DocumentDB 的常见使用情形](documentdb-use-cases.md)。  

### DocumentDB 的比例限制有哪些？
通过添加集合，可以在存储和吞吐量方面扩展 DocumentDB 帐户。 请参见[DocumentDB 限制](documentdb-limits.md)的回收数量的服务配额。 如果您需要其他收藏集，请[与支持部门联系](documentdb-increase-limits.md)能够增加您帐户的配额。 

### Microsoft Azure DocumentDB 成本是多少？
请[DocumentDB 定价详细信息](http://go.microsoft.com/fwlink/p/?LinkID=402317)页面的详细信息，参阅。 DocumentDB 的使用费由集正在使用中，集合处于联机状态的小时数的数目和每个集合的[性能级别](documentdb-performance-levels.md)。 

### 有免费的试用版可用吗？
如果您是新手 Azure，您可以注册[Azure 的免费试用版](https://azure.microsoft.com/pricing/free-trial/)，这为您提供 30 天和 200 美元可以尝试所有的 Azure 服务。 或者，如果您订阅了 MSDN，您可以获得[每月免费 Azure 片尾在 150 美元](http://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/)用于任何 Azure 服务。  

### 如何获取 DocumentDB 的其他帮助？
如果您需要任何帮助，请给我们动身的[堆栈溢出](http://stackoverflow.com/questions/tagged/azure-documentdb)， [Azure DocumentDB MSDN 开发人员论坛](https://social.msdn.microsoft.com/forums/azure/home?forum=AzureDocumentDB)上或安排[与 DocumentDB 的工程团队的 1:1 聊天](http://www.askdocdb.com/)。 保持最新的 DocumentDB 的最新新闻和功能，并遵照我们[使用 Twitter](https://twitter.com/DocumentDB)。

## 设置 Microsoft Azure DocumentDB

### 如何注册为 Microsoft Azure DocumentDB？
Microsoft Azure DocumentDB 是[Azure 预览门户][azure 门户]中可用。  首先您必须注册为 Microsoft Azure 订阅。  注册 Microsoft Azure 订阅后，您可以将 DocumentDB 帐户添加到 Azure 订购。 有关添加 DocumentDB 帐户的说明，请参阅[创建 DocumentDB 数据库帐户](documentdb-create-account.md)。   

### 主关键是什么？
主密钥是安全令牌访问帐户的所有资源。 与键的个人具有读取和写入数据库帐户中的所有资源的访问。 在分发主密钥时要格外小心。 [Azure 预览门户][azure 门户]的**键**刀片式服务器中有主键的主服务器和辅助主机键。 有关密钥的详细信息，请参阅[查看、 复制和再生的访问键](documentdb-manage-account.md#keys)。

### 如何创建数据库？
您可以创建数据库使用[Azure 预览门户]()[创建 DocumentDB 的数据库](documentdb-create-database.md)，一个[DocumentDB 的 Sdk](https://msdn.microsoft.com/library/azure/dn781482.aspx)，或通过[REST Api](https://msdn.microsoft.com/library/azure/dn781481.aspx)中所述。  

### 集合是什么？
集合是 JSON 文档和关联的 JavaScript 应用程序逻辑的容器。 查询和事务的范围内对集合。 您可以存储一组异类 JSON 文档中单个集合，所有自动建立索引。 

集合也是 DocumentDB 的计费实体。 您每月的 DocumentDB 使用费由集正在使用中，集合处于联机状态的小时数的数目和每个集合的[性能级别](documentdb-performance-levels.md)。 有关详细信息，请参阅[DocumentDB 定价](https://azure.microsoft.com/pricing/details/documentdb/)。  

### 是否有数据库和集合上的任何限制？
每个集合附带的数据库存储和支持[性能级别](documentdb-performance-levels.md)之一调配的产量分配。  在由服务管理的每个资源的地方也是配额。 所有限制的列表，请参阅[DocumentDB 限制](documentdb-limits.md)。 要请求更改的帐户限制，请参阅[请求增加 DocumentDB 帐户限制](documentdb-increase-limits.md)。  

### 如何设置用户和权限？
您可以创建用户和权限是使用一个[DocumentDB Sdk](https://msdn.microsoft.com/library/azure/dn781482.aspx)或通过[REST Api](https://msdn.microsoft.com/library/azure/dn781481.aspx)。   

## 开发针对 Microsoft Azure DocumentDB

### 如何为我启动针对 DocumentDB 开发？
为.NET、 Python，Node.js，JavaScript 和 Java 提供了[Sdk](https://msdn.microsoft.com/library/azure/dn781482.aspx) 。  开发人员还可以利用[rest 风格的 HTTP Api](https://msdn.microsoft.com/library/azure/dn781481.aspx)与从不同的平台和语言的 DocumentDB 资源进行交互。 

DocumentDB [.NET](https://github.com/Azure/azure-documentdb-net/tree/master/samples/code-samples)、 [Java](https://github.com/Azure/azure-documentdb-java)、 [Node.js](https://github.com/Azure/azure-documentdb-node/tree/master/samples)，和[Python](https://github.com/Azure/azure-documentdb-python) Sdk 示例是在 GitHub 上可用。

### DocumentDB 不支持 SQL 吗？
DocumentDB SQL 的查询语言提供了丰富的层次结构和关系运算符和扩展性通过 JavaScript 基于用户定义的函数 (Udf)。 JSON 语法允许建模的 JSON 文档具有标签树作为为树中的节点，由 DocumentDB 自动索引技术以及对 SQL 查询语言 DocumentDB。  有关如何使用 SQL 语法的详细信息，请参阅[查询 DocumentDB][查询]。

### 由 DocumentDB 支持的数据类型有哪些？
在 DocumentDB 中所支持的基元数据类型都相同的 json 格式。 JSON 具有简单类型系统，其中包含字符串、 数字 （IEEE754 双精度） 和布尔值的真，假和空。  日期时间、 Guid、 Int64 和几何等更复杂的数据类型可以通过创建的嵌套对象使用 {} 的运算符和阵列使用 [] 运算符在 JSON 和 DocumentDB 两个表示。 

### DocumentDB 是如何提供并发的？
DocumentDB 支持通过 HTTP 实体标记或 Etag 的乐观并发控制 (OCC)。 DocumentDB 中的每个资源都有 ETag，和 DocumentDB 的客户包括写入请求读取其最新版本。 如果 ETag 是当前的则会提交更改。 如果已从外部更改值，服务器将拒绝与写"HTTP 412 前置条件失败"响应代码。 客户端必须读取资源的最新版本，然后重试该请求。 

### 如何在 DocumentDB 中执行的交易记录？
DocumentDB 支持使用语言集成交易都通过 JavaScript 存储过程和触发器。 在脚本内部的所有数据库操作都执行下作用于该集合的快照隔离。 文档版本 (Etag) 的快照是在事务开始时获取并提交仅当该脚本成功完成。 如果 JavaScript 将引发错误，则回滚事务。 有关详细信息，请参阅[DocumentDB 服务器端编程](documentdb-programming.md)。

### 如何插入 DocumentDB 批量插入文档？ 
有三种方法可以到 DocumentDB 批量插入的文档︰

- 数据迁移工具，[将数据导入 DocumentDB](documentdb-import-data.md)中所述。
- 文档资源管理器在 Azure 预览门户中，[批量添加文档资源管理器的文档](documentdb-view-json-document-explorer.md#BulkAdd)中所述。
- [DocumentDB 服务器端编程](documentdb-programming.md)中所述的存储过程。

### DocumentDB 支持资源的链接缓存？
是的因为 DocumentDB 是 rest 风格的服务，资源链接是不可变的并可以缓存。 DocumentDB 客户端可以读取任何资源如文档或收集和更新其本地复制仅当服务器版本已更改时才对指定的"如果-无-匹配"头。 




[azure 门户]: https://portal.azure.com
[查询]: documentdb-sql-query.md
 
测试
