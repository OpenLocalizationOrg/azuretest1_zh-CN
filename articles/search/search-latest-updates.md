---
ms.openlocfilehash: 597be06a575f849cabf35d6444d9b09c0def6a84
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="什么是 Azure 搜索最新的更新新手 |Microsoft Azure" 
    description="Azure 搜索描述服务最新的更新的发行说明" 
    services="search" 
    documentationCenter="" 
    authors="HeidiSteen" 
    manager="mblythe" 
    editor=""/>

<tags 
    ms.service="search" 
    ms.devlang="rest-api" 
    ms.workload="search" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.date="07/08/2015" 
    ms.author="heidist"/>

#什么是 Azure 搜索最新的更新新手#

Azure 搜索现已可用， [2015年-02-28 API 版本](https://msdn.microsoft.com/library/azure/dn798935.aspx)的受支持的配置提供 99.9%的可用性服务级别协议 (SLA)。

##版本控制和回滚功能是如何出

功能是通过[REST API](https://msdn.microsoft.com/library/azure/dn798935.aspx)、 [.NET SDK](http://go.microsoft.com/fwlink/?LinkId=528216)或[Azure 门户](https://portal.azure.com)中的服务仪表板发布单独或联合。

.NET 库和 REST Api 都有多个版本。 当我们推出新的功能，旧 Api 保持运营。 您可以访问[搜索服务版本控制](https://msdn.microsoft.com/library/azure/dn864560.aspx)，更多地了解我们的版本控制策略。


##.NET SDK 0.10.0-preview

这是.NET 客户端库，Microsoft.Azure.Search.dll 的第二个迭代。 此版本增加了对创建、 管理和使用通过.NET 类的索引器的支持。 此外，对于 Azure SQL 索引器，没有索引地理点新的支持。

- [索引器类](https://msdn.microsoft.com/library/azure/microsoft.azure.search.models.indexer.aspx)
- [数据源类](https://msdn.microsoft.com/library/azure/microsoft.azure.search.models.datasource.aspx)

##.NET SDK 0.9.6-preview
**3 月 5日日发布︰ 2015**

这是首次发布.NET SDK 的 Azure 搜索。 它包括客户端库，Microsoft.Azure.Search.dll，具有两个命名空间︰

- [Microsoft.Azure.Search](https://msdn.microsoft.com/library/azure/microsoft.azure.search.aspx)
- [Microsoft.Azure.Search.Models](https://msdn.microsoft.com/library/azure/microsoft.azure.search.models.aspx)

排除︰

- [索引器](http://go.microsoft.com/fwlink/p/?LinkId=528173)（此功能将不再被排除在 0.10.0-preview 版本中）
- [管理 REST API，](https://msdn.microsoft.com/library/azure/dn832684.aspx)
- [2015-02-28-预览](search-api-2015-02-28-Preview.md)功能 (只可预览功能目前，包括 Microsoft 自然语言处理器和`moreLikeThis`)。

请访问[如何使用.NET 中的 Azure 搜索](http://go.microsoft.com/fwlink/p/?LinkId=528088)指南安装和使用 SDK。

##Api 版本 2015年-02-28-预览
**4 月 22 日发布︰ 2015**

- 在实际的字段名称是不同的外部数据库和 Azure 搜索索引之间时，现在支持 fieldMapping 结构的索引器提供字段赋值。 对于[索引器](search-api-indexers-2015-02-28-Preview.md)，请参阅`2015-02-28-preview`版本的索引文档。.

- 此外在此预览更新时，索引器现在支持字段类型转换，以便您可以假定源字段表示一个 JSON 数组到在搜索索引中，字符串集合字段映射 SQL 表中的字符串字段。

**3 月 5日日发布︰ 2015**

- [Microsoft 自然语言处理器](search-api-2015-02-28-Preview.md)将支持更多的语言和办公室和 Bing 所支持的所有语言的大规模词根。

- [moreLikeThis =](search-api-2015-02-28-Preview.md) exclusive 的相互为搜索参数， `search=`，来触发一个候选的查询执行路径。 而不是全文搜索的`search=`基于搜索条件输入，`moreLikeThis=`发现通过比较它们的可搜索字段类似于给定文档的文档。

##Api 版本 2015年-02-28
**3 月 5日日发布︰ 2015**

- [索引器](http://go.microsoft.com/fwlink/p/?LinkID=528210)是一种新的功能，极大地简化了 SQL Azure 数据库、 Azure DocumentDB 和在 Azure Vm 上的 SQL Server 上的数据源进行索引。

- [Suggesters](https://msdn.microsoft.com/library/azure/dn798936.aspx)会替换以前 （它仅匹配的前缀） 的实现的更有限，提前键入查询支持添加匹配的中缀的支持。 此实现在一个术语，可以找到任意位置匹配并且还支持模糊匹配。

- [标记提高](http://go.microsoft.com/fwlink/p/?LinkId=528212)使评分配置文件的新方案。 特别是，它利用持久的数据 （如购物首选参数），这样就可以增加根据个性化信息的单个用户的搜索结果。 

请访问[Azure 搜索是现在上市](http://go.microsoft.com/fwlink/p/?LinkId=528211)服务公告在 Azure 讨论所有这些功能的博客上。

##Api 版本 2014年 10-20 预览
**发布︰ 2014 11 月，2015 年 1 月**

- [Lucene 语言分析器](search-api-2014-10-20-preview.md)已添加为自定义语言分析器随 Lucene 提供多语言支持。 

- 用于生成索引，包括评分在[Azure 管理门户](https://portal.azure.com)配置文件中，引入了工具支持。

##Api 版本 2014年-07-31-预览
**8 月 21 日发布︰ 2014**

此版本是 Azure 搜索，提供下列核心功能的公共预览版本︰

- REST API，用于索引和文档的操作。 此 API 版本大部分是完整地保留在 2015年-02-28。 版本的文档`2014-07-31-Preview`可以找到在[Azure 搜索服务 REST API 版本 2014年-07-31](search-api-2014-07-31-preview.md)。

- 评分的配置文件进行优化搜索结果。 计分的配置文件中添加用来计算搜索成绩的条件。 有关此功能的文档可以位于[Azure 搜索服务评分配置文件其余部分的 API 版本 2014年-07-31](search-api-scoring-profiles-2014-07-31-preview.md)。

- 地理空间支持已从开始时，通过提供可用`Edm.GeographyPoint`数据类型自成立以来已 Azure 搜索的一部分。

- 资源调配在[Azure 管理门户](https://portal.azure.com )的预览版本。 Azure 搜索了几只已在新的门户中可用的服务之一。

##管理 api 版本 2015年-02-28
**3 月 5日日发布︰ 2015**

[管理 REST API](https://msdn.microsoft.com/library/azure/dn832684.aspx)将标记属于上市发布 Azure 搜索管理 API 的第一个版本。 有早期预览和这一没有功能差异。

##管理的 api 版本 2014年-07-31-预览
**发布︰ 2014年 10 月**

添加[管理 REST API](search-management-api-2014-07-31-preview.md)的预览版本是为了以编程方式支持服务管理。 它是独立于服务 REST API 版本控制。


 