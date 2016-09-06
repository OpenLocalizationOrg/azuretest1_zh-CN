---
ms.openlocfilehash: c370e6890cbecd985db402f4ce36e62aa3bd0aa9
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="从预览的 api 版本转换 = 2014年*到 api 版本 = 2015年*" 
    description="了解重大的更改，以及如何迁移代码写入对 2014年-07-31-预览或 2014年 10-20 预览到 Azure 搜索 api 版本 = 2015年-02-28。" 
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

#从预览的 api 版本转换 = 2014年*到 api 版本 = 2015年*#

下面的指南是为客户构建在 Azure 搜索预览版本的自定义应用程序，并要立即迁移到正式上市发行，2015年-02-28。

作为预览客户，您可能使用这些较旧的预览版本之一︰

- [2014-07-31-预览](search-api-2014-07-31-preview.md)
- [2014-10-20 的预览](search-api-2014-10-20-preview.md)

现在，Azure 搜索是普遍适用的我们鼓励过渡到较新的版本︰ 2015年-02-28 是 Azure 搜索正式上市发行的官方 API 版本。 在[MSDN](https://msdn.microsoft.com/library/azure/dn798933.aspx )上介绍了该版本。

我们也推出的下一步的预览版本， [2015年-02-28-预览](search-api-2015-02-28-preview.md)，引入仍在开发中的功能。 您可以预览 API 通过[Azure 搜索论坛](https://social.msdn.microsoft.com/forums/azure/home?forum=azuresearch )或我们的[反馈页面](http://feedback.azure.com/forums/263029-azure-search )上提供反馈。

###迁移的核对清单###

- 检查以确定是否会影响您的解决方案的重大更改。
- 凹凸的 API 版本为`2015-02-28`的锁定版本。 此版本是在 SLA 下。 如果您遇到问题，可以更快地解决。
- 生成、 测试部署。 您应该有 100%的搜索行为，除了重大更改的奇偶校验。
- 部署到生产环境。
- 对将来的功能采用的新功能进行评估。 凹凸再次到 2015年-02-28-预览如果您想要测试驱动器的 Microsoft 自然语言处理器或`morelikethis`。

##重大更改 api 版本 = 2015年 *##

最初版本的 api 包括自动完成或提前键入建议功能。 虽然有用，但只有有限不仅仅是前缀匹配前, 几个字符在搜索中搜索项，匹配字段中的其他地方没有支持。 实现了一个名为的布尔属性`suggestions`，您将设置为`true`如果您想要启用对特定字段匹配的前缀。

现在此原始实现不取代新建议使用`Suggesters`构造中提供中缀和模糊匹配[索引](https://msdn.microsoft.com/library/azure/dn798941.aspx)功能定义。 顾名思义，缀和模糊匹配提供了更广的匹配功能。 中缀匹配包含前缀，在于它仍匹配的开头字符，而扩展了匹配包含该字符串的其余部分。 

我们选择停止以前的实现 （布尔值属性），意味着它完全不可的 2015年版本与不向后兼容性，以避免无意成才通过构建新的解决方案的客户之一。 如果您可以使用两种`2015-02-28`或`2015-02-28-Preview`将需要使用新`Suggesters`结构以使提前键入查询。

##端口的现有代码##

建议属性是唯一重大更改。 如果您没有使用此属性可以影响`api-version`从`2014-07-31-Preview`或`2014-10-20-Preview`与`2015-02-28`，然后重新生成和重新部署。 应用程序可以像以前一样工作。 

实施建议的自定义应用程序应执行以下操作︰

1. 更新所有 NuGet 程序包。
1. 凹凸的 api 版本`2015-02-28`。 如果您使用下面的代码示例，api 版本是**AzureSearchHelper**类中。
1. 删除`Suggestions={true | false}`从 JSON 架构定义索引属性。
1. 有关索引的底部添加一个构造`Suggesters`（如上图所示的[后](#after)一节）。
1. 请验证您可以发布到您的服务 （您可能需要重命名索引以避免命名冲突）。
1. 重新生成解决方案并将部署到测试环境。
1. 运行所有的测试用例以确保解决方案行为与预期相同。
1. 部署到生产环境。

[在 codeplex 上的艾德示例](https://azuresearchadventureworksdemo.codeplex.com/)中的代码示例包含原始`Suggestions`实现。 您可能想要的示例代码使用此示例练习代码迁移到。 

在下面的部分中，我们将介绍建议的[之前](#before)和[之后](#after)实现。 可以替换的[后](#after)一节中的版本为**CreateCatalogIndex()**方法，然后生成和部署解决方案尝试新的功能。

<a name="before"></a>
###之前###

正如您所看到的`Suggestions`是将每个字段设置为的布尔值属性。 删除所有的`Suggestions`属性。

        private static void CreateCatalogIndex()
        {
            var definition = new 
            {
                Name = "catalog",
                Fields = new[] 
                { 
                    new { Name = "productID",        Type = "Edm.String",         Key = true,  Searchable = false, Filterable = false, Sortable = false, Facetable = false, Retrievable = true,  Suggestions = false },
                    new { Name = "name",             Type = "Edm.String",         Key = false, Searchable = true,  Filterable = false, Sortable = true,  Facetable = false, Retrievable = true,  Suggestions = true  },
                    new { Name = "productNumber",    Type = "Edm.String",         Key = false, Searchable = true,  Filterable = false, Sortable = false, Facetable = false, Retrievable = true,  Suggestions = true  },
                    new { Name = "color",            Type = "Edm.String",         Key = false, Searchable = true,  Filterable = true,  Sortable = true,  Facetable = true,  Retrievable = true,  Suggestions = false },
                    new { Name = "standardCost",     Type = "Edm.Double",         Key = false, Searchable = false, Filterable = false, Sortable = false, Facetable = false, Retrievable = true,  Suggestions = false },
                    new { Name = "listPrice",        Type = "Edm.Double",         Key = false, Searchable = false, Filterable = true,  Sortable = true,  Facetable = true,  Retrievable = true,  Suggestions = false },
                    new { Name = "size",             Type = "Edm.String",         Key = false, Searchable = true,  Filterable = true,  Sortable = true,  Facetable = true,  Retrievable = true,  Suggestions = false },
                    new { Name = "weight",           Type = "Edm.Double",         Key = false, Searchable = false, Filterable = true,  Sortable = false, Facetable = true,  Retrievable = true,  Suggestions = false },
                    new { Name = "sellStartDate",    Type = "Edm.DateTimeOffset", Key = false, Searchable = false, Filterable = true,  Sortable = false, Facetable = false, Retrievable = false, Suggestions = false },
                    new { Name = "sellEndDate",      Type = "Edm.DateTimeOffset", Key = false, Searchable = false, Filterable = true,  Sortable = false, Facetable = false, Retrievable = false, Suggestions = false },
                    new { Name = "discontinuedDate", Type = "Edm.DateTimeOffset", Key = false, Searchable = false, Filterable = true,  Sortable = false, Facetable = false, Retrievable = true,  Suggestions = false },
                    new { Name = "categoryName",     Type = "Edm.String",         Key = false, Searchable = true,  Filterable = true,  Sortable = false, Facetable = true,  Retrievable = true,  Suggestions = true  },
                    new { Name = "modelName",        Type = "Edm.String",         Key = false, Searchable = true,  Filterable = true,  Sortable = false, Facetable = true,  Retrievable = true,  Suggestions = true  },
                    new { Name = "description",      Type = "Edm.String",         Key = false, Searchable = true,  Filterable = true,  Sortable = false, Facetable = false, Retrievable = true,  Suggestions = false }
                }
            };

<a name="after"></a>
###之后###

已迁移的架构定义省略`Suggestions`属性，并添加`Suggesters`底部构造。

        private static void CreateCatalogIndex()
        {
            var definition = new 
            {
                Name = "catalog",
                Fields = new[] 
                { 
                    new { Name = "productID",        Type = "Edm.String",         Key = true,  Searchable = false, Filterable = false, Sortable = false, Facetable = false, Retrievable = true },
                    new { Name = "name",             Type = "Edm.String",         Key = false, Searchable = true,  Filterable = false, Sortable = true,  Facetable = false, Retrievable = true },
                    new { Name = "productNumber",    Type = "Edm.String",         Key = false, Searchable = true,  Filterable = false, Sortable = false, Facetable = false, Retrievable = true },
                    new { Name = "color",            Type = "Edm.String",         Key = false, Searchable = true,  Filterable = true,  Sortable = true,  Facetable = true,  Retrievable = true },
                    new { Name = "standardCost",     Type = "Edm.Double",         Key = false, Searchable = false, Filterable = false, Sortable = false, Facetable = false, Retrievable = true },
                    new { Name = "listPrice",        Type = "Edm.Double",         Key = false, Searchable = false, Filterable = true,  Sortable = true,  Facetable = true,  Retrievable = true },
                    new { Name = "size",             Type = "Edm.String",         Key = false, Searchable = true,  Filterable = true,  Sortable = true,  Facetable = true,  Retrievable = true },
                    new { Name = "weight",           Type = "Edm.Double",         Key = false, Searchable = false, Filterable = true,  Sortable = false, Facetable = true,  Retrievable = true },
                    new { Name = "sellStartDate",    Type = "Edm.DateTimeOffset", Key = false, Searchable = false, Filterable = true,  Sortable = false, Facetable = false, Retrievable = false },
                    new { Name = "sellEndDate",      Type = "Edm.DateTimeOffset", Key = false, Searchable = false, Filterable = true,  Sortable = false, Facetable = false, Retrievable = false },
                    new { Name = "discontinuedDate", Type = "Edm.DateTimeOffset", Key = false, Searchable = false, Filterable = true,  Sortable = false, Facetable = false, Retrievable = true },
                    new { Name = "categoryName",     Type = "Edm.String",         Key = false, Searchable = true,  Filterable = true,  Sortable = false, Facetable = true,  Retrievable = true },
                    new { Name = "modelName",        Type = "Edm.String",         Key = false, Searchable = true,  Filterable = true,  Sortable = false, Facetable = true,  Retrievable = true },
                    new { Name = "description",      Type = "Edm.String",         Key = false, Searchable = true,  Filterable = true,  Sortable = false, Facetable = false, Retrievable = true }
                },
                suggesters = new[]
                {
                new {
                    name = "sg",
                    searchMode = "analyzingInfixMatching",
                    sourceFields = new []{"name", "productNumber", "categoryName", "modelName", "description"}
                    }
                 }
            };

##评估新的功能和方法##

已经移植您的解决方案，并验证它按预期运行之后，可以使用这些链接可阅读有关新功能。

- [Azure 搜索是上市 （日志）](http://go.microsoft.com/fwlink/p/?LinkId=528211 )
- [什么是 Azure 搜索最新的更新新手](search-latest-updates.md)
- [什么是 Azure 搜索？](search-what-is-azure-search.md)

##获取帮助##

API 版本`2015-02-28`在 SLA。 使用[此网页](../support/options/)上的支持选项和链接文件的支持票。

 