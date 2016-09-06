---
ms.openlocfilehash: 8f5d35bd55b182483c73ec1c37a0e33221dd912e
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="尝试使用 Azure 搜索 Azure 应用程序服务" 
   description="请尝试 Azure 搜索释放，最多为一小时，使用 TryAzureAppService 模板。" 
   services="search" 
   documentationCenter="" 
   authors="HeidiSteen" 
   manager="mblythe" 
   editor=""/>

<tags
   ms.service="search"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="search" 
   ms.date="07/13/2015"
   ms.author="heidist"/>

# 尝试使用 Azure 搜索 Azure 应用程序服务

[尝试 Azure 应用程序服务](https://tryappservice.azure.com/)是一种新完全免费的方式来测试所选的 Azure 服务，包括 Azure 搜索，最多一小时与没有注册的 Azure 订阅所需。 

该站点为您提供了几个可供选择的模板。 选择包含 Azure 搜索的 ASP.NET 模板时，您可以获得一个小时的全功能的 web 站点，您所选的服务为后盾的访问。 您将无法更新或删除任何数据由 Azure 搜索 — 但可以执行查询，并使形状的用户体验的任意数量的代码更改。 如果在会话过期之前就进行探索，可以始终重新开始与另一会话，或如果您的目标是创建或直接加载索引为试用版或完整订阅移动上。

在[尝试 Azure 应用程序服务](https://tryappservice.azure.com/)网站上，Azure 搜索是 Web 应用程序模板 – 在 Azure 平台上提供丰富、 全文本搜索体验和搜索中心功能仅适用于该服务的一大堆的一部分。

尽管其他 Azure 服务如 SQL 数据库提供全文检索服务类似 Azure 搜索可以调节控制、 分页和计数，点击高亮，自动完成查询建议，自然语言就会支持，多面导航、 筛选等。 如我们[样本](https://github.com/AzureSearch)中的几个演示，就可以开发一个全功能基于搜索的应用程序使用只是 Azure 搜索和 ASP.NET。

作为产品，您将使用 Azure 搜索服务只读 —[尝试 Azure 应用程序服务](https://tryappservice.azure.com/)的一部分，这意味着您将需要使用会话中提供搜索 corpus。 您不能上载或使用您自己的索引或数据。 您将使用的数据取自[美国地质调查 (USGS)]()，在美国组成的大约 300 万行的界标、 历史网站、 建筑物和地标的其他功能。

为帮助您充分利用您一个小时的讨论会，按照以下说明将指导您完成查询和代码。 

前顺利，可能要花几分钟查看代码、 服务和可搜索的数据有关的几个要点。 有一点背景可能会证明非常有用，如果你不已经熟悉 Azure 搜索。 

##关于代码和 Azure 搜索

Azure 搜索是 service 加上数据[PaaS](https://en.wikipedia.org/wiki/Platform_as_a_service)服务的功能，组成完全托管的搜索服务，再加上搜索数据上载使用不受约束的 Azure 搜索实例时 （即当您不使用尝试 Azure 应用程序服务选项）。

在搜索操作中使用的数据都存储在 Azure，数据接近操作可以较低的延迟和一致的搜索行为确保您搜索服务。 目前尚不支持脱机或远程存储的可搜索的数据。 到这位进一步深化︰

- 搜索数据存储在管理 Azure 搜索，填充文档的可搜索项的每个文档的索引。 
- 从一个数据集，事先准备好的由您要包括的域中非常有用的搜索操作的上下文中加载大多数索引。 
- 架构定义索引是用户定义的并将指定的可搜索字段，可用于在筛选器表达式，并像评分调整结果的配置文件的构造的非可搜索字段。
- 可以通过索引器 （SQL Azure 数据库或 Azure DocumentDB 只支持），自动加载或通过 Azure 搜索 Api 被推到搜索索引数据。 当您使用该 API 时，可以将数据推从任何数据源，只要是以 JSON 格式。

[尝试 Azure 应用程序服务](https://tryappservice.azure.com/)选项，ASP.NET + Azure 搜索网站模板中提供了 Web 应用程序，可在 Visual Studio 联机 （可用作一小时会话的一部分） 中修改的源代码。 没有单独开发工具需要查看或更改代码。

使用[Azure 搜索.NET 客户端库](https://msdn.microsoft.com/library/dn951165.aspx)对索引执行查询时，提供多面导航，显示计数并在 web 页中的搜索结果在 C# 中，编写代码。

使用其他代码，不包括在该模板中，生成并加载 USGS 搜索索引。 由于服务是只读的需要写访问权限的所有操作都必须提前完成。 您可以查看[架构的副本](#schema)用于生成的架构，在这篇文章的末尾。

##入门

如果您尚未启动 1 小时会话，则请按照下列步骤着手。

1. 转到[https://tryappservice.azure.com](https://tryappservice.azure.com/) ，然后向下滚动到选择的**Web 应用程序**。 
2. 单击**下一步**。
3. 选择的**ASP.NET + Azure 搜索站点**模板。

    ![][1]

4. 单击**创建**。
5. 选择登录方法并提供用户名和密码。

    ![][2]

6. 配置此站点，请稍候。 准备就绪后，您将看到类似于此的一页。 每一页显示正在运行的时钟，以便您总是知道您已离开了多长时间。

    ![][3]

7. 选择**使用 Visual Studio 在线编辑**查看解决方案和浏览站点。 
9. 在 Visual Studio 在线，展开顶部的页面，会话选项，然后单击**浏览 Web 站点**。

    ![][4]

10. 您应该看到入门 Azure 搜索 web 站点的页面。 单击**开始**按钮以打开该网站。

    ![][5]

11. ASP.NET web 站点打开浏览器，提供了一个搜索框中。 输入一个熟悉的术语来搜索，如*Yellowstone*或*突出*像著名山地。 从熟悉的陆标开始便于评估结果。

    ![][6]


##首先要执行哪些
由于搜索索引是完全可操作的很好的第一步是尝试执行一些查询。 Azure 搜索支持所有标准搜索运算符 (+，-，|)，文本匹配、 通配符 （*） 和运算符优先级的引号。 您可以检查查询语法引用运算符的完整列表。

- 通过添加一个星号通配符搜索从开始 (`*`)。 这会告诉您在索引中找到多少文档︰ 2,262,578。
- 接下来，输入"Yellowstone"，然后添加"+ 中心"，"+ 构建"和"-ND"逐渐缩小范围到只 Yellowstone 访问者中心，但不包括那些在北达科他州的搜索结果︰ `Yellowstone +center +building -ND`。  
- 请尝试结合优先级的运算符和字符串匹配的搜索短语︰ `statue+(park+MT)`。 您应该看到类似于下面的屏幕截图的结果。 请注意的方面类别出现在功能类，提供自我定向筛选通过多面导航，大多数搜索应用程序中常见的一种功能。

    ![][7]

继续学习？ 让我们来更改几行代码以查看全文本搜索操作的影响。

##更改 searchMode.All

Azure 搜索具有可配置的**searchMode**属性，您可以使用它来控制搜索运算符的行为。 此属性的有效值为`Any`（默认值） 或`All`。 有关设置这些选项的更多指导，请参阅[简单查询语法](https://msdn.microsoft.com/library/dn798920.aspx)。

- **searchMode.Any**规定任何匹配的一个搜索词足以在搜索结果中包括的项。 如果您的搜索短语是`Yellowstone visitor center`，则包含这些条款中的任何任意文档包括在搜索结果中。 这种模式偏重朝*召回*。
- **searchModel.All**，在此示例中，使用要求的所有指定的条款包含在文档中。 这种模式是更严格比**searchMode.Any**，但如果在召回上倾向于*精度*，可能是您的应用程序的正确选择。 

> [AZURE.NOTE] **searchMode.Any**适合当构造查询主要包括短语，与最少使用的运算符。 一般的经验法则是，搜索使用者应用程序，如电子商务网站的人，往往使用不仅仅是术语，而搜索的内容或数据的人会更容易在搜索短语中包含运算符。 如果您认为搜索可能包含运算符，尤其是`NOT (-)`运算符，以**searchModel.All**开头。 与此相反的是，您的其他选择， **searchMode.Any**将`OR``NOT`其他搜索条件，从而可以显著扩大而不是修剪结果的运算符。 下面的示例可以帮助您理解的差异。

在此任务中，您将更改**searchMode** ，并比较基于模式的搜索结果。

1. 打开包含示例应用程序的浏览器窗口，请选择**连接到 Visual Studio 联机**。

    ![][8]

2. 打开**Search.cshtml**，发现`searchMode.All`在行 39，然后将其更改为`searchMode.Any`。

    ![][9]

3. 在向右跳栏中，单击**运行**。

    ![][10]
 
在重新生成的应用程序窗口中，输入搜索条件，如使用之前， `Yellowstone +center +building -ND`，并比较之前和之后对**searchMode**的更改的结果。

它是相当大的差异。 而不是七个搜索结果中，您将获得超过 200 万。 

   ![][11]
 
正在观察的行为是由于包含`NOT`运算符 (在这种情况下，"-ND")，这是*或有*而不是*AND 有*如果**searchMode**设置为`Any`。

给定此配置中，搜索结果的搜索术语包括点击`Yellowstone`， `center`，和`building`，但也是每个文档`NOT North Dakota`。 因为仅有 13,081 文档包含短语`North Dakota`，几乎所有的数据集返回。

不可否认，这也许是不太可能的方案，但是它阐释了搜索短语，其中包括对**searchMode**的影响`NOT`运算符，因此可用于了解行为出现的原因以及如何更改它，如果您不希望如此。

若要继续本教程，还原回其原始值的**searchMode** (设置为`All`39 行)、 运行程序，并重新生成应用程序用于其余的任务。
 
##添加为华盛顿州的全局筛选器

通常情况下，如果您想要搜索一个可用数据的子集，则设置该筛选器在数据源导入数据时。 出于学习目的，使用只读数据，我们将在我们的应用程序返回只包括华盛顿州的文档中设置筛选器。

1. 打开 Search.cshtml，找到**SearchParameters**代码块 （36 行上开始） 并添加注释行以及筛选器。

        var sp = new SearchParameters
        {
            //Add a filter for _Washington State
            Filter = "STATE_ALPHA eq 'WA'",
            // Specify whether Any or All of the search terms 
            SearchMode = SearchMode.All,
            // Include a count of results in the query result
            IncludeTotalResultCount = true,
            // Return an aggregated count of feature classes based on the specified query
            Facets = new[] { "FEATURE_CLASS" },
            // Limit the results to 20 documents
            Top = 20
        };


筛选器使用 OData 语法指定和经常与多面导航一起使用或包含在要限制查询的查询字符串。 [OData 筛选器语法](https://msdn.microsoft.com/library/azure/dn798921.aspx)的详细信息，请参见

2. 单击**运行**。

3. 打开应用程序。

4. 键入通配符 （*） 来返回计数。 请注意，结果现在均限于 42,411 项，是所有华盛顿州地理特征的所有文档。

   ![][12]

##添加点击突出显示

现在，进行一系列的单行代码更改，您可能想要尝试更深入需要在多个地方更改代码的修改。 在当前会话中可以直接通过 Search.cshtml 文件粘贴以下版本的**Search.cshtml** 。 

下面的代码添加突出显示。 注意到在[SearchParameters](https://msdn.microsoft.com/library/microsoft.azure.search.models.searchparameters_properties.aspx)中声明新的属性。 还有一个新函数，以循环访问集合的结果需要突出显示的文档加上呈现结果的 HTML。 

运行此代码示例，请指定字段中包含相应的匹配项的搜索词输入将以粗体突出显示。

   ![][14]

您可能想要保存一份原始的**Search.cshtml**文件，查看了两个版本进行比较。

> [AZURE.NOTE] 注释已被截掉，以减少文件的大小。
 
    @using System.Collections.Specialized
    @using System.Configuration
    @using Microsoft.Azure.Search
    @using Microsoft.Azure.Search.Models
    
    @{
    Layout = "~/_SiteLayout.cshtml";
    }
    
    @{
    // This modified search.cshtml file adds hit-highlighting
    
    string searchText = Request.Unvalidated["q"];
    string filterExpression = Request.Unvalidated["filter"];
    
    DocumentSearchResponse response = null;
    if (!string.IsNullOrEmpty(searchText))
    {
    searchText = searchText.Trim();
    
    // Retrieve search service name and an api key from configuration
    string searchServiceName = ConfigurationManager.AppSettings["SearchServiceName"];
    string apiKey = ConfigurationManager.AppSettings["SearchServiceApiKey"];
    
    var searchClient = new SearchServiceClient(searchServiceName, new SearchCredentials(apiKey));
    var indexClient = searchClient.Indexes.GetClient("geonames");
    
    // Set the Search parameters used when executing the search request
    var sp = new SearchParameters
    {
    // Specify whether Any or All of the search terms must be matched in order to count the document as a match.
    SearchMode = SearchMode.All,
    // Include a count of results in the query result
    IncludeTotalResultCount = true,
    // Return an aggregated count of feature classes based on the specified query
    Facets = new[] { "FEATURE_CLASS" },
    // Limit the results to 20 documents
    Top = 20,
    // Enable hit-highlighting
    HighlightFields = new[] { "FEATURE_NAME", "DESCRIPTION", "FEATURE_CLASS", "COUNTY_NAME", "STATE_ALPHA" },
    HighlightPreTag = "<b>",
    HighlightPostTag = "</b>",
    };
    
    if (!string.IsNullOrEmpty(filterExpression))
    {
    // Add a search filter that will limit the search terms based on a specified expression.
    // This is often used with facets so that when a user selects a facet, the query is re-executed,
    // but limited based on the chosen facet value to further refine results
    sp.Filter = filterExpression;
    }
    
    // Execute the search query based on the specified search text and search parameters
    response = indexClient.Documents.Search(searchText, sp);
    }
    }
    
    @functions
    {
    private string GetFacetQueryString(string facetKey, string facetValue)
    {
    NameValueCollection queryString = HttpUtility.ParseQueryString(Request.Url.Query);
    queryString["filter"] = string.Format("{0} eq '{1}'", HttpUtility.UrlEncode(facetKey), HttpUtility.UrlEncode(facetValue));
    return queryString.ToString();
    }
    
    private HtmlString RenderHitHighlightedString(SearchResult item, string fieldName)
    {
    if (item.Highlights != null && item.Highlights.ContainsKey(fieldName))
    {
    string highlightedResult = string.Join("...", item.Highlights[fieldName]);
    return new HtmlString(highlightedResult);
    }
    return new HtmlString(item.Document[fieldName].ToString());
    }
    }
    
    <!-- Form to allow user to enter search terms -->
    <form class="form-inline" role="search">
    <div class="form-group">
    <input type="search" name="q" id="q" autocomplete="off" size="80" placeholder="Type something to search, i.e. 'park'." class="form-control" value="@searchText" />
    <button type="submit" id="search" class="btn btn-primary">Search</button>
    </div>
    </form>
    @if (response != null)
    {
    if (response.Count == 0)
    {
    <p style="padding-top:20px">No results found for "@searchText"</p>
    <h3>Suggestions:</h3>
    <ul>
    <li>Ensure words are spelled correctly.</li>
    <li>Try rephrasing keywords or using synonyms.</li>
    <li>Try less specific keywords.</li>
    </ul>
    }
    else
    {
    <div class="col-xs-3 col-md-2">
    <h3>Feature Class</h3>
    <ul class="list-unstyled">
    @if (!string.IsNullOrEmpty(filterExpression))
    {
    <li><a href="?q=@HttpUtility.UrlEncode(searchText)">All</a></li>
    }
    <!-- Cycle through the facet results and show the values and counts -->
    @foreach (var facet in response.Facets["FEATURE_CLASS"])
    {
    <li><a href="?@GetFacetQueryString("FEATURE_CLASS", (string)facet.Value)">@facet.Value (@facet.Count)</a></li>
    }
    </ul>
    </div>
    <div class="col-xs-12 col-sm-6 col-md-10">
    <p style="padding-top:20px">1 - @response.Results.Count of @response.Count results for "@searchText"</p>
    
    <ul class="list-unstyled">
    <!-- Cycle through the search results -->
    @foreach (var item in response.Results)
    {
    <li>
    <h3>@RenderHitHighlightedString(item, "FEATURE_NAME")</h3>
    <p>@RenderHitHighlightedString(item, "DESCRIPTION")</p>
    <p>@RenderHitHighlightedString(item, "FEATURE_CLASS"), elevation: @item.Document["ELEV_IN_M"] meters</p>
    <p>@RenderHitHighlightedString(item, "COUNTY_NAME") County, @RenderHitHighlightedString(item, "STATE_ALPHA")</p>
    <br />
    </li>
    }
    </ul>
    </div>
    }
    }


##下一步行动

使用只读[尝试 Azure 应用程序服务](https://tryappservice.azure.com/)站点中提供的服务，您已经看到的查询语法和全文搜索操作，了解了在 searchMode 和过滤器，并将其添加突出显示您的搜索应用程序中。 作为下一步，可考虑移动到创建和更新索引。 这将添加以下能力︰

- [定义权重配置文件](https://msdn.microsoft.com/library/dn798928.aspx)用于调整搜索分数，以便首先显示高值项。
- 将自动完成或提前键入查询建议添加对用户输入的响应的[定义 Suggesters](https://msdn.microsoft.com/library/mt131377.aspx) 。
- SQL Azure 数据库或 Azure DocumentDB 的数据源时自动更新索引的[定义索引器](https://msdn.microsoft.com/library/dn946891.aspx)。

要执行所有这些任务，您将需要 Azure 的订阅，以便您可以创建并填充索引服务中。 有关如何注册免费试用版的详细信息，请访问[https://azure.microsoft.com/pricing/free-trial](https://azure.microsoft.com/pricing/free-trial/)。

要了解更多有关 Azure 搜索， [http://azure.microsoft.com](http://azure.microsoft.com) ，请访问我们的[文档页中找到](http://azure.microsoft.com/documentation/services/search/)或签出任何数量的[样本和视频](https://msdn.microsoft.com/library/dn818681.aspx)的浏览完整 Azure 搜索功能。

<a name="Schema"></a>
##有关架构

下面的屏幕快照显示了使用本模板中使用的索引创建的架构。
 
   ![][13]

###Schema.json 文件

    {
      "@odata.context": "https://tryappservice.search.windows.net/$metadata#indexes/$entity",
      "name": "geonames",
      "fields": [
    {
      "name": "FEATURE_ID",
      "type": "Edm.String",
      "searchable": false,
      "filterable": false,
      "retrievable": true,
      "sortable": false,
      "facetable": false,
      "key": true,
      "analyzer": null
    },
    {
      "name": "FEATURE_NAME",
      "type": "Edm.String",
      "searchable": true,
      "filterable": true,
      "retrievable": true,
      "sortable": true,
      "facetable": false,
      "key": false,
      "analyzer": null
    },
    {
      "name": "FEATURE_CLASS",
      "type": "Edm.String",
      "searchable": true,
      "filterable": true,
      "retrievable": true,
      "sortable": true,
      "facetable": true,
      "key": false,
      "analyzer": null
    },
    {
      "name": "STATE_ALPHA",
      "type": "Edm.String",
      "searchable": true,
      "filterable": true,
      "retrievable": true,
      "sortable": true,
      "facetable": true,
      "key": false,
      "analyzer": null
    },
    {
      "name": "STATE_NUMERIC",
      "type": "Edm.Int32",
      "searchable": false,
      "filterable": true,
      "retrievable": true,
      "sortable": true,
      "facetable": true,
      "key": false,
      "analyzer": null
    },
    {
      "name": "COUNTY_NAME",
      "type": "Edm.String",
      "searchable": true,
      "filterable": true,
      "retrievable": true,
      "sortable": true,
      "facetable": true,
      "key": false,
      "analyzer": null
    },
    {
      "name": "COUNTY_NUMERIC",
      "type": "Edm.Int32",
      "searchable": false,
      "filterable": true,
      "retrievable": true,
      "sortable": true,
      "facetable": true,
      "key": false,
      "analyzer": null
    },
    {
      "name": "ELEV_IN_M",
      "type": "Edm.Int32",
      "searchable": false,
      "filterable": true,
      "retrievable": true,
      "sortable": true,
      "facetable": true,
      "key": false,
      "analyzer": null
    },
    {
      "name": "ELEV_IN_FT",
      "type": "Edm.Int32",
      "searchable": false,
      "filterable": true,
      "retrievable": true,
      "sortable": true,
      "facetable": true,
      "key": false,
      "analyzer": null
    },
    {
      "name": "MAP_NAME",
      "type": "Edm.String",
      "searchable": true,
      "filterable": true,
      "retrievable": true,
      "sortable": true,
      "facetable": false,
      "key": false,
      "analyzer": null
    },
    {
      "name": "DESCRIPTION",
      "type": "Edm.String",
      "searchable": true,
      "filterable": false,
      "retrievable": true,
      "sortable": false,
      "facetable": false,
      "key": false,
      "analyzer": null
    },
    {
      "name": "HISTORY",
      "type": "Edm.String",
      "searchable": true,
      "filterable": false,
      "retrievable": true,
      "sortable": false,
      "facetable": false,
      "key": false,
      "analyzer": null
    },
    {
      "name": "DATE_CREATED",
      "type": "Edm.DateTimeOffset",
      "searchable": false,
      "filterable": true,
      "retrievable": true,
      "sortable": true,
      "facetable": true,
      "key": false,
      "analyzer": null
    },
    {
      "name": "DATE_EDITED",
      "type": "Edm.DateTimeOffset",
      "searchable": false,
      "filterable": true,
      "retrievable": true,
      "sortable": true,
      "facetable": true,
      "key": false,
      "analyzer": null
    }
      ],
      "scoringProfiles": [
    
      ],
      "defaultScoringProfile": null,
      "corsOptions": {
    "allowedOrigins": [
      "*"
    ],
    "maxAgeInSeconds": 300
      },
      "suggesters": [
    {
      "name": "AUTO_COMPLETE",
      "searchMode": "analyzingInfixMatching",
      "sourceFields": [
    "FEATURE_NAME",
    "FEATURE_CLASS",
    "COUNTY_NAME",
    "DESCRIPTION"
      ]
    }
      ]
    }


<!--Image references-->
[1]: ./media/search-tryappservice/AzSearch-TryAppService-TemplateTile.png
[2]: ./media/search-tryappservice/AzSearch-TryAppService-LoginAccount.png
[3]: ./media/search-tryappservice/AzSearch-TryAppService-AppCreated.png
[4]: ./media/search-tryappservice/AzSearch-TryAppService-BrowseSite.png
[5]: ./media/search-tryappservice/AzSearch-TryAppService-GetStarted.png
[6]: ./media/search-tryappservice/AzSearch-TryAppService-QueryWA.png
[7]: ./media/search-tryappservice/AzSearch-TryAppService-QueryPrecedence.png
[8]: ./media/search-tryappservice/AzSearch-TryAppService-VSOConnect.png
[9]: ./media/search-tryappservice/AzSearch-TryAppService-cSharpSample.png
[10]: ./media/search-tryappservice/AzSearch-TryAppService-RunBtn.png
[11]: ./media/search-tryappservice/AzSearch-TryAppService-searchmodeany.png
[12]: ./media/search-tryappservice/AzSearch-TryAppService-searchmodeWAState.png
[13]: ./media/search-tryappservice/AzSearch-TryAppService-Schema.png
[14]: ./media/search-tryappservice/AzSearch-TryAppService-HitHighlight.png