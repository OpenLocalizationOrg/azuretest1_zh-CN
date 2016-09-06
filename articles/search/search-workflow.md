---
ms.openlocfilehash: 3e6854f41895be3bc0e3a8b65efee0b5b48179d8
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="Azure 搜索开发的典型工作流 |Microsoft Azure"
    description="作为工作流或构建原型和生产应用程序中集成的 Azure 搜索的路线图。"
    services="search"
    documentationCenter=""
    authors="HeidiSteen"
    manager="mblythe"
    editor=""/>

<tags
    ms.service="search"
    ms.devlang="rest-api"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="07/08/2015"
    ms.author="heidist"/>

# Azure 搜索开发的典型工作流

这篇文章是为组件提供自定义应用程序中的搜索体验包括 Azure 搜索的路线图。 具体取决于是否要测试水或准备讲解中，您需要一些初步指导如何将 Azure 搜索集成到您的自定义开发项目。

在下面的章节中，我们中断将帮助您评估 Azure 搜索如何更好地满足您的应用程序的搜索要求初始原型的典型工作流出。 这篇文章的第二部分介绍了重要的设计决策因子成更严重的应用程序的开发工作。

在开始创建原型之前，建议您斜向上的一个我们快速入门教程或此[一小时深深入研究演示视频](http://azure.microsoft.com/documentation/videos/tech-ed-europe-2014-azure-search-deep-dive/)。 入门教程将提供在用这些语言︰ [.NET](search-get-started-dotnet.md)， [Java](search-get-started-java.md)， [Node.JS](search-get-started-nodejs.md)。

## 原型开发

成功的原型的最快路径通常在这一节中包括的步骤。 步骤包括供应服务、 定义索引架构、 加载文档，索引和查询索引。

对于易失性数据 （例如，常见的情况包括对库存或内容的快速更改） 的应用程序，原型应包括用于更新文档以及组件。

   ![][1]

### 步骤 1︰ 设置服务

Azure 的搜索是一个完全管理在线服务可通过 Azure 的订阅。 [一旦您注册 Azure](http://azure.microsoft.com/pricing/free-trial/)，添加搜索服务时，会比较快。 请[创建搜索服务门户中](search-create-service-portal.md)访问有关说明如何向订阅中添加搜索服务。

有两种定价层，可供选择。 我们建议为原型，同时发出警告，您将需要使用您的数据的一小部分 （免费） 共享的服务。 共享的服务都是免费的 （通过试用或常规成员资格） 的现有订阅服务器时，是快速设置，而它约束的索引和 3 索引，最多 10000 个文档，每个索引，您可以使用的文档数或 50 MB 的存储总计，准。

### 步骤 2︰ 创建索引

您在创建服务后，您就可以创建索引，开始其架构定义。

创建索引的速度最快和最简单的方法是通过 Azure 的门户。 至少，每个文档必须有一个独一无二的密钥，并至少一个字段包含可搜索的数据。 若要开始，请参阅[创建门户中的索引](search-create-index-portal.md)。

> [AZURE.NOTE] 在 Azure 的搜索索引
>
> *索引*的组织，保持数据作为*搜索 corpus*所有后续的搜索操作。 搜索集都存储在云中搜索服务订阅，这样来快速、 一致地执行的搜索操作的一部分。 在搜索术语搜索 corpus 中的一项称为*文档*，和所有文档的总长度是*文档集合*。
>
>*索引架构*定义所有文档中的字段名称、 数据类型和属性，用于指定该字段是否可以搜索、 筛选、 facetable，等等。
>
> 除了文档结构索引架构还指定计分为加快搜索分数，提供标准的配置文件和启用自动完成查询 (suggesters) 的配置设置和 CORS 跨域的查询请求。 *为原型，我们建议您首先只需指定只在文档中的字段*，然后逐步添加其他功能 （请参阅有关的附加功能，可以在以后添加列表的第 5 步）。  
>
> 应用到一个真实的例子，考虑电子商务应用程序。 搜索索引将包含所有的产品或服务的可搜索应用程序 （任何重新出现在搜索结果中） 中。 将一个文档的每个 SKU。 每个文档将包括产品名称、 品牌、 规模、 价格、 颜色和甚至参考图像或其他您想要在搜索结果中返回的资源文件。

### 第 3 步︰ 加载文档

在 Azure 搜索保存索引之后, 下一步是填充文档的索引。 在此步骤中，数据上载、 分析、 标记，并存储在用于搜索工作负载的数据结构 （如反向索引）。

上传到一个索引的数据必须符合您在上一步中定义的架构。 文档数据被表示为一组的每个字段，以 JSON 格式的键/值对。 如果您的架构指定标识符 （键） 值 （或空），它为每个字段必须具有域、 名称字段、 数字字段，和 URL 字段中 （这样做如果外部图像作为搜索结果的一部分），然后送入该索引的所有文档。

有几种方法来加载文档，但现在，所有这些需要一个 API。 对于大多数原型，这一步可能是最耗时由于编码要求。 本文内下文中介绍的选项。

> [AZURE.NOTE] 请记住，共享的服务限制您对每个索引的 10000 个文档。 一定要减少您的数据集，使其保持在限制之下。 有关详细信息，请参阅[限制和约束](https://msdn.microsoft.com/library/dn798934.aspx)。

#### 如何将数据加载到索引中

一种方法是使用索引器。 对于 Azure DocumentDB 或 SQL Server （特别是 Azure SQL 数据库或在 Azure VM 中的 SQL Server） 的 Azure 中的关系数据源，可以使用[索引器](https://msdn.microsoft.com/library/dn946891.aspx)支持的数据源中检索文档。 使用索引器，用于加载文档可在下列任一获取的代码示例启动教程︰ [.NET](search-get-started-dotnet.md)， [Java](search-get-started-java.md)， [Node.JS](search-get-started-nodejs.md)。

第二种方法是编写一个简单的程序，使用 REST API 或加载文档的.NET 库︰

- [添加、 更新或删除的文档 (REST API)](https://msdn.microsoft.com/library/dn798930.aspx)
- [DocumentOperationsExtensions 类](https://msdn.microsoft.com/library/microsoft.azure.search.documentoperationsextensions.aspx)

适合于非常小的数据集的第三个选项是使用[Fiddler](search-fiddler.md)或[镶边把邮递员弄](search-chrome-postman.md)要上载的文档。

第四个选项，可能是最简单的一个，是借用从[探险工作 C# REST API 的示例](https://azuresearchadventureworksdemo.codeplex.com/)，从嵌入式数据库 (.mdf) 在解决方案中，加载文档或[评分配置文件 C# REST API 的示例](https://azuresearchscoringprofiles.codeplex.com/)，从该解决方案中包含的 JSON 数据文件加载数据的代码。

> [AZURE.TIP] 您可以修改并运行[评分的示例配置文件](https://azuresearchscoringprofiles.codeplex.com/)，数据 JSON 文件和 schema.json 文件替换为适用于您的应用程序的数据。

### 步骤 4︰ 查询文档

一旦文档加载到索引，您可以编写您的第一个查询。

从搜索服务重新获得初始搜索结果的最快方法是使用[Fiddler](search-fiddler.md)或[镶边把邮递员弄](search-chrome-postman.md)要查看的答复，但实际上，要编写一些简单的用户界面代码，可以以可读格式查看结果。

Api，可用于搜索操作包括︰

- [搜索文档操作](https://msdn.microsoft.com/library/dn798927.aspx)
- [SearchIndexClient 类](https://msdn.microsoft.com/library/microsoft.azure.search.searchindexclient.aspx)

在 Azure 搜索查询可以是非常简单的。 包括`search=*`在 URI 将返回的前 50 个项目在您搜索集;指定`search=<some phrase>`将执行全文本搜索的短语，返回最多 50 个文档，前提至少 50 输入该术语包含匹配的文档。

50 文档是默认值。 您可以更改使用返回的项的数目`$Count`查询参数。 此参数记录在[搜索文档](https://msdn.microsoft.com/library/dn798927.aspx)。

> [AZURE.TIP] 最全面的查询示例列表可在[搜索文档](https://msdn.microsoft.com/library/dn798927.aspx)，但您可能需要查看要查看受支持的运算符的列表的[语法引用](https://msdn.microsoft.com/library/dn798920.aspx)。

### 步骤 5︰ 探索更多的功能

现在，有了服务和索引，您可以试验功能，以进一步发展搜索体验。 下文列出了功能研究的简短列表。

**搜索页**通常在结果集中，包括文档计数或使用分页到更易管理的数字细分的结果。 有关详细信息，请参阅[分页](search-pagination-page-layout.md)。

**searchMode = all**是更改 Azure 搜索计算 NOT 运算符的结果的查询参数。 默认情况下不包括的查询 （--） 展开，而不是缩小结果范围。 您可以设置此参数以更改运算符的顺序计算。 它在[搜索文档](https://msdn.microsoft.com/library/dn798927.aspx)或[SearchMode 枚举](https://msdn.microsoft.com/library/microsoft.azure.search.models.searchmode.aspx)中记录下来。

**评分配置文件**被用来提高搜索的成绩，从而导致符合预定义的条件才会出现在搜索结果中较高的项目。 请参阅[以计分的轮廓开始](search-get-started-scoring-profiles.md)逐步执行此功能。

使用**筛选器**来缩小搜索结果范围通过提供附加的条件对所选内容。 筛选器表达式放在查询中。 有关详细信息，请参阅[搜索文档](https://msdn.microsoft.com/library/dn798927.aspx)。

**多面导航**用于自我定向筛选。 Azure 搜索生成并返回的结构，并且您的代码将呈现在搜索结果页面中的多面导航结构。 有关详细信息，请参阅[多面导航](search-faceted-navigation.md)。

**Suggesters**指提前键入或自动完成查询返回的建议搜索搜索短语的第一个字符中的术语为用户类型。 有关详细信息，请参阅[建议操作](https://msdn.microsoft.com/library/dn798936.aspx)或[Suggesters 类](https://msdn.microsoft.com/library/microsoft.azure.search.models.suggester.aspx)。

**语言分析器**提供文本分析期间使用的语言规则。 Azure 搜索默认语言分析器 Lucene 英语，但您可以使用不同，甚至多个，通过指定索引中的分析器。 所有 Api 提供了 Lucene 分析器。 Microsoft 的自然语言处理器才在[2015年-02-28-预览 REST API，](search-api-2015-02-28-preview.md)可用。 [语言支持](https://msdn.microsoft.com/library/dn879793.aspx)的详细信息，请参见

### 第 6 步︰ 更新索引和文档

要计算的功能可能需要更新您的索引，它通常需要更新到您的文档的后续影响。

如果您需要更新索引或文档，例如，若要添加 suggesters，或者在您已为此目的，添加的字段上指定语言分析器，请参阅以下链接的说明︰

- [更新索引操作 (REST API)](https://msdn.microsoft.com/library/dn800964.aspx)
- [更新索引操作 (REST API)](https://msdn.microsoft.com/library/dn946892.aspx)
- [添加、 更新或删除的文档操作 (REST API)](https://msdn.microsoft.com/library/dn798930.aspx)
- [索引类 （.NET 库）](https://msdn.microsoft.com/library/microsoft.azure.search.models.index.aspx)
- [文档类 （.NET 库）](https://msdn.microsoft.com/library/microsoft.azure.search.models.document.aspx)

一旦您已经生成原型，建立了概念证明，可以学到新的水平的设计可以支持生产工作负载的开发项目。

## 应用程序开发

向前移动到下一个阶段需要决定有关哪些 Api 使用，如何管理文档，并将上载的频率，以及是否包括在搜索结果中的外部资源。

解决方案设计仍需包括所有为原型，描述的步骤，但是而不是确定优先级的最合适的路径，您将需要考虑哪些方法是最符合整体解决方案。

### 选择一个 API

Azure 搜索提供了两种编程模型︰.NET 库用于托管的代码和 REST API，为编程语言，如 Java、 JavaScript 或没有目标 Microsoft.NET Framework 的另一种语言。

目前，一个小的功能子集都不在但在.NET 库中，因此即使您希望编写托管的代码中，您可能需要使用 REST API 来获取您需要的功能。 仅在 REST API 提供的功能包括︰

- [仅预览 Microsoft 自然语言处理器 –](../search-api-2015-02-28-preview/)
- [moreLikeThis 功能-仅供预览](../search-api-2015-02-28-preview/)
- [管理 API](https://msdn.microsoft.com/library/dn832684.aspx)

您可以定期检查[新增](search-latest-updates.md)文章来监视功能状态的改变。

### 确定数据同步方法︰ 推或拉

推拉模型，请参阅文档索引中的更新方式。 通常情况下，方案规定最适合您的模型。

在线零售业务时，您很可能需要推模型，以便可以推或双写在库存中的任何更改对 OLTP 数据库及 Azure 搜索索引。 当 SKU 售出或大小或颜色变得不可用时，您会希望尽快以避免客户烦恼要更新的索引。 只有推模型可提供接近实时的更新与您的搜索索引。

在 Azure 搜索实现推模型没有特定的机制。 应用程序代码在数据层中，必须处理文档的更新操作使用的[REST API](https://msdn.microsoft.com/library/dn798935.aspx)或[.NET 库](https://msdn.microsoft.com/library/dn951165.aspx)更新集合中的文档。 为实现细节，使用文档键产品的 SKU 可以帮助完成该任务。

拉模型是通常预定从外部数据源检索数据的操作。 在 Azure 搜索拉模型是可通过[索引器](https://msdn.microsoft.com/library/azure/dn946891.aspx)，哪些是反过来可用于特定数据源︰ Azure DocumentDB 或 SQL Azure 数据库 （以及在 Azure Vm 上的 SQL Server）。

### 在批处理中加载文档

我们建议添加文档分批来提高吞吐量。 可批 1000 个文档，假设的平均文档大小为大约 1 到 2 KB。

没有 POST 请求的整体状态代码。 状态代码是 HTTP 200 （成功） 或 HTTP 207 （多重状态） 的成功和失败的文档组合时。 除了 POST 请求的状态代码，Azure 搜索维护每个文档的状态字段。 由于分批上载，需要一种方法来获取指示插入成功还是失败的每个文档的每个文档状态。 状态字段提供的信息。 它将设置为 false，如果文档加载失败。

在负载过重时下, 不鲜见有一些上载失败。 这会发生，整体的状态代码是 207，表明部分成功，失败索引的文档将具有状态属性设置为 false。

> [AZURE.NOTE]  当服务接收文档时，他们正在排队等候索引并不能立即包含在搜索结果中。 当不在负载过重时，文档通常在几秒钟内索引。

在更新索引时，您可以组合多个操作 （插入、 合并、 删除） 到同一批次，避免了多个往返。 目前 Azure 搜索不支持部分的更新 （修补 HTTP），因此，如果您需要更新的索引，您必须重新发送索引定义。

### 将外部数据集成到搜索结果

Azure 搜索使用内部存储的索引和搜索操作中使用的文档。 文本分析和索引分析是取决于所有可搜索字段和随时可用的关联的特性。

但是，并非所有的域在文档中将可搜索。 例如，如果您的应用程序是在线目录的音乐或视频，我们建议将二进制文件存储在 Azure Blob 服务或其它存储格式。 二进制文件本身不是可搜索，因此没有必要保留搜索 Azure 存储中。 虽然应在其他服务或位置来存储图像、 视频和音频文件，应包括引用文件的位置的 URL 的字段。 这种方式，可以为搜索结果的一部分返回外部数据。

要使用外部数据，应该在索引存储的 URL 链接到外部数据文件中定义一个字段。 如果发出[查找文档](https://msdn.microsoft.com/library/dn798929.aspx)请求，或在搜索结果中包含字段中，二进制文件将显示文档的上下文中。

### 容量规划

在 Azure 搜索更吸引人之一就是功能的的易于与其可以向上扩展或缩小以响应请求的资源。 此功能不会消除对容量规划的需要，而 does 最大限度地大部分风险。 您不是只能使用额外的硬件或不适当的硬件来运行您搜索的工作负载。

作为最后一步，查看现有的资源级别对副本和分区，并确定是否需要调整。 最简单的方法来调节容量是[Azure 的门户网站](https://ms.portal.azure.com/)中。

请记住只有定价层的标准可以放大或缩小。 此外，根据调整的程度，它可以花几分钟到几个小时来部署您的服务的其他群集。

> [AZURE.NOTE] 可以使用管理 REST API 以编程方式调整容量。 有关详细信息，请参阅[管理其他 API](https://msdn.microsoft.com/library/azure/dn832684.aspx)。


<!--Image references-->
[1]: ./media/search-workflow/AzSearch-Workflow.png
