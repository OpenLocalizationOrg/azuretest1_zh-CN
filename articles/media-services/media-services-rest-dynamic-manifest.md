---
ms.openlocfilehash: 35e2ed9cdf751c18b29a2390ca638e76ca6ce2f7
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="创建筛选器与媒体服务 REST API，" 
    description="本主题介绍如何创建筛选器，因此您的客户端可以使用它们流的流的特定部分。 介质服务创建动态清单来实现这种选择性的流。" 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="ne" 
    ms.topic="article" 
    ms.date="08/24/2015" 
    ms.author="juliako"/>

#创建筛选器与媒体服务 REST API，

> [AZURE.SELECTOR]
- [.NET](media-services-dotnet-dynamic-manifest.md)
- [REST](media-services-rest-dynamic-manifest.md)


从 2.11 发行版开始，媒体服务使您能够定义筛选器为您的资产。 这些筛选器可将允许您的客户能够选择做类似的服务器端规则︰ 仅部分 （而不是播放整个视频），视频播放，或指定客户的设备可处理 （而不是与该资产相关联的所有副本） 的音频和视频节目的子集。 这种资产的过滤被通过客户的请求流视频基于指定筛选器时创建的**动态清单**s。

与筛选器和动态清单相关的详细信息，请参阅[动态清单概述](media-services-dynamic-manifest-overview.md)。

本主题演示如何使用 REST Api 来创建、 更新和删除筛选器。 

##用于创建筛选器的类型

创建筛选器时，将使用以下类型︰  

