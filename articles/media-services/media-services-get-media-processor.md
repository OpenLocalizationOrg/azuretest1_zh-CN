---
ms.openlocfilehash: 193986a0e0e85f3a40543302951b9c461fe92854
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="如何创建媒体处理器 |Microsoft Azure" 
    description="了解如何创建媒体处理器组件进行编码、 转换格式、 加密或解密的 Azure 媒体服务的媒体内容。 代码示例编写的 C# 和用于.NET 的媒体服务 SDK。" 
    services="media-services" 
    documentationCenter="" 
    authors="juliako" 
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


#如何︰ 获取媒体处理器实例

> [AZURE.SELECTOR]
- [.NET](media-services-get-media-processor.md)
- [REST](media-services-rest-get-media-processor.md)
 

##概述

媒体服务，媒体处理器是处理特定的处理任务，如编码，组件中设置的格式转换，加密或解密的媒体内容。 创建一个任务进行编码、 加密，或将媒体内容的格式转换时，通常需要创建媒体处理器。

下表提供的名称和说明的每个可用的媒体处理器。

媒体处理器名称|说明|详细信息
---|---|---
Azure Media 编码器|允许您运行使用 Azure Media 编码器的编码任务。|[Azure Media 编码器](media-services-encode-asset.md#azure_media_encoder)
媒体编码标准|允许您运行使用媒体编码标准的编码任务。|[Azure Media 编码器](media-services-encode-asset.md#media_encoder_standard)
媒体编码器高级工作流|允许您运行编码使用媒体编码高级工作流的任务。|[媒体编码器高级工作流](media-services-encode-asset.md#media_encoder_premium_wokrflow)
Azure 媒体索引器| 使您可以将媒体文件和内容可搜索，以及生成闭合字幕轨道和关键字。|[媒体文件使用 Azure 媒体索引器的索引](media-services-index-content.md)。
Azure 媒体 Hyperlapse （预览）|使您能够消除视频稳定化与视频中的"碰撞"。 此外允许您在可耗用剪辑加快您的内容。|        [Azure 媒体 Hyperlapse](http://azure.microsoft.com/blog/?p=286281&preview=1&_ppp=61e1a0b3db)</a>
存储解密| 可用于解密加密使用的加密存储的媒体资产。|N/A
Windows Azure 媒体包装程序|允许您将.mp4 媒体资产转换为平滑流式处理的格式。 此外，还允许您从平滑流式处理到苹果 HTTP 实时流 (HLS) 格式转换媒体资产。|[任务预设 Azure 媒体包装程序字符串](http://msdn.microsoft.com/library/hh973635.aspx)
Windows Azure 媒体加密器|可以加密使用 PlayReady 保护的媒体资产。|[任务预设 Azure 媒体包装程序字符串](http://msdn.microsoft.com/library/hh973610.aspx)

##获得 MediaProcessor

下面的方法演示如何获取媒体处理器实例。 此代码示例假定一个名为**_context**来引用服务器上下文，部分中所述的模块级变量使用[方法︰ 连接到介质服务以编程方式]。

    private static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
    {
         var processor = _context.MediaProcessors.Where(p => p.Name == mediaProcessorName).
            ToList().OrderBy(p => new Version(p.Version)).LastOrDefault();
    
        if (processor == null)
            throw new ArgumentException(string.Format("Unknown media processor", mediaProcessorName));
    
        return processor;
    }

##下一步行动
您已经知道了如何获取媒体处理器实例，转到[如何编码资产][]主题将向您展示如何使用 Azure Media 编码器编码资产的。

[如何对资产进行编码]: media-services-encode-asset.md
[任务预设 Azure 媒体编码字符串]: http://msdn.microsoft.com/library/jj129582.aspx
[如何︰ 以编程方式连接到媒体服务]: ../media-services-set-up-computer/ 

测试
