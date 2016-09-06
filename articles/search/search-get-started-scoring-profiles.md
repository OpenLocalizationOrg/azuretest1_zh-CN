---
ms.openlocfilehash: b6d0466d4eb8c6a842183286e2da9804380748c7
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="如何在 Azure 搜索中使用计分的配置文件" 
    description="开始使用 Azure 搜索的计分方法配置文件" 
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

# 如何在 Azure 搜索中使用计分的配置文件

计分的配置文件是 Microsoft Azure 搜索自定义搜索分数的计算影响的项目都在搜索结果列表中按顺序排列的一种功能。 您可以认为通过提高符合预定义的条件的项目评分为模型的相互关系，一种方式的配置文件。 例如，假设您的应用程序是在线酒店预订网站。 通过提高`location`字段，搜索包含术语像西雅图将导致更高分数的项目中有西雅图`location`字段。 注意您可以具有的计分的多个配置文件，或无根本，如果默认评分足以满足您的应用程序。

为了帮助您试验评分的配置文件，您可以下载的示例应用程序使用计分的配置文件来更改搜索结果的排名顺序。 该示例是一个控制台应用程序 – 或许不非常现实真实的应用程序开发的--但有用仍然作为一个学习工具。 

示例应用程序演示如何计分行为，使用虚构数据，称为`musicstoreindex`。 示例应用程序的简单性容易地修改计分配置文件和查询，并执行程序时查看排名顺序上的即时效果。

<a id="sub-1"></a>
## 先决条件

使用 Visual Studio 2013年在 C# 中，编写示例应用程序。 如果您尚没有一份 Visual Studio，尝试免费[Visual Studio 2013年速成版](http://www.visualstudio.com/products/visual-studio-express-vs.aspx)。

您需要订阅了 Azure 和 Azure 搜索服务以完成本教程。 设置服务的帮助信息，请参阅[创建门户搜索服务](search-create-service-portal.md)。

[AZURE。包括[需要要完成本教程的 Azure 帐户︰](../../includes/free-trial-note.md)]

<a id="sub-2"></a>
## 下载示例应用程序

在 codeplex 下载本教程中介绍的示例应用程序转到[Azure 搜索评分配置文件演示](https://azuresearchscoringprofiles.codeplex.com/)。

在源代码选项卡上，单击**下载**以获取解决方案的 zip 文件。 

 ![][12]

<a id="sub-3"></a>
## 编辑 app.config

1. 提取文件后，请在要编辑的配置文件的 Visual Studio 中打开的解决方案。
1. 在解决方案资源管理器中双击**app.config**。 此文件指定的服务终结点和一个`api-key`用于对请求进行身份验证。 您可以从管理门户网站获取这些值。
1. 登录到[Azure 的门户](https://portal.azure.com)。
1. 转到服务面板的 Azure 搜索。
1. 请单击要复制的服务 URL**属性**平铺
1. 请单击要复制**键**平铺`api-key`。

当您完成添加 URL 和`api-key`app.config，为应用程序设置应如下所示︰

   ![][11]


<a id="sub-4"></a>
## 使用此应用程序

至此，几乎可以生成并运行该应用程序，但在执行之前，看一看用来创建并填充索引的 JSON 文件。

**Schema.json**定义的索引，在此演示中包括的强调程度计分配置文件。 请注意，该架构定义的所有字段中的索引，如包括非可搜索字段，使用`margin`，可以在计分的配置文件中使用。 评分配置文件语法进行了说明[添加到 Azure 搜索索引的计分配置文件](http://msdn.microsoft.com/library/azure/dn798928.aspx)。

**Data1 3.json**提供的数据，跨流派的少数 246 相册。 数据是实际的唱片集和艺术家信息，具有相似的虚构字段的组合`price`和`margin`用来显示搜索操作。 数据文件符合索引并上载到 Azure 搜索服务。 上载数据并将数据编制索引后，您可以发出针对该查询。

**Program.cs**将执行以下操作︰

- 打开一个控制台窗口。

- 连接到 Azure 搜索服务 URL 和`api-key`。

- 删除`musicstoreindex`如果存在。

- 创建一个新`musicstoreindex`使用 schema.json 文件。

- 填充使用的数据文件的索引。

- 查询使用四种查询的索引。 请注意计分的配置文件都指定为查询参数。 所有查询搜索同一术语中，最佳。 第一个查询演示默认评分。 其余三个查询使用计分的配置文件。

<a id="sub-5"></a>
## 生成并运行应用程序

要判断连接或程序集引用问题，请生成并运行该应用程序，以确保没有任何问题，计算出第一。 您应该看到在后台打开一个控制台应用程序。 所有四个查询执行序列中不停顿。 在许多系统中，整个程序执行在 15 秒以内。 如果控制台应用程序中包含一条消息:"完成。 按回车键继续"，该程序已成功完成。 

若要比较查询运行，可以标记复制粘贴控制台中的查询结果并将它们粘贴到 Excel 文件。 

下面的插图显示从第三个查询通过并行的结果。 所有查询使用相同的搜索词，最好，出现在多个唱片集标题。

   ![][10]

第一个查询使用默认评分。 因为仅在唱片集标题中出现的搜索项，不指定任何其他条件，搜索服务发现它们的顺序返回唱片的标题中最有项。 

第二个查询使用计分的配置文件，但注意到该配置文件并不影响。 将第一个查询相同的结果。 这是因为计分的配置文件将提高不是 germane 到查询中的字段 （流派）。 搜索词 '最佳' 中不存在任何流派字段的任何文档。 当计分的配置文件不起作用时，结果是与默认评分相同。  

第三个查询是评分的影响配置文件的第一个证据。 搜索词是仍然 '最佳'，所以我们正在与一组相同的影集，但是，由于计分的配置文件提供了附加的条件，大大提高了等级和上次更新，可以将一些项目更高的 propelled 列表中。

下图显示第四个也是最后的查询，由边距提升。 边距字段是不可搜索并不能在搜索结果中返回。 边距值已手动添加到电子表格来帮助说明具有更高的利润的项目会显示在搜索结果列表中较高的点。 

   ![][9]

既然您体验过计分的配置文件，请尝试更改程序使用不同的查询语法，评分配置式，或者更丰富的数据。 在下一节中的链接提供了详细信息。

<a id="next-steps"></a>
## 下一步行动

了解有关评分配置文件的详细信息。 有关详细信息，请参阅[添加到 Azure 搜索索引的计分的配置文件](http://msdn.microsoft.com/library/azure/dn798928.aspx)。

了解有关搜索语法和查询参数。 有关详细信息，请参阅[搜索文档 (Azure 搜索 REST API)](http://msdn.microsoft.com/library/azure/dn798927.aspx) 。

需要退一步并了解更多有关创建索引？ [观看此视频](http://channel9.msdn.com/Shows/Cloud+Cover/Cloud-Cover-152-Azure-Search-with-Liam-Cavanagh)以了解的基本知识。

<!--Anchors-->
[先决条件]: #sub-1
[下载示例应用程序]: #sub-2
[编辑 app.config]: #sub-3
[使用此应用程序]: #sub-4
[生成并运行应用程序]: #sub-5
[下一步行动]: #next-steps

<!--Image references-->
[12]: ./media/search-get-started-scoring-profiles/AzureSearch_CodeplexDownload.PNG
[11]: ./media/search-get-started-scoring-profiles/AzureSearch_Scoring_AppConfig.PNG
[10]: ./media/search-get-started-scoring-profiles/AzureSearch_XLSX1.PNG
[9]: ./media/search-get-started-scoring-profiles/AzureSearch_XLSX2.PNG 