---
ms.openlocfilehash: 3168c113dfbe935c850591cda34127d4bcd3b5f1
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="如何对资产使用 Azure Media 编码器进行编码" 
    description="了解如何使用 Azure Media 编码器编码在媒体服务上的媒体内容。 代码示例使用 REST API。" 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/11/2015" 
    ms.author="juliako"/>


#如何对资产使用 Azure Media 编码器进行编码


> [AZURE.SELECTOR]
- [.NET](media-services-dotnet-encode-asset.md)
- [REST](media-services-rest-encode-asset.md)
- [门户](media-services-manage-content.md#encode)

##概述
为了通过互联网提供数字视频必须压缩介质。 数字视频文件非常大，可能会太大，无法提供通过互联网或您客户的设备才能正确显示。 编码是压缩视频和音频，因此您的客户可以查看您的媒体的过程。

编码作业是一种媒体服务中最常见的处理操作。 创建从到另一种编码转换媒体文件的编码工作。 在编码时，您可以使用介质服务内置 Media 编码器。 您还可以使用编码器提供的媒体服务合作伙伴;第三方编码器能够通过 Azure 市场。 您可以指定编码任务通过使用预设的字符串定义为您的编码器，或通过使用预设的配置文件的详细的信息。 若要查看可用的预设的类型，请参阅[任务预设 Azure 媒体服务](https://msdn.microsoft.com/library/azure/dn619392.aspx)。 如果您使用第三方编码，您应[验证您的文件](https://msdn.microsoft.com/library/azure/dn750842.aspx)。


每个作业可以有一个或多个任务，具体取决于您想要完成的处理的类型。 通过 REST API，您可以创建作业和其相关的任务两种方法之一︰ 

- 任务可以通过作业实体的任务导航属性的内联定义或 
- 通过 OData 批处理。
  

建议始终对夹层文件到 MP4 设置，然后将组转换为所需的格式，使用[动态打包](https://msdn.microsoft.com/library/azure/jj889436.aspx)自适应比特率进行编码。 利用动态打包，必须先从中计划交付给您的内容的流终结点获取至少一个点播流计价单位。 有关详细信息，请参阅[如何规模的媒体服务](media-services-manage-origins.md#scale_streaming_endpoints)。

如果输出资产存储加密，您必须配置资产交付策略。 有关更多信息，请参见[配置资产交付策略](media-services-rest-configure-asset-delivery-policy.md)。


>[AZURE.NOTE]在开始引用媒体处理器之前，请确保您有正确的媒体处理器 id。 有关详细信息，请参阅[获取媒体处理器](media-services-rest-get-media-processor.md)。

##与单个编码任务创建作业 

>[AZURE.NOTE] 使用介质服务 REST API 时, 应该注意下列事项︰
>
>在访问中介质服务的实体，您必须设置特定的标头字段和值 HTTP 请求中。 有关详细信息，请参阅[设置介质服务 REST API 的开发](media-services-rest-how-to-use.md)。

>已成功连接到 https://media.windows.net 之后, 您将收到指定另一个媒体服务 URI 的 301 重定向。 必须使[连接到介质服务使用 REST API，](media-services-rest-connect_programmatically.md)所述新的 URI 对的后续调用。 


下面的示例演示如何创建和过帐一个任务设置对特定的分辨率和质量视频编码的作业。 当与 Azure Media 编码器编码，您可以使用指定任务配置预置[此处](https://msdn.microsoft.com/library/azure/dn619389.aspx)。
    
请求︰

    POST https://media.windows.net/API/Jobs HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer <token value>
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000
    Host: media.windows.net

    
    {"Name" : "NewTestJob", "InputMediaAssets" : [{"__metadata" : {"uri" : "https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3Aaab7f15b-3136-4ddf-9962-e9ecb28fb9d2')"}}],  "Tasks" : [{"Configuration" : "H264 Broadband 720p", "MediaProcessorId" : "nb:mpid:UUID:1b1da727-93ae-4e46-a8a1-268828765609",  "TaskBody" : "<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset><outputAsset>JobOutputAsset(0)</outputAsset></taskBody>"}]}

响应︰
    
    HTTP/1.1 201 Created

    . . . 

###设置输出资产的名称

下面的示例演示如何将 assetName 属性设置︰

    { "TaskBody" : "<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset><outputAsset assetName=\"CustomOutputAssetName\">JobOutputAsset(0)</outputAsset></taskBody>"}

##注意事项

- TaskBody 属性必须使用 XML 文本定义的输入或输出任务将使用的资产。 任务主题包含 XML 架构定义的 xml。
- 在 TaskBody 定义中，每个内部值为<inputAsset>，<outputAsset>必须设置为 JobInputAsset(value) 或 JobOutputAsset(value)。
- 任务可以具有多个输出资产。 一个 JobOutputAsset(x) 仅在作业中的任务的输出作为使用一次。
- 您可以为任务输入资产指定 JobInputAsset 或 JobOutputAsset。
- 任务未必须形成一个周期。
- Value 参数传递给 JobInputAsset 或 JobOutputAsset 表示某一资产的索引值。 在作业实体定义的 InputMediaAssets 和 OutputMediaAssets 导航属性中定义的实际资产。 
- 因为媒体服务基于 OData v3，通过引用 InputMediaAssets 和 OutputMediaAssets 导航属性集合中的单个资产"__metadata: uri"名称-值对。
- InputMediaAssets 将映射到一个或多个介质服务中创建的资产。 OutputMediaAssets 是由系统创建的。 他们不会参照现有的资产。
- 可以使用 assetName 属性命名为 OutputMediaAssets。 如果此特性不存在，则 OutputMediaAsset 的名称将任何内部文本的值<outputAsset>元素是带有后缀的作业名称值，或者作业 Id 值 （在名称属性并不在其中定义的情况）。 例如，如果设置为"Sample"的 assetName 的值，然后将 OutputMediaAsset 名称属性设置为"Sample"中。 但是，如果您未设置的值 assetName，但未设置为"NewJob"的作业名称，OutputMediaAsset 名称将是"JobOutputAsset （值） _NewJob"。 


##链接任务创建作业

在很多应用程序方案中，开发人员要创建一系列的处理任务。 在介质服务，您可以创建一系列链接的任务。 每个任务执行不同的处理步骤，可以使用不同的媒体处理器。 链接的任务可以递交资产从一个任务到另一个，在资产上执行任务的线性序列。 但是，在作业中执行的任务不是需要序列中。 创建链接的任务时，在单个**IJob**对象创建链接的**ITask**对象。

>[AZURE.NOTE] 目前有 30 个任务每个作业任务的限制。 如果需要链接 30 多个任务，请创建包含任务的多个作业。


    POST https://media.windows.net/api/Jobs HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer <token value>
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000

    {  
       "Name":"NewTestJob",
       "InputMediaAssets":[  
          {  
             "__metadata":{  
                "uri":"https://testrest.cloudapp.net/api/Assets('nb%3Acid%3AUUID%3A910ffdc1-2e25-4b17-8a42-61ffd4b8914c')"
             }
          }
       ],
       "Tasks":[  
          {  
             "Configuration":"H264 Adaptive Bitrate MP4 Set 720p",
             "MediaProcessorId":"nb:mpid:UUID:1b1da727-93ae-4e46-a8a1-268828765609",
             "TaskBody":"<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset><outputAsset>JobOutputAsset(0)</outputAsset></taskBody>"
          },
          {  
             "Configuration":"H264 Smooth Streaming 720p",
             "MediaProcessorId":"nb:mpid:UUID:1b1da727-93ae-4e46-a8a1-268828765609",
             "TaskBody":"<?xml version=\"1.0\" encoding=\"utf-16\"?><taskBody><inputAsset>JobOutputAsset(0)</inputAsset><outputAsset>JobOutputAsset(1)</outputAsset></taskBody>"
          }
       ]
    }


###注意事项

若要启用任务链接︰

- 作业必须有至少 2 项任务
- 必须有至少一个任务的输入是输出的作业中的另一个任务。

## 使用 OData 批处理 

下面的示例演示如何使用 OData 批处理创建作业和任务。 有关批处理的信息，请参阅[打开数据协议 (OData) 批处理](http://www.odata.org/documentation/odata-version-3-0/batch-processing/)。
 
    POST https://media.windows.net/api/$batch HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Content-Type: multipart/mixed; boundary=batch_a01a5ec4-ba0f-4536-84b5-66c5a5a6d34e
    Accept: multipart/mixed
    Accept-Charset: UTF-8
    Authorization: Bearer <token>
    x-ms-version: 2.11
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000
    Host: media.windows.net
    
    
    --batch_a01a5ec4-ba0f-4536-84b5-66c5a5a6d34e
    Content-Type: multipart/mixed; boundary=changeset_122fb0a4-cd80-4958-820f-346309967e4d
    
    --changeset_122fb0a4-cd80-4958-820f-346309967e4d
    Content-Type: application/http
    Content-Transfer-Encoding: binary
    
    POST https://media.windows.net/api/Jobs HTTP/1.1
    Content-ID: 1
    Content-Type: application/json
    Accept: application/json
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    Accept-Charset: UTF-8
    Authorization: Bearer <token>
    x-ms-version: 2.11
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000
    
    {"Name" : "NewTestJob", "InputMediaAssets@odata.bind":["https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3A2a22445d-1500-80c6-4b34-f1e5190d33c6')"]}
    
    --changeset_122fb0a4-cd80-4958-820f-346309967e4d
    Content-Type: application/http
    Content-Transfer-Encoding: binary
    
    POST https://media.windows.net/api/$1/Tasks HTTP/1.1
    Content-ID: 2
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    Accept-Charset: UTF-8
    Authorization: Bearer <token>
    x-ms-version: 2.11
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000
    
    {  
       "Configuration":"H264 Adaptive Bitrate MP4 Set 720p",
       "MediaProcessorId":"nb:mpid:UUID:1b1da727-93ae-4e46-a8a1-268828765609",
       "TaskBody":"<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset><outputAsset assetName=\"Custom output name\">JobOutputAsset(0)</outputAsset></taskBody>"
    }

    --changeset_122fb0a4-cd80-4958-820f-346309967e4d--
    --batch_a01a5ec4-ba0f-4536-84b5-66c5a5a6d34e--
 


## 创建作业使用作业模板


在处理多个资源使用一组通用的任务时，JobTemplates 可指定默认任务预设，任务的顺序，依次类推。

下面的示例演示如何使用 TaskTemplate 定义内联创建作业模板。 TaskTemplate MediaProcessor 作为使用 Azure Media 编码器编码资源文件中;但是，可能还使用其他 MediaProcessors。 


    POST https://media.windows.net/API/JobTemplates HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer <token value>
    Host: media.windows.net

    
    {"Name" : "NewJobTemplate25", "JobTemplateBody" : "<?xml version=\"1.0\" encoding=\"utf-8\"?><jobTemplate><taskBody taskTemplateId=\"nb:ttid:UUID:071370A3-E63E-4E81-A099-AD66BCAC3789\"><inputAsset>JobInputAsset(0)</inputAsset><outputAsset>JobOutputAsset(0)</outputAsset></taskBody></jobTemplate>", "TaskTemplates" : [{"Id" : "nb:ttid:UUID:071370A3-E63E-4E81-A099-AD66BCAC3789", "Configuration" : "H264 Smooth Streaming 720p", "MediaProcessorId" : "nb:mpid:UUID:2e7aa8f3-4961-4e0c-b4db-0e0439e524f5", "Name" : "SampleTaskTemplate2", "NumberofInputAssets" : 1, "NumberofOutputAssets" : 1}] }
 

>[AZURE.NOTE]与其他媒体服务的实体，必须为每个 TaskTemplate 中定义一个新的 GUID 标识符并在请求正文中将其放置在 taskTemplateId 和 Id 属性。 内容的标识方案必须按照所述标识 Azure 媒体服务实体的方案。 此外，不能更新 JobTemplates。 相反，您必须创建一个新的更新后的更改。
 

如果成功，将返回下面的响应︰
    
    HTTP/1.1 201 Created
    
    . . .


下面的示例演示如何创建一个作业引用作业模板 Id:

    POST https://media.windows.net/API/Jobs HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer <token value>
    Host: media.windows.net

    
    {"Name" : "NewTestJob", "InputMediaAssets" : [{"__metadata" : {"uri" : "https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3A3f1fe4a2-68f5-4190-9557-cd45beccef92')"}}], "TemplateId" : "nb:jtid:UUID:15e6e5e6-ac85-084e-9dc2-db3645fbf0aa"}
     

如果成功，将返回下面的响应︰
    
    HTTP/1.1 201 Created
    
    . . . 


##下一步行动
您已经知道了如何创建一个作业进行编码 assset，转到[如何与检查作业进度与介质服务](media-services-rest-check-job-progress.md)主题。


##请参见

[获取媒体处理器](media-services-rest-get-media-processor.md)测试
