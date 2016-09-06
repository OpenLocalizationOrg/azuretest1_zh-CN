---
ms.openlocfilehash: f82fc8a484761d0dba91f10fac4f4b1768d44c45
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="多语言内容的客户概述" 
    description="本主题概述在提供您使用 Azure 媒体服务的内容所涉及的内容。" 
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


#多语言内容的客户概述

##概述

交付给客户 （流实时事件或视频点播） 的内容时您的目标是将高品质的视频传送到不同的网络条件下的各种设备。 

要实现这一目标︰

- 编码到多比特率 （自适应比特率） 视频流 （这将负责质量和网络条件） 流和 
- 使用介质服务[动态打包](media-services-dynamic-packaging-overview.md)动态重新打包到不同的协议 （这将负责流在不同的设备上） 流。 媒体服务支持传送流技术下自适应比特率︰ HTTP 实时流 (HLS)、 平滑流式处理、 MPEG 划线和 HDS （对 Adobe 至此/访问许可证持有者只）。

本主题概述[内容传递概念](media-services-deliver-content-overview.md#concepts)和链接的主题演示如何执行内容交付[的任务](media-services-deliver-content-overview.md#tasks)。

##<a id="concepts"></a>概念

在交付介质时，下面的列表描述有用的术语和概念。

###动态打包

建议使用动态打包来传递内容。 有关详细信息，请参阅[动态打包](media-services-dynamic-packaging-overview.md)。  

利用动态打包，必须先从中计划交付给您的内容的流终结点获取至少一个点播流计价单位。 有关详细信息，请参阅[如何规模的媒体服务](media-services-manage-origins.md#scale_streaming_endpoints)。

###筛选器和动态清单

媒体服务使您能够定义筛选器为您的资产。 这些筛选器可将允许您的客户能够选择做类似的服务器端规则︰ 仅部分 （而不是播放整个视频），视频播放，或指定客户的设备可处理 （而不是与该资产相关联的所有副本） 的音频和视频节目的子集。 这种资产的过滤被通过客户的请求流视频基于指定筛选器时创建的**动态清单**s。

有关详细信息，请参阅[筛选器和动态清单](media-services-dynamic-manifest-overview.md)。

###定位器

为您的用户提供可用于流或下载您的内容的 URL，首先需要通过创建定位程序"发布"您的资产。  定位器提供入口点来访问资产中包含的文件。 媒体服务支持两种类型的定位器︰ 

- **OnDemandOrigin**定位器，用于流媒体 （例如，MPEG 划线、 HLS，或平滑流式处理） 或渐进式下载文件。
-  **SAS**（访问签名）用于下载到本地计算机上的媒体文件的 URL 定位器。 

**访问策略**用于定义的权限 （如读取、 写入和列表） 和客户端具有访问给定资产的持续时间。 请注意，创建 OrDemandOrigin 定位器时不使用的列表权限 (AccessPermissions.List)。

定位程序已过期。 当使用门户发布您的资产，被创建的定位器使用 100 年的到期日期。 

>[AZURE.NOTE] 如果您使用门户网站在 3 月 2015年之前创建定位程序，创建了定位器两年过期日期。  

若要更新定位符上的到期日期，请使用[其余部分](http://msdn.microsoft.com/library/azure/hh974308.aspx#update_a_locator )或[.NET](http://go.microsoft.com/fwlink/?LinkID=533259) Api。 注意当您更新 SAS 定位器的到期日期，该 URL 将更改。 
 
定位器不用于管理每个用户的访问控制。 要为单个用户的不同访问权限，请使用数字版权管理 (DRM) 解决方案。 有关详细信息，请参阅[保护媒体](http://msdn.microsoft.com/library/azure/dn282272.aspx)。

请注意，当您创建定位程序，可能有 30 秒的延迟，由于 Azure 存储中需要存储和传播的过程。


###自适应流式处理 

自适应的比特率技术允许视频播放器应用程序以确定网络条件并选择从几个比特率。 降低了网络通信，客户端可以选择较低的比特率允许玩家继续播放视频，在视频质量的降低。 根据网络条件改善客户端可以切换为使用提高了视频质量较高的比特率。 Azure 媒体服务支持下的自适应比特率技术︰ HTTP 实时流 (HLS)、 平滑流式处理、 MPEG 划线和 HDS。

为用户提供流 Url，首先必须创建 OnDemandOrigin 定位器。 创建定位程序，使您的基础路径包含您要传输的内容资产。 但是，可以进行流式处理此内容，您需要修改该路径进一步。 若要构造流的清单文件的完整 URL，您必须连接定位器的路径值和 (filename.ism) 的清单文件的名称。 然后，定位程序路径追加 /Manifest 和合适的格式 （如果需要）。 

>[AZURE.NOTE]您还可以通过 SSL 连接传输您的内容。 要做到这一点，请确保您流的 Url 以 HTTPS 开头。 

请注意，您可以只流通过 SSL 是否 2014 9 月 10 日之后, 创建将内容传递的流终结点。 如果您流的 Url 基于在 9 月 10 日之后创建的流终结点，该 URL 包含"streaming.mediaservices.windows.net"（新的格式）。 流的 Url 包含"origin.mediaservices.windows.net"（旧格式） 不支持 SSL。 如果您的 URL 是以旧的格式，并且您希望能够通过 SSL 流，创建新的流终结点。 创建基于新的流终结点的 Url 用于通过 SSL 传输您的内容。 


####流式传输 URL 格式︰

**短划线 MPEG 格式**

{流终结点名称介质服务帐户 name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf) 

示例

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=mpd-time-csf)

**苹果 HTTP 实时流 (HLS) V4 格式**

{流终结点名称介质服务帐户 name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=m3u8-aapl)

**苹果 HTTP 实时流 (HLS) V3 格式**

{流终结点名称介质服务帐户 name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl-v3)
    
    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=m3u8-aapl-v3)


**光滑流格式**

{流终结点名称介质服务帐户 name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest

示例：

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest

**平滑流式 2.0 清单 （旧清单）**

默认情况下平滑流式处理清单格式包含重复的标记 （r 标记）。 但是，某些播放器不支持 r 标记。 这些客户端可以使用禁用 r 标记的格式︰

{流终结点名称介质服务帐户 name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=fmp4-v20)

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=fmp4-v20)

**HDS （对 Adobe 至此/访问许可证持有者仅）**

{流终结点名称介质服务帐户 name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=f4m-f4f)

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=f4m-f4f)


###动态打包

媒体服务提供动态打包以便您可以传递中流媒体服务 MPEG 划线、 HLS、 平滑流式处理 （HDS） 支持的格式的 MP4 或平滑流编码的自适应比特率内容而不必对这些流格式重新打包。 

利用动态打包，您需要执行下列操作︰

- 夹层 （源） 文件进行编码成一组自适应比特率 MP4 文件或自适应的比特率平滑流式处理文件。
- 从中您计划交付给您的内容的流终结点获取至少一个点播流计价单位。 有关详细信息，请参阅[如何比例按需数据流保留单位](media-services-manage-origins.md#scale_streaming_endpoints)。 

用动态包装只需要存储并支付中单个存储格式的文件，介质服务将生成和提供适当的响应，基于客户机的请求。 

请注意，除了可以使用动态打包功能之外，点播流保留的单位为您提供专用的可以购买 200 Mbps 的速度增量的出口产能。 默认情况下，对于哪一台服务器 （例如，计算，出口容量，等等） 的资源共享与所有其他用户的共享实例模型中，配置流式点播。 为了提高点播流吞吐量，建议购买点播流保留单位。

###渐进式下载 

渐进式下载，可开始播放媒体之前已下载整个文件。 您不能渐进式下载.ism * （ismv isma、 ismt、 ismc） 文件。 

渐进式下载内容，请使用定位器的 OnDemandOrigin 类型。 下面的示例演示基于 OnDemandOrigin 类型的定位符 URL:

    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_650kbps_AAC_und_ch2_96kbps.mp4

适用以下考虑因素︰

- 您必须将解密任何您希望从原始服务用于渐进式下载流的加密的存储资产。


###下载

若要下载到客户端设备上的内容，您必须创建 SA 定位器。 SAS 定位器使您访问您的文件所在的 Azure 存储容器。 若要生成下载 URL，您必须嵌入之间的主机和 SAS 签名的文件的名称。 

下面的示例演示基于 SAS 定位符 URL:

    https://test001.blob.core.windows.net/asset-ca7a4c3f-9eb5-4fd8-a898-459cb17761bd/BigBuckBunny.mp4?sv=2012-02-12&se=2014-05-03T01%3A23%3A50Z&sr=c&si=7c093e7c-7dab-45b4-beb4-2bfdff764bb5&sig=msEHP90c6JHXEOtTyIWqD7xio91GtVg0UIzjdpFscHk%3D

以下注意事项︰

- 您必须将解密任何您希望从原始服务用于渐进式下载流的加密的存储资产。
- 在 12 小时内没有完成的下载将失败。



###流终结点

**流终结点**表示可以直接与客户机应用程序，或链接进行进一步分发内容传递网络 (CDN) 提供的内容的流服务。 流服务终结点中的出站流可以实时流或在媒体服务帐户要求资产上的视频。 此外，您可以控制流终结点服务的产能，通过调整流保留的单位处理不断增长的带宽需求。 对于在生产环境中的应用程序，应分配至少一个保留的设备。 有关详细信息，请参阅[如何调整介质服务](media-services-manage-origins.md#scale_streaming_endpoints)。

##<a id="tasks"></a>与提供资产相关的任务


###配置流式处理的终结点

有关流终结点以及如何管理它们的信息的概述，请参阅[如何在媒体的服务帐户管理流式传输端点](media-services-manage-origins.md)。

###上传媒体 

使用**Azure 管理门户网站**、 **.NET**或**REST API，**文件上传。

[AZURE.INCLUDE [media-services-selector-upload-files](../../includes/media-services-selector-upload-files.md)]

###编码的资产

**Azure Media 编码器**使用**Azure 管理门户**、 **.NET**或**REST API，**对进行编码。
 
[AZURE.INCLUDE [media-services-selector-encode](../../includes/media-services-selector-encode.md)]

###配置资产交付策略

配置资产交付策略使用**.NET**或**REST API**。

[AZURE.INCLUDE [media-services-selector-asset-delivery-policy](../../includes/media-services-selector-asset-delivery-policy.md)]

###发布资源

（通过创建定位器） 发布资产使用**Azure 管理门户**或**.NET**。

[AZURE.INCLUDE [media-services-selector-publish](../../includes/media-services-selector-publish.md)]


##相关的主题

[在滚动存储键之后更新媒体服务定位器](media-services-roll-storage-access-keys.md)
 
测试
