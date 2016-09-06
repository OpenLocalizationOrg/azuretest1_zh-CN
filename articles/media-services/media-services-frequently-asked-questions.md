---
ms.openlocfilehash: 04edfa89832fff4df7da5b016952b849683ac04b
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="常见问题及的解答" 
    description="常见问题 (Faq)" 
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


#常见问题及的解答  

##概述

问︰ 如何扩展索引？

答︰ 保留的单位都相同的编码和索引任务。 如何[缩放编码保留单位](media-services-how-to-scale.md)按照说明进行操作。 **注意**索引器性能不受保留的设备类型。

问︰ 我上载、 编码，并发布视频。 当我试图将其流式处理时不播放视频的原因是什么？ 

答︰ 一种最常见的原因是您没有至少一个保留流计价单位正试图播放的流终结点分配。  如何[缩放流保留单位](media-services-how-to-scale.md)按照说明进行操作。

问︰ 我如何合成，对实时流？ 

答︰ 合成，对实时流当前中未提供 Azure 媒体服务，因此需要预先撰写您的计算机上。

问︰ 我可以使用 Azure CDN 实时流？ 

答︰ 媒体服务支持与 Azure CDN 的集成 （有关详细信息，请参阅[如何在媒体的服务帐户管理流式传输端点](media-services-manage-origins.md#enable_cdn)）。  您可以使用流与 CDN 的生存。 Azure 媒体服务提供了平滑流式、 HLS 和 MPEG-短线输出。 所有这些格式使用 HTTP 传输数据，获取 HTTP 缓存的优点。 在实时流实际视频/音频数据被划分成碎片和 CDN 中获取缓存此单个片段。 仅刷新数据需要是的清单数据。 CDN 定期刷新清单数据。

问︰ 没有 Azure 媒体服务支持存储图像？

答︰ 如果您只希望存储 JPEG 或 PNG 图像，则应该保留那些在 Azure Blob 存储。 不没有将它们放入介质服务帐户中除非您想要将其与您的视频或音频的资产不会带来任何益处。 或者，如果您可能必须将图像用作叠加在视频编码器的需要。 媒体服务编码器支持的视频，上面的覆盖图像并且什么它列出 JPEG 和 PNG 支持输入格式。 有关详细信息，请参阅[创建叠加](https://msdn.microsoft.com/library/azure/dn640496.aspx)。

问︰ 如何复制资产从一个介质服务帐户到另一个。 

答︰ 将资源复制到另一个介质服务帐户，使用[IAsset.Copy](https://github.com/Azure/azure-sdk-for-media-services-extensions/blob/dev/MediaServices.Client.Extensions/IAssetExtensions.cs#L354)扩展方法可用在[Azure 媒体服务.NET SDK 扩展](https://github.com/Azure/azure-sdk-for-media-services-extensions/)存储库中。 有关详细信息，请参阅[此](https://social.msdn.microsoft.com/Forums/azure/28912d5d-6733-41c1-b27d-5d5dff2695ca/migrate-media-services-across-subscription?forum=MediaServices)论坛线程。 测试
