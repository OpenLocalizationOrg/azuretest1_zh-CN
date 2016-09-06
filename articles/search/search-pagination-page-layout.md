---
ms.openlocfilehash: 2323b644cd68b2c0f0840b1944698dc5f72213dc
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="如何在 Azure 搜索的网页搜索结果" 
    description="在 Azure 搜索中分页" 
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

#如何在 Azure 搜索的网页搜索结果#

本文介绍如何使用 Azure 搜索服务 REST API 来实现标准的搜索结果页面，例如总计数、 文档检索、 排序顺序和导航元素提供了指南。
 
在下面提到的每种情况下，通过[搜索文档](http://msdn.microsoft.com/library/azure/dn798927.aspx)请求发送到 Azure 搜索服务指定提供数据或信息到搜索结果页面的页面相关的选项。 请求包含 GET 命令、 路径和查询参数，通知内容请求的服务和如何制定响应。

> [AZURE.NOTE] 有效的请求包括大量的元素，例如服务 URL 和路径，HTTP 谓词， `api-version`，依此类推。 为简洁起见，我们修整以突出显示只与分页相关的语法的示例。 请[Azure 搜索服务 REST API](http://msdn.microsoft.com/library/azure/dn798935.aspx)文档有关请求语法的详细信息，参阅。

## 总点击量和页计数 ##

显示从一个查询，返回的结果的总数和在较小的块，然后返回这些结果是几乎所有搜索页的基础。

![][1]
 
在 Azure 搜索，使用`$count`， `$top`，和`$skip`参数中返回这些值。 下面的示例演示示例请求的总访问量、 作为返回`@OData.count`:

        GET /indexes/onlineCatalog/docs?$count=true

检索文档中的第 15 组，同时显示的总点击率，从第一页开始︰

        GET /indexes/onlineCatalog/docs?search=*$top=15&$skip=0&$count=true

同时对结果进行分页需要`$top`和`$skip`，其中`$top`指定在批处理中，返回的邮件数和`$skip`指定要跳过的邮件数。 在以下示例中，每页显示的下一步 15 项，由中增量的跳转`$skip`参数。

        GET /indexes/onlineCatalog/docs?search=*$top=15&$skip=0&$count=true

        GET /indexes/onlineCatalog/docs?search=*$top=15&$skip=15&$count=true

        GET /indexes/onlineCatalog/docs?search=*$top=15&$skip=30&$count=true

## 布局  ##

在搜索结果页上，您可能想要显示的缩略图图像、 字段的子集和完整的产品页面的链接。

 ![][2]
 
在 Azure 的搜索，您将使用`$select`和查找命令来实现这种体验。

要返回的平铺布局字段的子集︰

        GET /indexes/ onlineCatalog/docs?search=*&$select=productName,imageFile,description,price,rating 

图像和媒体文件不是可直接搜索，应存储在另一个存储平台，如 Azure Blob 存储，以降低成本。 在索引和文档中，定义存储外部内容的 URL 地址的字段。 然后可以使用字段中为图像引用。 图像的 URL，应该在文档中。

若要检索产品描述页面的**onClick**事件，使用[查找文档](http://msdn.microsoft.com/library/azure/dn798929.aspx)的文档检索密钥传递。 项数据类型是`Edm.String`。 在此示例中，它是*246810*。 
   
        GET /indexes/onlineCatalog/docs/246810

## 排序依据相关性、 分级或价格 ##

排序次序通常默认为关联性，但常见的替代排序订单随时可用以使客户可以快速重新现有结果安排到不同的排名顺序。

 ![][3]

在 Azure 搜索，排序基于`$orderby`表达式，作为被编入索引的所有字段 `"Sortable": true.`

相关性是强烈与评分配置文件相关联。 您可以使用默认评分这依赖于文本分析和统计排名顺序的所有结果，以更高的分数将有更多的或更强的匹配的文档搜索词。

替换排序顺序是通常与生成排序的方法对回调的**onClick**事件。 例如，对于此页面元素︰

 ![][4]

将创建一个接受所选的排序选项作为输入，并返回与该选项相关联的条件为一个有序列的表的方法。

 ![][5]
 
> [AZURE.NOTE] 尽管默认评分是不足，而很多情况下，我们建议相关性改为基于计分的自定义配置文件。 计分的自定义配置文件提供了提升项目对您的业务更有益的方法。 有关详细信息，请参阅[添加评分的配置文件](http://msdn.microsoft.com/library/azure/dn798928.aspx)。 

## 多面导航 ##

搜索导航是页面的常见结果页，通常位于两侧或顶部上。 在 Azure 搜索多面导航提供自我定向的搜索基于预定义的筛选器。 有关详细信息，请参见[在 Azure 搜索的多面导航](search-faceted-navigation.md)。

## 在页级别的筛选器 ##

如果您的解决方案设计包括特定类型的内容 （例如，在线零售应用程序已在页面的顶部列出的部门） 的专用的搜索页面，您可以插入旁边**onClick**事件处于 prefiltered 状态打开页面筛选器表达式。 

您可以发送带有或不带搜索表达式的筛选器。 例如，以下请求将筛选品牌名称，返回那些与之匹配的文档。

        GET /indexes/onlineCatalog/docs?$filter=brandname eq ‘Microsoft’ and category eq ‘Games’

的详细信息查看[搜索文档 (Azure 搜索 API)](http://msdn.microsoft.com/library/azure/dn798927.aspx) `$filter`表达式。

## 请参见 ##

- [REST API，azure 搜索服务](http://msdn.microsoft.com/library/azure/dn798935.aspx)
- [索引操作](http://msdn.microsoft.com/library/azure/dn798918.aspx)
- [文档操作](http://msdn.microsoft.com/library/azure/dn800962.aspx)
- [视频和 Azure 搜索有关教程](http://msdn.microsoft.com/library/azure/dn818681.aspx)
- [在 Azure 搜索多面导航](search-faceted-navigation.md)


<!--Image references-->
[1]: ./media/search-pagination-page-layout/Pages-1-Viewing1ofNResults.PNG
[2]: ./media/search-pagination-page-layout/Pages-2-Tiled.PNG
[3]: ./media/search-pagination-page-layout/Pages-3-SortBy.png
[4]: ./media/search-pagination-page-layout/Pages-4-SortbyRelevance.png
[5]: ./media/search-pagination-page-layout/Pages-5-BuildSort.png 