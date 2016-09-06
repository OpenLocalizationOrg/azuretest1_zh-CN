---
ms.openlocfilehash: b03fd659fb1d017d1369b478ccd2599da10d951f
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="自定义 azure 搜索索引器" 
    description="了解如何自定义设置和策略的 Azure 搜索索引器。" 
    services="search" 
    documentationCenter="" 
    authors="chaosrealm" 
    manager="pablocas" 
    editor=""/>

<tags 
    ms.service="search" 
    ms.devlang="rest-api" 
    ms.workload="search" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.date="07/08/2015" 
    ms.author="eugenesh"/>

#自定义 azure 搜索索引器#

在本文中，您将学习如何使用 Azure 搜索索引器来实现这些方案︰ 

- 重命名数据源之间的目标索引字段 
- 将数据库表中的字符串转换为字符串集合
- 打开数据源更改检测策略 
- URL 编码文档包含 URL 不安全字符的键 
- 容忍失败某些文档编制索引 

如果你不熟悉 Azure 搜索索引器，您可能想要看一看下面的文章的第一︰

- [将 SQL Azure 数据库连接到 Azure 使用索引进行搜索](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers-2015-02-28.md)
- [使用索引器的 Azure 搜索连接 DocumentDB](../documentdb/documentdb-search-indexer.md)
- [为索引器支持.NET SDK](https://msdn.microsoft.com/library/dn951165.aspx)或 
- [索引器的 REST API 参考](https://msdn.microsoft.com/library/azure/dn946891.aspx)

##重命名数据源之间的目标索引字段##

**字段映射**是属性字段定义之间的差异进行协调。 在违反 Azure 搜索命名规则的字段名称中找到的最常见示例。 考虑其中一个或多个字段名称开头前导下划线源表 (如`_id`)。 Azure 搜索不允许字段名称以下划线领导，因此必须重命名字段。 

下面的示例阐释了更新以包括"重命名"字段映射的索引器`_id`字段到数据源的`id`在目标索引字段︰

    PUT https://[service name].search.windows.net/indexers/myindexer?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]
    {
        "dataSourceName" : "mydatasource",
        "targetIndexName" : "myindex",
        "fieldMappings" : [ { "sourceFieldName" : "_id", "targetFieldName" : "id" } ] 
    } 

**注意︰**您需要使用预览 API 版本 2015年-02-28-预览字段映射。 

您可以指定多个字段映射︰ 

    "fieldMappings" : [ 
        { "sourceFieldName" : "_id", "targetFieldName" : "id" },
        { "sourceFieldName" : "_timestamp", "targetFieldName" : "timestamp" },
     ]

源和目标字段名称不区分大小写。

##将数据库表中的字符串转换为字符串集合##

此外可以使用字段映射来转换源字段值，使用*映射函数*。

一个此类函数， `jsonArrayToStringCollection`，Collection(Edm.String) 域中的目标索引，包含字符串的字段格式化为 JSON 数组的分析。 它是使用 SQL Azure 索引器的特别情况下，由于 SQL 没有本机集合数据类型。 可以按以下方式使用它︰ 

    "fieldMappings" : [ { "sourceFieldName" : "tags", "mappingFunction" : { "name" : "jsonArrayToStringCollection" } } ] 

例如，如果源字段包含的字符串`["red", "white", "blue"]`，然后目标字段类型的`Collection(Edm.String)`将使用三个值填充`"red"`， `"white"` ， `"blue"`。

请注意，`targetFieldName`属性是可选的。如果省略， `sourceFieldName` ，则使用值。

##在数据源上切换更改检测策略##
  
在 Azure 搜索，要随身携带的变化检测策略的决策很大程度上取决于由支持的数据源和数据架构。 随着时间的推移尤其是升级或迁移的数据库，您可以切换到另一种类型的变更检测策略。 例如，也许您已不仅仅被更新 SQL Azure 数据库到支持集成更改跟踪功能，因此，要从高水位策略切换到集成更改跟踪策略的新版本。 或也许您决定为高位线使用不同的列。

如果只是调用 PUT 数据源 REST API 来更新数据源时，可能会出现 400 响应返回并显示错误消息类似于以下︰


    "Change detection policy cannot be changed for data source '…' because indexer '…' references this data source and has a non-empty change tracking state. You can use Reset API to reset the indexer's change tracking state, and retry this call."

 您可能会想知道这意味着什么，以及如何解决这个问题。 由于 Azure 搜索维护与变更检测策略相关联的内部状态，将发生此错误。 策略更改时，现有的状态将失效，因为它不能应用于新的策略。 这意味着索引器已启动索引数据源从零开始使用新的策略，为您 （如数据库或其他网络的带宽费用上的额外负载） 的潜在影响。 这正是为什么 Azure 搜索请求调用[重置索引器 API]( https://msdn.microsoft.com/library/azure/dn946897.aspx)来重置当前变更检测策略，其后可以与常规传数据源调用更改策略与关联的状态。 当然，Azure 搜索无法重置为您自动执行，但我们觉得它是重要为您显式调用重置 API 确认您的含义的理解。

##URL 编码文档包含 URL 不安全字符的键##

Azure 搜索限于文档键字段内的字符 URL 安全字符，因为用户必须能够按相应的键查找文档。 所以怎么样时需要进行索引的文档包含这样的键字段中的字符？ 如果您正在使用客户端 SDK 或 REST API，自己索引文档，您可以对键进行 URL 编码。 具有索引器，就会进行 URL 编码的 Azure 搜索您的密钥通过将**base64EncodeKeys**参数设置为`true`时创建或更新索引器︰

    PUT https://[service name].search.windows.net/indexers/myindexer?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]
    {
        "dataSourceName" : "mydatasource",
        "targetIndexName" : "myindex",
        "parameters" : { "base64EncodeKeys": true }
    }

有关编码的详细信息，请参阅[MSDN 文章](http://msdn.microsoft.com/library/system.web.httpserverutility.urltokenencode.aspx)。 

注意︰ 如果您需要搜索或筛选器上的键值，您将需要同样对您的请求中使用的键进行编码，使您的请求匹配搜索索引中存储的已编码的值。


##容忍失败某些文档编制索引##

默认情况下，Azure 搜索索引器将停止索引一旦甚至为单个文档无法编制索引。 这取决于您的方案中，您可以选择容忍某些故障 （例如，如果反复重新索引整个数据源）。 Azure 搜索提供了两个索引器参数来精细调整此行为︰ 

- **maxFailedItems**︰ 索引索引器执行操作均被视为失败之前可能会失败的项目数。 默认值为 0。
- **maxFailedItemsPerBatch**︰ 可能失败之前索引器执行操作均被视为失败索引在单个批处理中的项的数目。 默认值为 0。

通过指定一个或两个这些参数创建或更新您的索引时，可以随时更改这些值︰

    PUT https://[service name].search.windows.net/indexers/myindexer?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]
    {
        "dataSourceName" : "mydatasource",
        "targetIndexName" : "myindex",
        "parameters" : { "maxFailedItems" : 10, "maxFailedItemsPerBatch" : 5 }
    }

即使您选择容忍一些故障，[获取索引器状态 API](https://msdn.microsoft.com/library/azure/dn946884.aspx)将返回失败的文档有关的信息。

这就是现在。 如果您有任何想法或建议未来内容的想法，我们使用 #AzureSearch hashtag，tweet 或提交您的意见我们[UserVoice 页](http://feedback.azure.com/forums/263029-azure-search)上。    
 