- [Filter](http://msdn.microsoft.com/library/azure/mt149056.aspx)
- [AssetFilter](http://msdn.microsoft.com/library/azure/mt149053.aspx)
- [PresentationTimeRange](http://msdn.microsoft.com/library/azure/mt149052.aspx)
- [FilterTrackSelect 和 FilterTrackPropertyCondition](http://msdn.microsoft.com/library/azure/mt149055.aspx)



>[AZURE.NOTE] 使用介质服务 REST API 时, 应该注意下列事项︰
>
>在访问中介质服务的实体，您必须设置特定的标头字段和值 HTTP 请求中。 有关详细信息，请参阅[设置介质服务 REST API 的开发](media-services-rest-how-to-use.md)。

>已成功连接到 https://media.windows.net 之后, 您将收到指定另一个媒体服务 URI 的 301 重定向。 必须使[连接到介质服务使用 REST API，](media-services-rest-connect_programmatically.md)所述新的 URI 对的后续调用。 


##创建筛选器

###创建全局筛选器

若要创建全局筛选器，请使用以下 HTTP 请求︰  

####HTTP 请求

请求标头

    POST https://media.windows.net/API/Filters HTTP/1.1 
    DataServiceVersion:3.0 
    MaxDataServiceVersion: 3.0 
    Content-Type: application/json 
    Accept: application/json 
    Accept-Charset: UTF-8 
    Authorization: Bearer <token value> 
    x-ms-version: 2.11 
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000 
    Host:media.windows.net 

请求正文 

    {  
       "Name":"GlobalFilter",
       "PresentationTimeRange":{  
          "StartTimestamp":"0",
          "EndTimestamp":"9223372036854775807",
          "PresentationWindowDuration":"12000000000",
          "LiveBackoffDuration":"0",
          "Timescale":"10000000"
       },
       "Tracks":[  
          {  
             "PropertyConditions":
                  [  
                {  
                   "Property":"Type",
                   "Value":"audio",
                   "Operator":"Equal"
                },
                {  
                   "Property":"Bitrate",
                   "Value":"0-2147483647",
                   "Operator":"Equal"
                }
             ]
          }
       ]
    }




####HTTP 响应
    
    HTTP/1.1 201 Created 

###创建本地 AssetFilters

若要创建本地 AssetFilter，请使用以下 HTTP 请求︰  

####HTTP 请求

请求标头

    POST https://media.windows.net/API/AssetFilters HTTP/1.1 
    DataServiceVersion: 3.0 
    MaxDataServiceVersion: 3.0 
    Content-Type: application/json 
    Accept: application/json 
    Accept-Charset: UTF-8 
    Authorization: Bearer <token value> 
    x-ms-version: 2.11 
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000 
    Host: media.windows.net  

请求正文 

    {   
       "Name":"AssetFilter", 
       "ParentAssetId":"nb:cid:UUID:536e555d-1500-80c3-92dc-f1e4fdc6c592", 
       "PresentationTimeRange":{   
          "StartTimestamp":"0", 
          "EndTimestamp":"9223372036854775807", 
          "PresentationWindowDuration":"12000000000", 
          "LiveBackoffDuration":"0", 
          "Timescale":"10000000" 
       }, 
       "Tracks":[   
          {   
             "PropertyConditions": 
                  [   
                {   
                   "Property":"Type", 
                   "Value":"audio", 
                   "Operator":"Equal" 
                }, 
                {   
                   "Property":"Bitrate", 
                   "Value":"0-2147483647", 
                   "Operator":"Equal" 
                } 
             ] 
          } 
       ] 
    } 

####HTTP 响应 

    HTTP/1.1 201 Created 
    . . . 

##列表筛选器

###AMS 帐户中获取所有全局**筛选器**s

列出的筛选器，请使用以下 HTTP 请求︰ 

####HTTP 请求
     
    GET https://media.windows.net/API/Filters HTTP/1.1 
    DataServiceVersion:3.0 
    MaxDataServiceVersion: 3.0 
    Accept: application/json 
    Accept-Charset: UTF-8 
    Authorization: Bearer <token value> 
    x-ms-version: 2.11 
    Host: media.windows.net 
    
### 获取与资产相关联的**AssetFilter**s

####HTTP 请求

    GET https://media.windows.net/API/Assets('nb%3Acid%3AUUID%3A536e555d-1500-80c3-92dc-f1e4fdc6c592')/AssetFilters HTTP/1.1 
    DataServiceVersion: 3.0 
    MaxDataServiceVersion: 3.0 
    Accept: application/json 
    Accept-Charset: UTF-8 
    Authorization: Bearer <token value> 
    x-ms-version: 2.11 
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000 
    Host: media.windows.net 

###获取基于其 Id **AssetFilter**

####HTTP 请求

    GET https://media.windows.net/API/AssetFilters('nb%3Acid%3AUUID%3A536e555d-1500-80c3-92dc-f1e4fdc6c592__%23%23%23__TestFilter') HTTP/1.1 
    DataServiceVersion: 3.0 
    MaxDataServiceVersion: 3.0 
    Accept: application/json 
    Accept-Charset: UTF-8 
    Authorization: Bearer <token value> 
    x-ms-version: 2.11 
    x-ms-client-request-id: 00000000


##更新筛选器
 
使用修补程序，传或合并使用新属性值更新一个筛选器。  有关这些操作的详细信息，请参阅[修补程序、 放、 合并](http://msdn.microsoft.com/library/dd541276.aspx)。
 
如果您在更新一个筛选器，它可以采取 2 分钟的时间以刷新规则流终结点。 如果已通过使用此筛选器提供内容 （并在代理服务器和 CDN 缓存缓存），则更新此筛选器可能导致播放机故障。 这是建议，如果要更新筛选器后清除缓存。 如果此选项是不可能的可以考虑使用不同的筛选器。  
 
###更新全局筛选器

若要更新全局筛选器，请使用以下 HTTP 请求︰ 

####HTTP 请求
 
请求标头︰ 

    MERGE https://media.windows.net/API/Filters('filterName') HTTP/1.1 
    DataServiceVersion:3.0 
    MaxDataServiceVersion: 3.0 
    Content-Type: application/json 
    Accept: application/json 
    Accept-Charset: UTF-8 
    Authorization: Bearer <token value> 
    x-ms-version: 2.11 
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000 
    Host: media.windows.net 
    Content-Length: 384
    
请求正文︰ 
    
    { 
       "Tracks":[   
          {   
             "PropertyConditions": 
             [   
                {   
                   "Property":"Type", 
                   "Value":"audio", 
                   "Operator":"Equal" 
                }, 
                {   
                   "Property":"Bitrate", 
                   "Value":"0-2147483647", 
                   "Operator":"Equal" 
                } 
             ] 
          } 
       ] 
    } 

###更新本地 AssetFilters

若要更新本地筛选器，请使用以下 HTTP 请求︰ 

####HTTP 请求

请求标头︰ 

    MERGE https://media.windows.net/API/AssetFilters('nb%3Acid%3AUUID%3A536e555d-1500-80c3-92dc-f1e4fdc6c592__%23%23%23__TestFilter')  HTTP/1.1 
    DataServiceVersion: 3.0 
    MaxDataServiceVersion: 3.0 
    Content-Type: application/json 
    Accept: application/json 
    Accept-Charset: UTF-8 
    Authorization: Bearer <token value> 
    x-ms-version: 2.11 
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000 
    Host: media.windows.net 
    
请求正文︰ 
    
    { 
       "Tracks":[   
          {   
             "PropertyConditions": 
             [   
                {   
                   "Property":"Type", 
                   "Value":"audio", 
                   "Operator":"Equal" 
                }, 
                {   
                   "Property":"Bitrate", 
                   "Value":"0-2147483647", 
                   "Operator":"Equal" 
                } 
             ] 
          } 
       ] 
    } 


##删除筛选器


###删除全局筛选器

要删除全局筛选器，请使用以下 HTTP 请求︰
    
####HTTP 请求

    DELETE https://media.windows.net/api/Filters('GlobalFilter') HTTP/1.1 
    DataServiceVersion:3.0 
    MaxDataServiceVersion: 3.0 
    Accept: application/json 
    Accept-Charset: UTF-8 
    Authorization: Bearer <token value> 
    x-ms-version: 2.11 
    Host: media.windows.net 


###删除本地 AssetFilters

若要删除本地 AssetFilter，请使用以下 HTTP 请求︰

####HTTP 请求

    DELETE https://media.windows.net/API/AssetFilters('nb%3Acid%3AUUID%3A536e555d-1500-80c3-92dc-f1e4fdc6c592__%23%23%23__LocalFilter') HTTP/1.1 
    DataServiceVersion: 3.0 
    MaxDataServiceVersion: 3.0 
    Accept: application/json 
    Accept-Charset: UTF-8 
    Authorization: Bearer <token value> 
    x-ms-version: 2.11 
    Host: media.windows.net 

##生成流使用筛选器的 Url

有关如何发布和提供您的资产的信息，请参阅[为客户概述提供内容](media-services-deliver-content-overview.md)。


下面的示例演示如何将筛选器添加到流的 Url。

**MPEG 短划线** 

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=mpd-time-csf, filter=MyFilter)

**苹果 HTTP 实时流 (HLS) V4**

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=m3u8-aapl, filter=MyFilter)

**苹果 HTTP 实时流 (HLS) V3**

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=m3u8-aapl-v3, filter=MyFilter)

**平滑流式处理**

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(filter=MyFilter)


**HDS**

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=f4m-f4f, filter=MyFilter)



##请参见 

[动态清单概述](media-services-dynamic-manifest-overview.md)
 

 

测试
