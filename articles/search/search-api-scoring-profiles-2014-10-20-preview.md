---
ms.openlocfilehash: 09daa73096f37550e184011705a0fc8a203247cd
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="评分 （Azure 搜索 REST API 版本 2014年-10-20-预览） 的配置文件" 
    description="评分 （Azure 搜索 REST API 版本 2014年-10-20-预览） 的配置文件" 
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
      
#评分 （Azure 搜索 REST API 版本 2014年-10-20-预览） 的配置文件#

> [AZURE.NOTE] 本文介绍了计分 REST API 的 2014年-10-20 的预览版本中的配置文件。 在 MSDN 上[搜索索引添加评分配置文件](http://msdn.microsoft.com/library/azure/dn798928.aspx)找不到此 API 的发布的版本。 2014-10-20 的预览版本有关详细信息，请参阅[Azure 搜索服务 REST API: 2014年 10-20 预览](search-api-2014-10-20-preview.md)。

评分是指的搜索结果中返回的每个项的搜索得分计算。 分数为当前搜索操作的上下文中的项的相关性的指标。 分数越高，更多相关项目。 在搜索结果中，项目位于排名低，根据计算出的每一项的搜索得分从高有序。

Azure 搜索使用默认评分，以计算初始分数，但您可以自定义计算通过计分的配置文件。 计分的配置文件可以更好地控制项的排名在搜索结果中。 例如，可能希望提高它们的收入潜力的项目、 升级更新的项目，或可能是大大提高了太长的库存中的项目。

计分的配置文件是包含字段、 函数和参数的索引定义的一部分。 

若要让您知道的计分配置文件如下所示，下面的示例演示一个简单的配置文件，名为地理。 这样大大提高了项目中包含搜索词`hotelName`字段。 它还使用`distance`函数来优选项目内的当前位置十公里。 如果有人搜索术语内，并且 inn 碰巧旅馆名称的一部分，则包括旅馆内与的文档将显示在搜索结果中更高。

    "scoringProfiles": [
      {
        "name": "geo",
        "text": {
          "weights": { "hotelName": 5 }
        },
        "functions": [ 
          {
            "type": "distance",
            "boost": 5,
            "fieldName": "location",
            "interpolation": "logarithmic",
            "distance": {
              "referencePointParameter": "currentLocation",
              "boostingDistance": 10 }
          }
        ]
      }
    ]

若要使用此计分的配置文件，您的查询表述来为查询字符串中指定的配置文件。 在下面的查询中，请注意查询参数，`scoringProfile=geo`请求中。

    GET /indexes/hotels/docs?search=inn&scoringProfile=geo&scoringParameter=currentLocation:-122.123,44.77233&api-version=2014-10-20-Preview

此查询搜索术语内，并在当前位置传递。 请注意，此查询包括其他参数，如`scoringParameter`。 查询参数[搜索文档 (Azure 搜索 API)](search-api-2014-10-20-preview.md#SearchDocs)所述。

单击以查看更详细的计分的配置文件示例的[示例](#example)。

## 什么默认评分？ ##

评分计算搜索分数等级有序的结果集中每个项目。 搜索结果集中的每个项目分配的搜索分数，则排名最高到最低。 以最高分数的项目将返回到应用程序。 默认情况下，返回了前 50，但您可以使用`$top`参数来返回更小或更大数量的项目 （最多 1000 中单个响应)。

默认情况下，搜索分数计算基于统计数据和查询的属性。 Azure 搜索查找在查询字符串中包含搜索词的文档 (部分或全部，具体情况取决于`searchMode`)，倾向于包含该搜索项的多个实例的文档。 搜索分数上升一词很少见的整个数据集，但是文档中通常也更高。 这种方法对计算相关性的基础称为 TF IDF 或 （术语频率逆文档频率）。

假设没有任何自定义排序，结果是再按排名搜索分数之前它们返回到调用应用程序。 如果`$top`未指定，则具有最高搜索返回分数 50 项。

可以在整个结果集重复搜索得分值。 例如，可能有 10 个项目的分数为 1.2，20 项的分数为 1.0 和 20 项的分数为 0.5。 当多个命中得分相同搜索时，相同分数的项的顺序未定义，则并不稳定。 运行一次查询，并且您可能会看到项偏移位置。 给出两个项目完全相同的分数，就哪一个先出现不能保证。

## 何时使用自定义权重##

当默认分级行为不能进入远不足以满足您的业务目标，您应该创建一个或多个计分的配置文件。 例如，您可能决定搜索相关性应优选新添加的项。 同样，您可能必须包含利润率的字段或其他字段，指示潜在的收入。 提高对您的业务带来好处的命中可以决定使用计分的配置文件中的一个重要因素。

基于关联性的顺序也是通过评分配置文件实现的。 考虑让您通过价格、 日期、 分级或相关性排序过去使用过的搜索结果页面。 在 Azure 搜索计分的配置文件的驱动器相关性选项。 相关性的定义是由您，前提是业务目标和您想要提供搜索体验的类型控制的。

<a name="example"></a>
## 示例##

如所述，自定义权重通过计分索引架构中定义配置文件实现。 

此示例显示具有两个计分的配置文件的索引架构 (`boostGenre`， `newAndHighlyRated`)。 此查询参数将使用该配置文件以结果集的成绩包括任意配置文件的索引的任何查询。

[尝试运行此例](search-get-started-scoring-profiles.md)。

    {
      "name": "musicstoreindex",
      "fields": [
        { "name": "key", "type": "Edm.String", "key": true },
        { "name": "albumTitle", "type": "Edm.String", "suggestions": true },
        { "name": "albumUrl", "type": "Edm.String", "filterable": false },
        { "name": "genre", "type": "Edm.String" },
        { "name": "genreDescription", "type": "Edm.String", "filterable": false },
        { "name": "artistName", "type": "Edm.String", "suggestions": true },
        { "name": "orderableOnline", "type": "Edm.Boolean" },
        { "name": "rating", "type": "Edm.Int32" },
        { "name": "tags", "type": "Collection(Edm.String)" },
        { "name": "price", "type": "Edm.Double", "filterable": false },
        { "name": "margin", "type": "Edm.Int32", "retrievable": false },
        { "name": "inventory", "type": "Edm.Int32" },
        { "name": "lastUpdated", "type": "Edm.DateTimeOffset" }
      ],
      "scoringProfiles": [
        {
          "name": "boostGenre",
          "text": {
            "weights": {
              "albumTitle": 1,
              "genre": 5 ,
              "artistName": 2 
            }
          }
        },
        {
          "name": "newAndHighlyRated",
          "functions": [
            {
              "type": "freshness",
              "fieldName": "lastUpdated",
              "boost": 10,
              "interpolation": "quadratic",
              "freshness": {
                "boostingDuration": "P365D"
              }
            },
            {
              "type": "magnitude",
              "fieldName": "rating",
              "boost": 10,
              "interpolation": "linear",
              "magnitude": {
                "boostingRangeStart": 1,
                "boostingRangeEnd": 5,
                "constantBoostBeyondRange": false
              }
            }
          ]
        }
      ]
    }


##工作流##

若要实现自定义的计分行为，添加到架构定义索引计分的配置文件。 您可以有多个计分的配置文件内的索引，但在任何给定的查询时只能指定一个配置文件。 

开始使用本主题中提供的[模板](#bkmk_template)。

提供一个名称。 计分的配置文件是可选的但如果您将添加一个名称是必需。 请务必按照字段的命名约定 （开头字母、 避免特殊字符和保留的字）。 有关详细信息，请参见[命名规则](http://msdn.microsoft.com/library/azure/dn857353.aspx)。

计分的配置文件的主体构造加权的字段和函数。

<font>
<table>
<thead>
<tr><td><b>元素</b></td><td><b>说明</b></td></tr></thead>
<tbody 
<tr>
<td><b>重量</b></td>
<td>
指定相对权重分配给字段的名称 / 值对。 在示例 [#bkmk_ex] 中，albumTitle、 类型和 artistName 字段分别提升 1、 5 和 null。 为什么是流派放大比其他得更高？ 如果在某种程度上是同构的数据执行搜索 (原样流派的情况在`musicstoreindex`)，您可能需要在相对权重较大差异。 例如，在`musicstoreindex`，作为这两个流派和具有相同表示流派描述显示 '摇滚'。 如果要高于流派描述的流派，流派字段需要更高的相对权重。
</td>
</tr>
<tr>
<td><b>函数</b></td><td>其他计算所需的特定上下文时使用。 有效值为`freshness`， `magnitude`， `distance` ， `tag`。 每个函数都有它特有的参数。
<br>
- `freshness` 要提高时，应使用通过如何新的或旧的项。 此函数仅可与日期时间字段 (Edm.DataTimeOffset)。 注意`boostingDuration`特性只能用于新鲜值函数。
<br>
- `magnitude` 要提高基于最高或较低的数值是时应使用。 此函数的调用的方案包括提高利润率、 高价格、 最低价格或下载次数的。 此函数仅可与双和整数字段。
<br>
- `distance` 应使用时需通过近似或地理位置来提高。 此函数仅可与`Edm.GeographyPoint`字段。
<br>
- `tag` 应使用当您想要标记文档和搜索查询之间的共同提高。 此函数仅可与`Edm.String`和 '(Collection(Edm.String) fields.
<br>
<b>使用函数的规则</b>
<br>
新鲜、 量、 距离 （标记） 的函数类型必须为小写。 
<br>
函数不能包含 null 值或空值。 具体来说，如果包含字段名，您必须将其设置为。
<br>
函数只用于筛选字段。 有关筛选字段的详细信息，请参阅[创建索引 (Azure 搜索 API)](search-api-2014-10-20-preview.md#createindex) 。
<br>
函数只用于索引的字段集合中定义的字段。
<td>
</tr>
</tbody>
</table>
</font>

定义索引后，通过上传的索引架构，跟文档建立索引。 有关这些操作的说明，请参阅[创建索引 (Azure 搜索 API)](search-api-2014-10-20-preview.md#createindex)和[添加或更新文档 (Azure 搜索 API)](search-api-2014-10-20-preview.md#AddOrUpdateDocuments) 。 一旦生成索引时，应该与您的搜索数据功能评分配置文件。

<a name="bkmk_template"></a>
##Template##

此部分显示的语法和评分的配置文件的模板。 请参阅下一节有关属性的说明中的[索引属性的引用](#bkmk_indexref)。

    ...
    "scoringProfiles": [
      { 
        "name": "name of scoring profile", 
        "text": (optional, only applies to searchable fields) { 
          "weights": { 
            "searchable_field_name": relative_weight_value (positive #'s), 
            ... 
          } 
        }, 
        "functions": (optional) [
          { 
            "type": "magnitude | freshness | distance | tag", 
            "boost": # (positive number used as multiplier for raw score != 1), 
            "fieldName": "...", 
            "interpolation": "constant | linear (default) | quadratic | logarithmic", 
            
            "magnitude": { 
              "boostingRangeStart": #, 
              "boostingRangeEnd": #, 
              "constantBoostBeyondRange": true | false (default) 
            }

            // (- or -) 
    
            "freshness": { 
              "boostingDuration": "..." (value representing timespan over which boosting occurs) 
            } 

            // (- or -) 

            "distance": { 
              "referencePointParameter": "...", (parameter to be passed in queries to use as reference location) 
              "boostingDistance": # (the distance in kilometers from the reference location where the boosting range ends) 
            } 

            // (- or -) 

            "tag": {
              "tagsParameter": "..." (parameter to be passed in queries to specify list of tags to compare against target field)
            }
          } 
        ], 
        "functionAggregation": (optional, applies only when functions are specified) 
            "sum (default) | average | minimum | maximum | firstMatching" 
      } 
    ], 
    "defaultScoringProfile": (optional) "...",
    ...

<a name="bkmk_indexref"></a>
##索引属性的引用##

**请注意**评分函数只能应用于可筛选的字段。 

<table style="font-size:12">
<thead>
<tr>
<td>属性</td>
<td>说明</td>
</tr>
<tr>
<td>名称</td>   <td>必需。 这是评分的配置文件的名称。 它遵循相同的命名约定的字段。 它必须以字母开头，不能包含冒号点或 @ 符号，并不能以短语 azureSearch （区分大小写） 开头。 </td>
</tr><tr>
<td>文字</td>   <td>包含的权重属性。</td>
</tr><tr>
<td>重量</td>    <td>可选项。 指定的字段名称和相对权重的名称-值对。 相对权重必须是一个正整数。 最大值为 int32。如何。 您可以指定字段名称而无需相应的权值。 用重量来指示相对于另一个字段的重要性。</td>
<tr>
<td>函数</td>  <td>可选项。 请注意，评分函数仅可应用于可筛选的字段。</td>
</tr><tr>
<td>类型</td>   <td>所需的评分函数。 指示要使用的函数的类型。 有效值包括模、 新鲜度、 距离和标记。 您可以在每个计分的配置文件中包括多个函数。 该函数的名称必须是小写字母。</td>
</tr><tr>
<td>提升</td>  <td>所需的评分函数。 用作原始分乘数一个正数。 它不能等于 1。</td>
</tr><tr>
<td>Fieldname</td>  <td>所需的评分函数。 评分函数只能用于属于索引的字段集合和，则筛选字段。 此外，每个函数类型引入了额外的限制 （与日期时间字段，具有整型或双域的大小、 距离与位置字段和具有字符串或字符串集合字段标记一起使用的新鲜）。 您只能指定每个函数定义的单个字段。 例如，若要使用同一个配置文件中的两次模，将需要包括两个定义量值，另一个用于每个字段。</td>
</tr><tr>
<td>内插</td>  <td>所需的评分函数。 定义斜率的分数提高了从范围起始点增至范围的结束。 有效值包括线性 （默认值）、 常数、 二次和对数。 有关详细信息，请参阅[设置内插]([#bkmk_interpolation])。</td>
</tr><tr>
<td>量值</td>  <td>量值评分函数用于改变排名基于数值字段的值的范围。 下面是一些这最常见的用法示例︰
<br>
- 星级︰ 改变基于"星等级"字段中的值的权重。 如果两个项目都相关，具有较高评级的项将首先显示。
<br>
- 边距︰ 当两个文档是相关的时零售商可能会提高文档的第一次有更高的利润。
<br>
- 单击计数︰ 对于应用程序跟踪单击产品或页面的操作，您可以使用就会变得最大流量的提升项目的大小。
<br>
- 下载次数︰ 跟踪下载、 模函数使您提高项目的应用程序具有最大的下载。
<tr>
<td>量值 |boostingRangeStart</td> <td>设置对其评定量的范围的开始值。 值必须是整数或双倍。 对于星级评定至 4，这是 1 的 1。 对于边距超过 50%，这是 50。</td>
</tr><tr>
<td>量值 |boostingRangeEnd</td>   <td>设置对其评定量范围的结束值。 值必须是整数或双倍。 对于星级评定至 4，这是 1 的 4。</td>
</tr><tr> 
<td>量值 |constantBoostBeyondRange</td>   <td>有效的值为 true 或 false （默认值）。 如果设置为 true，完全的提升将继续应用于文档具有高于上限的范围末尾的目标字段的值。 如果是 false，此函数的提升不会应用于文档范围外的目标字段的值。</td>
</tr><tr>
<td>新鲜</td>  <td>新鲜度评分函数用于改变基于方法字段中的值项的排名分数。 例如，日期更近的商品可以排名高于较旧的项目。 在当前的服务版本中，将为当前时间固定一个范围的末尾。 从最大值提高了改变和小范围由插值的速率应用到计分的配置文件 （请参见下图）。 若要反向应用的跃进因子，选择提升因子为 < 1。</td>
</tr><tr>
<td>新鲜 |boostingDuration</td>   <td>后增加有效期将停止特定文档集。 请参阅下一节有关语法和示例中[设置 boostingDuration](#bkmk_boostdur) 。</td>
</tr><tr>
<td>距离</td>   <td>使用评分函数影响的距离具体取决于文档的分关闭或到目前为止是相对于参考地理位置。 作为查询中参数的一部分提供的引用位置 (使用`scoringParameterquery`选项的字符串) 作为 lon，lat 参数。</td>
</tr><tr>
<td>距离 |referencePointParameter</td> <td>要在查询中用作引用位置传递一个参数。 scoringParameter 是一个查询参数。 查询参数的说明，请参阅[搜索文档 (Azure 搜索 API)](search-api-2014-10-20-preview.md#SearchDocs) 。</td>
</tr><tr>
<td>距离 |boostingDistance</td>    <td>一个数字，指示在跃进范围的结束位置的引用位置公里的距离。</td>
</tr><tr>
<td>tag</td>    <td>评分函数标记用于影响基于搜索查询和文档中的标记文档的分数。 将提升与搜索查询标记的文档。 搜索查询的标签提供为每个搜索请求中的评分参数 (使用`scoringParameterquery`选项的字符串)。</td>
</tr><tr>
<td>标记 |tagsParameter</td>    <td>要在查询中指定用于特定请求的标记传递一个参数。 scoringParameter 是一个查询参数。 查询参数的说明，请参阅[搜索文档 (Azure 搜索 API)]() 。</td>
</tr><tr>
<td>functionAggregation</td>    <td>可选项。 仅当指定的函数将会应用。 有效值包括︰ 总和 （默认值）、 平均值、 最小值、 最大值和 firstMatching。 搜索分是计算出多个变量，其中包括多个函数的单值。 此属性指示函数合并到单个聚合所有的提升方式提高，然后应用到基本文档分。 基础分数为基础计算出从文档和搜索查询的 tf idf 值。</td>
</tr><tr>
<td>defaultScoringProfile</td>  <td>当执行搜索请求，如果未不指定任何计分的配置文件时，默认评分则使用 (仅 tf 的 idf)。
在这里，导致 Azure 搜索时没有特定的配置文件提供搜索请求中使用该配置文件，均可以设置评分配置文件名称的默认值。 </td>
</tr>
</tbody>
</table>

<a name="bkmk_interpolation"></a>
##组内插##

内插，可以定义为其分数提高从范围起始点到范围的末尾增加的斜率。 可以使用下面的内插︰

- `Linear`  为最大值和最小范围内的项目，将在不断递减量执行应用于项大大提高。 线性是默认插值评分的配置文件。

- `Constant`    在开始和结束范围内的项目，不断提升都将应用于排名的结果。

- `Quadratic`   相比具有降低不断提升的线性插值，二次最初会以较小的速度降低，然后随着它临近结束范围，它而减少按照更高的时间间隔。 评分函数标记中不允许此插值法选项。

- `Logarithmic` 相比具有降低不断提升的线性插值，对数最初将以更高的速度下降，然后随着它临近结束范围，它而减少以更小的间隔。 评分函数标记中不允许此插值法选项。
 
<a name="Figure1"></a>
 ![][1]

<a name="bkmk_boostdur"></a>
##一组 boostingDuration##

`boostingDuration` 是的新鲜度函数的特性。 用于设置到期期间之后增加将停止为特定的文档。 例如，10 天的促销期间提高品牌的产品线，应指定这些文档为"P10D"的 10 天时间。

`boostingDuration` 必须格式化为 XSD"dayTimeDuration"值 （受限 ISO 8601 工期值的子集）。 这样的模式是:"P(nD)(T(nH)(nM)(nS))"。

下表提供了几个示例。 

<table style="font-size:12">
<thead>
<tr>
<td><b>持续时间</b></td> <td><b>boostingDuration</b></td>
</tr>
</thead>
<tbody>
<tr>
<td>1 天</td>  <td>""P1D</td>
</tr><tr>
<td>2 天 12 小时</td>    <td>""P2DT12H</td>
</tr><tr>
<td>15 分钟</td> <td>""PT15M</td>
</tr><tr>
<td>30 天，5 小时 10 分钟，和 6.334 秒</td>    <td>"P30DT5H10M6.334S"</td>
</tr>
</tbody>
</table>

有关更多示例，请参见[XML 架构︰ 数据类型 （W3.org 网站）](http://www.w3.org/TR/xmlschema11-2/)。

**请参见**

在 MSDN 上[搜索 azure 服务 REST API，](http://msdn.microsoft.com/library/azure/dn798935.aspx) <br/>
MSDN 上的[创建索引 （Azure 搜索 API）](http://msdn.microsoft.com/library/azure/dn798941.aspx)<br/>
在 MSDN 上[添加到搜索索引的计分配置文件](http://msdn.microsoft.com/library/azure/dn798928.aspx)<br/>

<!--Image references-->
[1]: ./media/search-api-scoring-profiles-2014-10-20-Preview/scoring_interpolations.png
 