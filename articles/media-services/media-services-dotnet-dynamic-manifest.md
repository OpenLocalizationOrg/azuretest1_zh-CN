---
ms.openlocfilehash: 5743ea1a6c1a6f5c33d5c30cc50d5d744f90dc08
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="使用介质服务.NET SDK 创建筛选器" 
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
    ms.date="08/14/2015" 
    ms.author="juliako"/>


#使用介质服务.NET SDK 创建筛选器

> [AZURE.SELECTOR]
- [.NET](media-services-dotnet-dynamic-manifest.md)
- [REST](media-services-rest-dynamic-manifest.md)

从 2.11 发行版开始，媒体服务使您能够定义筛选器为您的资产。 这些筛选器可将允许您的客户能够选择做类似的服务器端规则︰ 仅部分 （而不是播放整个视频），视频播放，或指定客户的设备可处理 （而不是与该资产相关联的所有副本） 的音频和视频节目的子集。 这种资产的过滤被通过客户的请求流视频基于指定筛选器时创建的**动态清单**s。

与筛选器和动态清单相关的详细信息，请参阅[动态清单概述](media-services-dynamic-manifest-overview.md)。

本主题演示如何使用媒体服务.NET SDK 来创建、 更新和删除筛选器。 


注意是否您在更新一个筛选器，它可能需要 2 分钟的时间进行流式处理的终结点，以刷新规则。 如果已通过使用此筛选器提供内容 （并在代理服务器和 CDN 缓存缓存），则更新此筛选器可能导致播放机故障。 这是建议，如果要更新筛选器后清除缓存。 如果此选项是不可能的可以考虑使用不同的筛选器。 

##用于创建筛选器的类型

创建筛选器时，将使用以下类型︰ 

- **IStreamingFilter**。  这种类型基于 REST API，下面的[筛选器](http://msdn.microsoft.com/library/azure/mt149056.aspx)
- **IStreamingAssetFilter**。 这种类型基于以下的 REST API， [AssetFilter](http://msdn.microsoft.com/library/azure/mt149053.aspx)
- **PresentationTimeRange**。 这种类型基于以下的 REST API， [PresentationTimeRange](http://msdn.microsoft.com/library/azure/mt149052.aspx)
- **FilterTrackSelectStatement**和**IFilterTrackPropertyCondition**。 这些类型基于以下 REST Api [FilterTrackSelect 和 FilterTrackPropertyCondition](http://msdn.microsoft.com/library/azure/mt149055.aspx)


##创建/更新/读/删除全局筛选器

下面的代码演示如何使用.NET 创建、 更新、 读取和删除资源筛选器。
    
    string filterName = "GlobalFilter_" + Guid.NewGuid().ToString();
                
    List<FilterTrackSelectStatement> filterTrackSelectStatements = new List<FilterTrackSelectStatement>();
    
    FilterTrackSelectStatement filterTrackSelectStatement = new FilterTrackSelectStatement();
    filterTrackSelectStatement.PropertyConditions = new List<IFilterTrackPropertyCondition>();
    filterTrackSelectStatement.PropertyConditions.Add(new FilterTrackNameCondition("Track Name", FilterTrackCompareOperator.NotEqual));
    filterTrackSelectStatement.PropertyConditions.Add(new FilterTrackBitrateRangeCondition(new FilterTrackBitrateRange(0, 1), FilterTrackCompareOperator.NotEqual));
    filterTrackSelectStatement.PropertyConditions.Add(new FilterTrackTypeCondition(FilterTrackType.Audio, FilterTrackCompareOperator.NotEqual));
    filterTrackSelectStatements.Add(filterTrackSelectStatement);
    
    // Create
    IStreamingFilter filter = _context.Filters.Create(filterName, new PresentationTimeRange(), filterTrackSelectStatements);
    
    // Update
    filter.PresentationTimeRange = new PresentationTimeRange(timescale: 500);
    filter.Update();
    
    // Read
    var filterUpdated = _context.Filters.FirstOrDefault();
    Console.WriteLine(filterUpdated.Name);

    // Delete
    filter.Delete();


##创建/更新/读/删除资产筛选器

下面的代码演示如何使用.NET 创建、 更新、 读取和删除资源筛选器。

    
    string assetName = "AssetFilter_" + Guid.NewGuid().ToString();
    var asset = _context.Assets.Create(assetName, AssetCreationOptions.None);
    
    string filterName = "AssetFilter_" + Guid.NewGuid().ToString();
    
        
    // Create
    IStreamingAssetFilter filter = asset.AssetFilters.Create(filterName,
                                        new PresentationTimeRange(), 
                                        new List<FilterTrackSelectStatement>());
    
    // Update
    filter.PresentationTimeRange = 
            new PresentationTimeRange(start: 6000000000, end: 72000000000);
    
    filter.Update();
    
    // Read
    asset = _context.Assets.Where(c => c.Id == asset.Id).FirstOrDefault();
    var filterUpdated = asset.AssetFilters.FirstOrDefault();
    Console.WriteLine(filterUpdated.Name);
    
    // Delete
    filterUpdated.Delete();
    



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
