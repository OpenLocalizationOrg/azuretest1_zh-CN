---
ms.openlocfilehash: a6aa2d72b15695cc8cc667ae55b2c7550c5a3fa3
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="创建第一个搜索解决方案使用 Azure 搜索" 
    description="创建第一个搜索解决方案使用 Azure 搜索" 
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

# 创建第一个搜索解决方案使用 Azure 搜索

此示例演示的艾德自行车公司的搜索应用程序。 后学完本教程，您将有丰富的搜索体验，包括导航多面、 多个排序选项为您的搜索结果和提前键入查询建议联机产品目录。

   ![][7]

该演示获取您开始使用 Azure 搜索通过逐步引导您完成这些练习︰

+   创建 Azure 搜索索引。
+   加载到 SQL Server 数据库中的 Azure 搜索索引文档 （数据）。
+   生成 ASP.NET MVC4 应用程序，它利用 Azure 搜索︰
    +   搜索和显示从 Azure 搜索索引结果
    +   显示在搜索框中输入一个搜索词时的提前键入的建议
    +   使用 faceting 筛选搜索结果


<a id="sub-1"></a>
## 先决条件

+   [Azure 的订阅](../includes/free-trial-note.md)。 如果您不准备注册试用订阅，可以跳过该教程，而是选择了[尝试 Azure 应用程序服务](https://tryappservice.azure.com/)的。 此可选选项提供 Azure 搜索与 ASP.NET Web 应用程序免费每个会话一个小时-没有订阅所需。
+   Visual Studio 2012 或更高版本使用 ASP.NET MVC 4 和安装 SQL Server。 如果您没有安装的软件，您可以下载免费的速成版︰ [Visual Studio 2013年表达](http://www.visualstudio.com/products/visual-studio-express-vs.aspx)和[Microsoft SQL Server 2014年表达](http://msdn.microsoft.com/evalcenter/dn434042.aspx)。
+   Azure 搜索服务。 您将需要搜索服务名称，再加上管理密钥。 有关详细信息，请参阅[创建门户中 Azure 搜索服务](search-create-service-portal.md)。
+   [在 CodePlex 上的探险工作 Azure 搜索演示项目](http://go.microsoft.com/fwlink/p/?LinkID=510972)。 在源选项卡上，单击**下载**以获取解决方案的 zip 文件。 

    ![][12]


此解决方案包含两个项目︰

+   **目录索引**创建 Azure 搜索索引，从该项目中包含 SQL Server 数据库中加载数据。
+   **AdventureWorksWeb**是一个基于 MVC4 的应用程序查询 Azure 搜索索引。 本教程假设您具有 ASP.NET MVC 的工作知识。

<a id="sub-2"></a>
## 创建 Azure 搜索索引

在此步骤中，您将使用**目录索引**创建新 Azure 搜索索引调用基于 AdventureWorks SQL Server 数据库中的数据"目录"。

1.  在 Visual Studio，打开名为**AdventureWorksCatalog.sln**的 Azure 搜索演示解决方案。  
2.  **CatalogIndexer**在解决方案资源管理器中用鼠标右键单击并选择**设为启动项目**，以便此应用程序运行，而不是**AdventureWorksWeb**项目中，当您按**f5 键**。
3.  打开此项目中的**App.config**和 Azure 搜索服务的"SearchServiceName"和"SearchServiceApiKey"更新的值。 查看搜索服务名称，如果您的服务是"mysearch.search.windows.net"，您可以输入"mysearch"。
4.  App.config （可选），包括一项为"SourceSqlConnectionString"时，假定 SQL Server 2014年直通 LocalDB (Server=(LocalDB)\v11.0)。 如果您使用的是不同版本的 SQL Server，则相应地更新的服务器名称。 例如，如果您有一个本地的默认实例，可以使用 （本地） 或本地主机。
5.  保存**App.config**。
6.  按**f5 键**以启动项目。

在命令提示符下打开以下文字:"创建索引...

后一段时间，它应完成的文本:"完成。  按<enter>继续:"

   ![][8]

按**enter 键**关闭应用程序。 此时，您已成功创建 Azure 搜索索引。 

> [AZURE.NOTE] 如果出现错误，包括"无效值 attachdbfilename 键"或某些其他数据库附件错误，您可能会遇到 UAC 冲突。 本演示的目的，为解决这些错误按以下方法︰ 将解决方案复制到新的或现有的文件夹 （如温度） 向经过身份验证的用户提供访问。 
> 使用**以管理员身份运行**命令来启动 Visual Studio。 
> 打开的解决方案，生成它，然后按**F5**以创建索引。

若要验证创建索引和文档上载，请转到[Azure 管理门户](https://portal.azure.com)中搜索服务仪表板。 在用法中，索引计数应该加一，并应具有 294 文档，另一个用于数据库中的每个产品。

单击以显示索引列表的**索引**图块。 索引列表出幻灯片以显示新的索引和文档数。 请注意在自由定价层可以有最多三个索引。 如果您已经有了三个索引，您需要删除一个要释放任何新的空间。

   ![][9]


<a id="sub-3"></a>
## 探讨 CatalogIndexer

让我们更仔细地看一下**CatalogIndexer**项目，以了解它的工作原理。

1.  从解决方案资源管理器中打开**Program.cs** ，转到`Main(string[] args)`函数。

    注意到此功能如何建立称为 Uri`_serviceURI`基于您特定的 Azure 搜索服务，然后创建名为 HttpClient`_httpClient`使用的 API 密钥对此搜索服务进行身份验证。

2.  `ChangeEnumeratorSql` 和`ChangeSet`用于枚举在您的 SQL Server AdventureWorks 数据库的产品表中的数据。 

3.  从这个表中，收集的数据根据`ApplyChanges`然后调用将此数据应用于 Azure 搜索索引。

4.  移动到`ApplyChanges`在同一文件中。 注意如果已经存在，则此函数删除索引的方式 (`DeleteCatalogIndex`)，然后创建一个新索引，称为"目录"(`CreateCatalogIndex`)。  

5.  移动到`CreateCatalogIndex`正常工作，并注意如何与架构相匹配在 SQL Server 中产品表中的列创建索引。 每个字段都有一个类型 (即`Edm.String`或`Edm.Double`) 和属性来定义这些字段的用途是什么。 请参阅有关这些属性的详细的[REST API，Azure 搜索文档](http://msdn.microsoft.com/library/azure/dn798935.aspx)。

6.  回到`ApplyChanges`函数。 注意到此功能如何循环遍历所有中所枚举的变更数据的`ChangeSet`。 而不是应用更改一个一个地，它们是成批放入 1000年组，然后应用到搜索服务。 这是比应用文档由一个一个的效率高得多。

现在，您已经了解如何创建和填充索引使用 SQL Server 中的关系数据，让我们看看如何创建产品目录，使用我们的搜索数据。

<a id="sub-4"></a>
## 生成使用 Azure 搜索 MVC4 应用程序

在此步骤中，您将生成并在 web 浏览器中运行的搜索应用程序。

1.  在 Visual Studio，打开名为**AdventureWorksCatalog.sln**Azure 搜索演示解决方案。  

2.  **AdventureWorksWeb**在解决方案资源管理器中用鼠标右键单击并选择**设为启动项目**。

3.  在此项目中打开**Web.config**和 Azure 搜索服务的"SearchServiceName"和"SearchServiceApiKey"更新的值。 查看搜索服务名称，如果您的服务是"mysearch.search.windows.net"，您可以输入"mysearch"。

4.  将保存**Web.config**中。

5.  按**f5 键**以启动项目。 如果产生一个生成错误，请按照下列[疑难解答](#err-mvc)步骤。 

    ![][10]

6.  将搜索框保留为空，然后单击**搜索**返回所有 294 产品。

7.  请注意的方面是沿左侧的列表。 请尝试单击的必要条件之一。 重新执行搜索，但这一次它添加您选择要筛选结果的方面。 您可能想要将断点添加到**HomeController.cs**中的第一行`Search`函数来逐步执行每一行 (F11)。

7.  请输入搜索词，例如"山地自行车"。 当您键入时，注意到建议查询下拉列表。

    ![][11]

此演示从头到尾屏幕截图会再次出现下面，作为提醒的内容被覆盖。 在前面的章节中，我们讨论了多面导航 （在左边），顶部的页面、 建议，并按相关性、 列表或价格进行排序的排序选项的文档数。

   ![][7]


<a id="sub-5"></a>
## 探讨 AdventureWorksWeb

项目 AdventureWorksWeb 显示我们如何使用 ASP.NET MVC 4 Azure 搜索与交互的 web 应用程序。 在本节中，我们将回顾各个部分应用程序代码，以查看他们做些什么。

1.  在解决方案资源管理器中，展开**AdventureWorksWeb** | **控制器**，并打开**HomeController.cs**

    这是从索引视图中管理交互的 MVC 控制器。  在控制器的顶部，您会注意到对`CatalogSearch _catalogSearch`的创建 HttpClient 连接对象，Azure 搜索服务。 此代码`CatalogSearch`位于**CatalogSeach.cs**

2. 在**HomeController.cs**内, 有两个主要功能︰

    **搜索**︰ 当用户单击搜索按钮或选择某个方面时，调用此函数以检索结果并将它们发送回索引视图将显示。

    **建议**︰ 根据用户键入的搜索框中时，调用此函数以返回一组基于 Azure 搜索索引中的内容的建议。

让我们深入到这两个函数中的详细信息。  

3.  在**HomeController.cs** `Search`函数中，您将注意到对的调用`_catalogSearch.Search`，其中包括到 Azure 搜索服务的连接对象。 位于**CatalogSearch.cs**的搜索函数调用时，您可以看到它使用这些参数来扩建 Azure 搜索查询。 将查询结果存储在一个 JSON 对象，称为`result`，然后传递回`Index`的结果分析，并在网页上显示的视图。

4.  打开在视图下的**Index.cshtml** |主页，您会注意到的代码，用来分析这些结果。

5.  如果仍在运行，停止该应用程序并打开视图下的**Index.cshtml**文件 |家庭。  在该文件的结尾，您将看到一个 JavaScript 函数，使用`JQuery $(function ())`。 在页面加载时调用此函数。 它使用 JQuery 记忆式键入功能函数和链接此作为从搜索文本框中，标识为"q"的回调函数。 每当有人在文本框中键入，此自动建议函数称为后者又调用 /home/suggest 与输入的内容。  `/home/suggest` 在调用**HomeController.cs**函数参考`Suggest`。

6.  打开**HomeController.cs**并移到建议函数。 此代码是非常相似的搜索功能使用`_catalogSearch`对象调用的函数中调用**CatalogSearch.cs** `Suggest`。 而不是进行搜索查询，`Suggest`函数调用[API 的建议](http://msdn.microsoft.com/library/azure/dn798936.aspx)。 这将使用输入的文本框和构建一组潜在的建议中的条款。 作为提前键入选项在搜索框中会自动列出的**Index.cshtml**文件返回值。

您可能会问自己在此时 Azure 搜索如何知道哪些域通过构建的建议。 这问题的答案都在创建索引时。 在`CreateCatalogIndex`函数在文件 Program.cs 的**CatalogIndexer**项目中，有被称为属性`Suggestions`。  当此属性设置为`True`，它意味着 Azure 搜索可以使用它作为字段检索建议

让我们试一试。  

7.  再一次生成应用程序，然后按**f5 键**以运行该应用程序。

8.  在"道路"一词在搜索框中键入。  后一秒钟，您应该看到与建议，用户可能无法选择文本框下方显示项的列表。  

您可能想要添加到断点`Suggest` **HomeController.cs**内起作用，并逐步在进行扩建建议的替换列表中的每个调用 (F11)。

该演示到此结束。 您现在已经完成所有您需要构建一个使用 Azure 搜索的 ASP.NET MVC4 应用程序之前需要了解的主要操作。


<a id="err-mvc"></a>
## 如何解决"无法加载文件或程序集 System.Web.Mvc"

当构建 AdventureWorksWeb，如果您收到"无法加载文件或程序集 System.Web.Mvc、 版本 = 4.0.0.0，区域性程序 = 31bf3856ad364e35 或其依赖项之一"，尝试下列步骤以解决此错误。

1. 打开程序包管理器控制台︰**工具** | **NuGet 程序包管理器** | **程序包管理器控制台**
2. 下午 > 提示，输入"更新软件包-重新安装 Microsoft.AspNet.Mvc"
3. 当系统询问重新加载该文件，选择**全部**。
4. 重新生成解决方案，然后重试**F5** 。


<a id="next-steps"></a>
## 下一步行动

其他自学，请考虑添加当用户单击某个搜索结果时将打开详细信息页。 若要准备，您可以执行下列操作︰

+   自学[查找 API](http://msdn.microsoft.com/library/azure/dn798929.aspx) ，您可以对 Azure 搜索将返回特定文档 （例如可以传递产品 id） 进行查询。
+   尝试调用的详细信息的**HomeController.cs**文件中添加一个新函数。 添加相应的**Details.cshtml**视图接收此查找的结果，并显示结果。
+   签出此附加代码示例和上地理空间搜索视频︰[通道 9-Azure 搜索和地理空间数据](http://channel9.msdn.com/Shows/Data-Exposed/Azure-Search-and-Geospatial-Data)和[CodePlex: Azure 搜索 GeoSearch 示例](http://azuresearchgeospatial.codeplex.com)

您还可以查看在 MSDN 上[搜索 REST API，Azure](http://msdn.microsoft.com/library/azure/dn798935.aspx) 。


<!--Anchors-->
[先决条件]: #sub-1
[创建 Azure 搜索索引]: #sub-2
[探讨 CatalogIndexer]: #sub-3
[生成使用 Azure 搜索 MVC4 应用程序]: #sub-4
[探讨 AdventureWorksWeb]: #sub-5
[下一步行动]: #next-steps


<!--Image references-->
[7]: ./media/search-create-first-solution/AzureSearch_Create1_WebApp.PNG
[8]: ./media/search-create-first-solution/AzureSearch_Create1_ConsoleMsg.PNG
[9]: ./media/search-create-first-solution/AzureSearch_Create1_DashboardIndexes.PNG
[10]: ./media/search-create-first-solution/AzureSearch_Create1_WebAppEmpty.PNG
[11]: ./media/search-create-first-solution/AzureSearch_Create1_Suggestions.PNG
[12]: ./media/search-create-first-solution/AzureSearch_Create1_CodeplexDownload.PNG