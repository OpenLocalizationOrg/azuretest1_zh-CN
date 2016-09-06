---
ms.openlocfilehash: 8671f6037330d17764b8700b8559b83b8b2dcf0a
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="Azure 搜索与 ASP.NET Web 应用程序 |Microsoft Azure"
    description="挂钩的 Azure 搜索的 ASP.NET Web 应用程序。 了解如何连接、 查询和呈现使用.Net 库或 REST API 的结果。"
    services="search"
    documentationCenter=""
    authors="HeidiSteen"
    manager="mblythe"
    editor="v-lincan"/>

<tags
    ms.service="search"
    ms.devlang="na"
    ms.workload="search"
    ms.topic="hero-article"
    ms.tgt_pltfrm="na"
    ms.date="08/26/2015"
    ms.author="heidist"/>

#如何使用 ASP.NET Web 应用程序中集成 Azure 搜索

ASP.NET 是主流的 web 应用程序框架将 Azure 搜索与集成的自定义解决方案中。 在本文中，您将学习如何将 ASP.NET web 应用程序连接到 Azure 搜索，斜向提刀设计模式的常见的操作，以及查看几个编码做法可帮助您开发遇到转到更加顺利。 

##组织代码

到独立项目中相同的 Visual Studio 解决方案使您可以更灵活地设计方式将您的工作负载，维护和运行的每个程序。 建议三︰

- 索引创建代码
- 数据接收代码
- 用户交互的代码

在 Azure 搜索索引操作和文档操作 — 例如添加或更新的文档，或执行查询 – 是完全相互独立。 这意味着您可以将索引管理过程还成立了搜索请求并将结果呈现在 ASP.NET 用户交互代码分隔开来。

在大多数的我们的代码样本中，索引是创建并加载一个项目 （称为 DataIndexer，CatalogIndexer 或 DataCatalog 的各种示例中），同时，处理搜索请求和响应放在 ASP.NET MVC 应用程序项目中的代码中。 在代码示例中，在一个项目中，但生产代码包索引创建和文档上载到其实际可能会隔离这些操作。 一旦创建了一个索引，它很少发生变化 （而且如果它发生了更改，则需要重新生成），而文档，可能需要定期刷新。

将工作负荷分隔提供不同级别的权限的窗体中的其他优点 Azure 搜索 （而不是只查询权限的完全管理权限） 使用不同的编程语言，更特定的依赖关系，每个程序，加上能够独立地修改程序或创建多个前端应用程序的操作生成和维护中央的索引应用程序的索引。

##示例和演示使用 ASP.NET 和 Azure 搜索

几个代码示例已经存在，说明如何使用 ASP.NET 集成搜索。 您可以直接到代码或演示应用程序通过访问这些链接之一︰

