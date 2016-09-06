---
ms.openlocfilehash: ef776b03458042184310e08c012a38f34fdc731a
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="动态打包概述" 
    description="主题使和动态打包的概述。" 
    authors="Juliako" 
    manager="dwrede" 
    editor="" 
    services="media-services" 
    documentationCenter=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/11/2015"  
    ms.author="juliako"/>


#动态打包 

##概述

Microsoft Azure 介质服务可用于传送多种媒体源文件格式，媒体流格式，和客户端技术的各种格式的内容保护 (例如，iOS，XBOX，Silverlight，Windows 8)。 这些客户端能够理解不同的协议，例如 iOS 需要 HTTP 实时流 (HLS) V4 格式和 Silverlight 和 Xbox 需要平滑流式处理。 如果您有一组自适应比特率 （多比特率） 的 MP4 （ISO 基本媒体 14496-12） 文件或一组自适应比特率平滑流式处理的文件要为了解 MPEG 划线、 HLS 或平滑流式处理的客户端提供服务，您应该利用媒体服务动态打包。  

动态打包只需是创建包含一组自适应比特率 MP4 文件或自适应的比特率平滑流式处理文件的资产。 然后，根据清单或片段请求，点播流中指定格式，服务器将确保您在您选择的协议接收流。 因此，您只需将存储并支付中单个存储格式的文件和媒体服务将生成并提供适当的响应，基于客户机的请求。

下图显示了传统编码和静态包装工作流程。

![静态编码](./media/media-services-dynamic-packaging-overview/media-services-static-packaging.png)

下图显示了动态打包工作流。

![动态编码](./media/media-services-dynamic-packaging-overview/media-services-dynamic-packaging.png)


>[AZURE.NOTE]利用动态打包，必须先从中计划交付给您的内容的流终结点获取至少一个点播流计价单位。 有关详细信息，请参阅[如何规模的媒体服务](media-services-manage-origins.md#scale_streaming_endpoints)。

##常见方案

1. 将上载输入的文件 （称为夹层文件）。 例如，H.264、 MP4 或 WMV （有关受支持的格式见媒体服务编码器支持格式列表）。
 
1. 对 H.264 MP4 自适应的比特率设置夹层文件进行编码。
 
1. 发布包含 MP4 通过创建手动定位程序集的自适应比特率的资产。
 
1. 生成流的 Url 来访问和传输您的内容。
 
>[AZURE.NOTE]不是所有的 MP4 文件格式所支持的动态打包，有关详细信息，请参阅[动态打包格式不受支持](media-services-dynamic-packaging-overview.md#unsupported_formats)。

##准备资产动态流

准备您的资产进行动态流必须有两个选项︰ 

- 传主文件并生成使用 Azure 媒体编码 H.264 MP4 自适应的比特率设置。
- 将现有的自适应的比特率设置上载并进行验证使用媒体包装程序。

###传主文件并生成使用 Azure 媒体编码 H.264 MP4 自适应的比特率设置

有关如何上载和编码信息资产请参阅以下文章︰


使用**Azure 管理门户网站**、 **.NET**或**REST API，**文件上传。

[AZURE.INCLUDE [media-services-selector-upload-files](../../includes/media-services-selector-upload-files.md)]

**Azure Media 编码器**使用**Azure 管理门户**、 **.NET**或**REST API，**对进行编码。
 
[AZURE.INCLUDE [media-services-selector-encode](../../includes/media-services-selector-encode.md)]


###将上载现有自适应的比特率设置并进行验证使用的媒体包装程序

您通常想要执行此任务，如果您要上载一套不与媒体服务编码器编码的自适应比特率 MP4 文件。 [验证自适应比特率 MP4s 编码与外部编码器](https://msdn.microsoft.com/library/azure/dn750842.aspx)主题演示了如何完成此任务。

##流式传输到客户端内容

自适应的比特率设置之后，可以通过创建手动定位程序发布您的资产和 （对于 Adobe 至此/访问许可证） 的平滑 Steaming、 MPEG 划线、 HLS 和 HDS 撰写流的 Url。

有关如何创建定位程序和动态打包用于传输您的内容的信息，请参阅下列主题︰

[提供给客户概述的内容](media-services-deliver-content-overview.md)。 

配置资产交付策略使用**.NET**或**REST API**。

[AZURE.INCLUDE [media-services-selector-asset-delivery-policy](../../includes/media-services-selector-asset-delivery-policy.md)]

（通过创建定位器） 发布资产使用**Azure 管理门户**或**.NET**。

[AZURE.INCLUDE [media-services-selector-publish](../../includes/media-services-selector-publish.md)]


##<a id="unsupported_formats"></a>动态打包不支持的格式

下面的源代码文件格式不受动态打包。

- 杜比数字加上 mp4 文件。
- 杜比数字加上平稳的文件。 测试