- [纽约城 （纽约） 作业演示站点](http://aka.ms/azjobsdemo)
- [尝试应用程序服务 + Azure 搜索](search-tryappservice.md)
- [视频、 教程、 演示和代码示例的完整列表](earch-video-demo-tutorial-list.md)

##连接到服务

若要建立到服务和问题的请求的连接，Web 应用程序，只需三样东西︰ 

- 您已经设置，Azure 搜索服务的 URL 的格式设置为 https://<service-name>.search.windows.net
- 验证连接到 Azure 搜索 API 密钥 (GUID)
- 要制定连接请求的 HTTPClient 或 SearchServiceClient

####Url 和 API 键

可以在[门户网站](search-create-service-portal.md)中查找 URL 和 API 键或其使用[管理 REST API](https://msdn.microsoft.com/library/dn832684.aspx)以编程方式检索。 

通常情况下，URL 和密钥放在 web.config 文件中的用户交互程序︰

      <appSettings>
        <add key="SearchServiceName" value="[SEARCH SERVICE NAME]" />
        <add key="SearchServiceApiKey" value="[API KEY]" />
        . . .
      </appSettings>

搜索服务名称都可以只要您将附加域 (search.windows.net) 的连接，资源调配过程中指定的短名称，或者可以指定完全限定的名称 (< 服务名称 >。 search.windows.net) 在 web.config 文件中，而无需 HTTPS 的前缀。

API 密钥是服务资源调配 （admin 密钥只） 过程中生成，或如果您要在门户中创建查询键手动生成的身份验证标记。 键的类型决定哪些搜索操作可供您的应用程序︰

- 管理密钥 （读写权限，每个服务 2）
- 查询键 （只读，到每一项服务 50）

所有的 API 键是 Guid。 显然，管理和查询键之间没有什么区别。 您需要检查该门户或管理 REST API，用于确定键的类型。

> [AZURE.TIP] 查询项向客户端提供只读的体验。 请参阅[TryAppService + Azure 搜索](search-tryappservice.md)亲手操作中只读服务可用的 Azure 搜索操作。 注意，TryAppService，在 Web 应用程序代码是完全修改--您可以更改任何 ASP.NET 项目来修改网页布局，构造搜索查询中的 C# 代码或搜索结果 — — 它是只是 Azure 搜索索引和文档负载的服务操作都是只读的每个包含的查询 api 键上的服务连接。

####客户端连接

下面两个代码段设置与使用的 URL 和 API 键的搜索服务的连接。 回想起在 web.config 文件中指定的服务名称和 API 键。 对于 REST 调用，必须将管理密钥传递在请求标题中，可以在标题中或直接在 URL 中传递查询键时。

**[HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient.aspx)使用 REST API 调用**

    public class CatalogSearch
    {
        private static readonly Uri _serviceUri;
        private static HttpClient _httpClient;
        public static string errorMessage;

        static CatalogSearch()
        {
            try
            {
                _serviceUri = new Uri("https://" + ConfigurationManager.AppSettings["SearchServiceName"] + ".search.windows.net");
                _httpClient = new HttpClient();
                _httpClient.DefaultRequestHeaders.Add("api-key", ConfigurationManager.AppSettings["SearchServiceApiKey"]);
            }
            catch (Exception e)
            {
                errorMessage = e.Message.ToString();
            }
        }

**与.NET [SearchServiceClient](https://msdn.microsoft.com/library/azure/microsoft.azure.search.searchserviceclient.aspx)**

        static UsgsSearch()
        {
            try
            {
                string searchServiceName = ConfigurationManager.AppSettings["SearchServiceName"];
                string apiKey = ConfigurationManager.AppSettings["SearchServiceApiKey"];

                // Create an HTTP reference to the catalog index. Alternatively, include the index name in the query
                _searchClient = new SearchServiceClient(searchServiceName, new SearchCredentials(apiKey));
            }
            catch (Exception e)
            {
                errorMessage = e.Message.ToString();
            }
        }

##设计模式

Azure 搜索与集成的 Web 应用程序将需要构建查询并呈现结果。 本节提供如何组织代码中包含用户交互的代码的程序执行的任务的指导。 故意排除架构定义、 索引生成和数据提取。 有关如何进行这些操作的代码的指导，请参见演练和[视频、 示例和教程在 Azure 搜索](search-video-demo-tutorial-list.md)中列出的示例。 

###查询规范化

在标记为**isSearchable**架构中定义索引的字段上执行全文搜索索引通过。 给定搜索条件输入 （以下字符串"q"表示），搜索引擎查找内所有的可搜索字段匹配项并返回标记为**isRetrievable**的字段中的结果。 

> [AZURE.NOTE] 虽然大多数字段，可能需要搜索，索引可以包含字段只能在筛选器表达式，则会将其标记为不可搜索全文搜索中排除它们和非-可检索要从搜索中排除的情况下的结果。 

搜索查询包装到指定的目标索引，搜索请求用户提供的输入的术语以及用于筛选或改进请求参数。 运算符嵌入整个搜索字符串，如 +、-或 |，自动处理，这意味着没有用于分析搜索字词编码要求。 任何分析都将通过使用搜索引擎，作为内部操作。 您可以假定传入的字符串将被分析和分析引擎。

搜索查询有两种形式︰**搜索**或**建议**。 您将定义单独的方法，每种类型的查询。 在索引中的字段，则全文搜索**搜索**。 **建议**是提前键入或自动完成查询功能在 Azure 搜索生成的潜在基于前三个字符的用户输入的搜索条件列表中。 在大多数情况下，将包含相对独特或与众不同的值 （如产品或出版物名称），而不是包含无差别的数据字段的字段限制**的建议**。

下面的代码段捕获在程序中使用 REST API 搜索字词输入。 输入的术语由字符串 q，而其余的参数用于从多面导航结构相同的搜索页上的筛选值传递。 这两个输入术语和筛选中使用参数搜索方法。

        public ActionResult Search(string q = "", string color = null, string category = null, double? priceFrom = null, double? priceTo = null, string sort = null)
        {
            dynamic result = null;

            // If blank search, assume they want to search everything
            if (string.IsNullOrWhiteSpace(q))
                q = "*";

            result = _catalogSearch.Search(q, sort, color, category, priceFrom, priceTo);
            ViewBag.searchString = q;
            ViewBag.color = color;
            ViewBag.category = category;
            ViewBag.priceFrom = priceFrom;
            ViewBag.priceTo = priceTo;
            ViewBag.sort = sort;

            return View("Index", result);
        }
接受此查询的**搜索**方法定义如下。 请注意它在查询上定义的参数字符串，加上多面导航结构 （通过执行繁重的工作在缩小搜索结果范围的筛选器支持） 和排序顺序。

        public dynamic Search(string searchText, string sort, string color, string category, double? priceFrom, double? priceTo)
        {
            string search = "&search=" + Uri.EscapeDataString(searchText);
            string facets = "&facet=color&facet=categoryName&facet=listPrice,values:10|25|100|500|1000|2500";
            string paging = "&$top=10";
            string filter = BuildFilter(color, category, priceFrom, priceTo);
            string orderby = BuildSort(sort);

            Uri uri = new Uri(_serviceUri, "/indexes/catalog/docs?$count=true" + search + facets + paging + filter + orderby);
            HttpResponseMessage response = AzureSearchHelper.SendSearchRequest(_httpClient, HttpMethod.Get, uri);
            AzureSearchHelper.EnsureSuccessfulSearchResponse(response);

            return AzureSearchHelper.DeserializeJson<dynamic>(response.Content.ReadAsStringAsync().Result);
        }

在 MVC 视图或控制器，可以放在.NET 方法，构造一个搜索字符串。 此函数将字符串传递到主控制器。 它还定义数据结构的结果。 

    function Search() {

        var q = $("#q").val();
        
        $.post('/home/search',
        {
            q: q
        },
        function (data) {
            var searchResultsHTML = "<tr><td>FEATURE NAME</td><td>FEATURE CLASS</td>";
            searchResultsHTML += "<td>STATE ALPHA</td><td>COUNTY_NAME</td>";
            searchResultsHTML += "<td>Elevation (m)</td><td>Elevation (ft)</td><td>MAP NAME</td>";
            searchResultsHTML += "<td>DESCRIPTION</td><td>HISTORY</td><td>DATE CREATED</td>";
            searchResultsHTML += "<td>DATE EDITED</td></tr>";
            for (var i = 0; i < data.length; i++) {
                searchResultsHTML += "<td>" + data[i].Document.FEATURE_NAME + "</td>";
                searchResultsHTML += "<td>" + data[i].Document.FEATURE_CLASS + "</td>";
                searchResultsHTML += "<td>" + data[i].Document.STATE_ALPHA + "</td>";
                searchResultsHTML += "<td>" + data[i].Document.COUNTY_NAME + "</td>";
                searchResultsHTML += "<td>" + data[i].Document.ELEV_IN_M + "</td>";
                searchResultsHTML += "<td>" + data[i].Document.ELEV_IN_FT + "</td>";
                searchResultsHTML += "<td>" + data[i].Document.MAP_NAME + "</td>";
                searchResultsHTML += "<td>" + data[i].Document.DESCRIPTION + "</td>";
                searchResultsHTML += "<td>" + data[i].Document.HISTORY + "</td>";
                searchResultsHTML += "<td>" + parseJsonDate(data[i].Document.DATE_CREATED) + "</td>";
                searchResultsHTML += "<td>" + parseJsonDate(data[i].Document.DATE_EDITED) + "</td></tr>";
            }

            $("#searchResults").html(searchResultsHTML);

        });

在.NET 方法的调用**搜索**可能如下所示，包含在主 C# 程序，提供连接和搜索操作︰

        public DocumentSearchResponse Search(string searchText)
        {
            // Execute search based on query string
            try
            {
                SearchParameters sp = new SearchParameters() { SearchMode = SearchMode.All };
                return _indexClient.Documents.Search(searchText, sp);
            }
            catch (Exception e)
            {
                errorMessage = e.Message.ToString();
            }
            return null;
        }


###处理搜索结果

为行集组成标记为 isRetrievable 的索引架构中的字段返回搜索结果。 结果集呈现的更简单方法之一是使用 MVC ViewBag 系统对象。 下面的代码段是从 Index.cshtml [AdventureWorksDemo 在 CodePlex 上的项目](https://azuresearchadventureworksdemo.codeplex.com/)中。

    @model dynamic
    
    @{
        ViewBag.Title = "Search";
    }
    
    <h2>Product search</h2>
    
    @if (@ViewBag.errorMessage != null) {
        @ViewBag.errorMessage
    } else {
        <div class="container">
            <form action="/Home/Search" method="get">
                <input type="search" name="q" id="q" value="@ViewBag.searchString" autocomplete="off" size="100" /> <button type="submit">Search</button>
                <input type="hidden" name="color" id="color" value="@ViewBag.color" />
                <input type="hidden" name="category" id="category" value="@ViewBag.category" />
                <input type="hidden" name="priceFrom" id="priceFrom" value="@ViewBag.priceFrom" />
                <input type="hidden" name="priceTo" id="priceTo" value="@ViewBag.priceTo" />
                <input type="hidden" name="sort" id="sort" value="@ViewBag.sort" />
            </form>
        </div>

###多面导航

在相同的 Index.cshmtl 文件中，可以找到用于生成提供自我定向筛选、 分类的多面导航结构逐渐缩小按颜色、 价格或类别对搜索结果的 HTML。 

        if (@Model != null)
        {
            <div class="container">
                <div class="row">
                    <div class="col-md-4">
                        Colors:
                        <ul>
                            @foreach (var colorFacet in Model["@search.facets"].color)
                            {
                                <li><a href="#" onclick="document.getElementById('color').value='@colorFacet.value'; 
    document.forms[0].submit(); 
    return false;">@colorFacet.value</a> (@colorFacet.count)</li>
                            }
                        </ul>
                        Categories:
                        <ul>
                            @foreach (var categoryFacet in Model["@search.facets"].categoryName)
                            {
                                <li><a href="#" onclick="document.getElementById('category').value='@categoryFacet.value'; document.forms[0].submit(); return false;">@categoryFacet.value</a> (@categoryFacet.count)</li>
                            }
                        </ul>
                        Prices:
                        <ul>
                            @foreach (var priceFacet in Model["@search.facets"].listPrice)
                            {
                                if (priceFacet.count > 0)
                                {
                           <li><a href="#" onclick="document.getElementById('priceFrom').value=@(priceFacet.from ?? 0); document.getElementById('priceTo').value=@(priceFacet.to ?? 0); 
    document.forms[0].submit(); return false;
    ">@(priceFacet.from ?? 0) - @(priceFacet.to ?? "more")</a> (@priceFacet.count)</li>
                                }
                            }
                        </ul>
                    </div>
                    <div class="col-md-8">
                        <p>
                            Sort -
                            <a href="#" onclick="document.getElementById('sort').value=null; document.forms[0].submit(); return false;">by relevance</a>
                            <a href="#" onclick="document.getElementById('sort').value='listPrice'; document.forms[0].submit(); return false;">by list price</a>
                            <a href="#" onclick="document.getElementById('sort').value='color'; document.forms[0].submit(); return false;">by color</a>
                        </p>
                        <p>Found @Model["@odata.count"] products in the catalog</p>
    
                        <ul>
                            @foreach (var product in Model.value)
                            {
                                <li>
                                    <h3><b>@product.name</b></h3>
                                    price: @product.listPrice, color: @product.color, weight: @product.weight, size: @product.size
                                </li>
                            }
                        </ul>
                    </div>
                </div>
            </div>
        }
    }


###词突出显示

将样式应用于搜索项的搜索结果中的实例称为点击突出显示。 在 Azure 搜索词突出显示指定在查询中，突出显示的搜索参数，通过向其提供字段扫描匹配的术语的逗号分隔列表。 取决于您是实际应用的样式。 下面的三个代码段是从[TryAppService + Azure 搜索方法辅导教程](search-tryappservice.md)。

首先，作为搜索参数指定词的突出并列出字段以检查匹配的术语。 指定要使用点击突出显示的 HTML 样式。

    // Set the Search parameters used when executing the search request
         var sp = new SearchParameters
    {
    // Include a count of results in the query result
         IncludeTotalResultCount = true,
    // Limit the results to 20 documents
         Top = 20,
    // Enable hit-highlighting
         HighlightFields = new[] { "FEATURE_NAME", "DESCRIPTION", "FEATURE_CLASS", "COUNTY_NAME", "STATE_ALPHA" },
         HighlightPreTag = "<b>",
         HighlightPostTag = "</b>",
    };

接下来，循环搜索结果以查找需要突出显示的字符串。
专用的 HtmlString RenderHitHighlightedString(SearchResult item, string fieldName)

      {
         if (item.Highlights != null && item.Highlights.ContainsKey(fieldName))
          {
          string highlightedResult = string.Join("...", item.Highlights[fieldName]);
          return new HtmlString(highlightedResult);
          }
          return new HtmlString(item.Document[fieldName].ToString());
       }

最后，提供搜索结果，在前一段指定评估结果集的布局。

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


##常见的编码实践

新 MVC、.NET 编程，或 REST Api？  以下各节提供了几个编码实践以帮助您加快。

###MVC 模板

下表总结了 MVC 模板组件包括 Azure 搜索的应用程序中的使用方式。 如果您使用的 MVC 4 或 5，MVC，通常将被集成 Azure 搜索的代码添加到这些模块

文件|说明
----|-----------
Web.config|提供的服务的 URL 和 api 密钥。 添加在您的主程序模块读取值对 System.Configuration 的引用。
Program.cs|在主程序中，将 HttpClient 或 SearchServiceClient 建立到服务的连接设置。 添加对此程序的搜索方法。
数据模型|不使用。 假定索引创建和数据加载操作在不同的程序，没有数据模型是 Azure 搜索 web 应用程序中是必需的。
视图|视图包含应用程序的 web 页，从动态 HTML 处理搜索结果的搜索框中输入 HTML。
控制器|通常在 HomeContoller.cs 中找到查询构造和错误处理。 至少，控制器应该包括从 Azure 搜索检索结果并将转发到视图结果集的搜索方法。 

（可选） 如果使用的自动完成的查询的建议，将包括返回建议的查询，具体取决于您的索引是否包含向用户提供的搜索字词输入匹配的值的方法。

###何时使用 REST API 与.NET 客户端库

对于 ASP.NET 应用程序，.NET 客户端库被视为更好的选择，因为它设置 HTTP 连接并处理 JSON 序列化和反序列化，从而简化您的代码。

在某些情况下，您选择的 API 可能会为由功能两种方法之间的奇偶校验。 通常情况下， [.NET 客户端库](https://msdn.microsoft.com/library/azure/dn951165.aspx)和[REST API，服务](https://msdn.microsoft.com/library/azure/dn798935.aspx)可以互换，只要在两者中实现所需的操作。 但是，有时新功能显示 REST API 的预览版，一部分中第一个也是唯一添加到.NET 库以后几个月。 例如，索引器，用于自动从特定的数据源类型的数据加载操作，出现在预览 REST API，首先之前显示了客户端库在以后几个月。 在功能实现上的任何限制功能文档中说明。

###JSON 序列化和反序列化在 REST API，包括 AzureSearchHelper.cs

与.NET 库的执行这一步您服务 REST Api 必须序列化和反序列化中请求-响应的 JSON 文档交换的服务。 JSON 是有效载荷格式为数据传输时加载或刷新索引中的文档。 

可以以几个样本，在名为**AzureSearchHelper.cs**的文件中找到用于 JSON 序列化的代码︰

    using System;
    using System.Net.Http;
    using System.Text;
    using Newtonsoft.Json;
    using Newtonsoft.Json.Converters;
    using Newtonsoft.Json.Serialization;
    
    namespace CatalogCommon
    {
        public class AzureSearchHelper
        {
            public const string ApiVersionString = "api-version=2014-07-31-Preview";
    
            private static readonly JsonSerializerSettings _jsonSettings;
    
            static AzureSearchHelper()
            {
                _jsonSettings = new JsonSerializerSettings
                {
                    Formatting = Formatting.Indented, // for readability, change to None for compactness
                    ContractResolver = new CamelCasePropertyNamesContractResolver(),
                    DateTimeZoneHandling = DateTimeZoneHandling.Utc
                };
    
                _jsonSettings.Converters.Add(new StringEnumConverter());
            }
    
            public static string SerializeJson(object value)
            {
                return JsonConvert.SerializeObject(value, _jsonSettings);
            }
    
            public static T DeserializeJson<T>(string json)
            {
                return JsonConvert.DeserializeObject<T>(json, _jsonSettings);
            }
    
            public static HttpResponseMessage SendSearchRequest(HttpClient client, HttpMethod method, Uri uri, string json = null)
            {
                UriBuilder builder = new UriBuilder(uri);
                string separator = string.IsNullOrWhiteSpace(builder.Query) ? string.Empty : "&";
                builder.Query = builder.Query.TrimStart('?') + separator + ApiVersionString;
    
                var request = new HttpRequestMessage(method, builder.Uri);
    
                if (json != null)
                {
                    request.Content = new StringContent(json, Encoding.UTF8, "application/json");
                }
    
                return client.SendAsync(request).Result;
            }
    
            public static void EnsureSuccessfulSearchResponse(HttpResponseMessage response)
            {
                if (!response.IsSuccessStatusCode)
                {
                    string error = response.Content == null ? null : response.Content.ReadAsStringAsync().Result;
                    throw new Exception("Search request failed: " + error);
                }
            }
        }
    }


##下一步行动

若要进一步了解 Azure 搜索和 ASP.NET 集成，请访问以下链接︰

- [如何使用 Azure 搜索从.NET 应用程序](search-howto-dotnet-sdk.md) 
- [Azure 搜索开发案例研究](search-dev-case-study-whattopedia.md)
- [Azure 搜索开发的典型工作流](search-workflow.md) 

