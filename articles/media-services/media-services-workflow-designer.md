---
ms.openlocfilehash: 24e07bbc1950427221ac993a55adcc1e0fc77ad5
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="使用工作流设计器创建高级编码的工作流" 
    description="了解如何使用工作流设计器创建高级编码的工作流。" 
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


#使用工作流设计器创建高级编码的工作流

##概述
**工作流设计**器用来使用**媒体编码高级工作流**创建工作流/图编码的独立工具。

此工具还可以用于修改[现有工作流](media-services-workflow-designer.md#existing_workflows)。 

>[AZURE.NOTE]若要获得一份工作流设计器工具，请联系 mepd@microsoft.com。


工作流文件创建后，它可以作为一项资产，上载，然后可用于媒体文件进行编码。 有关如何使用**媒体编码高级工作流**使用**.NET**编码的信息，请参阅[高级媒体编码器高级工作流与编码](media-services-encode-with-premium-workflow.md)。

##<a id="existing_workflows"></a>修改现有的工作流

可以使用设计器工具修改默认工作流文件。 获取默认工作流文件[此处](https://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows)。 该文件夹还包含这些文件的说明。

以下视频演示了如何使用设计器。

###一天 1-快速入门

第 1 天视频涵盖了︰

- 设计器的概述
- 基本工作流 –"Hello World"
- 创建多个输出使用 Azure 媒体服务流的 MP4 文件

> [AZURE.VIDEO azure-premium-encoder-workflow-designer-training-videos-day-1]

###第 2 天

第 2 天视频涵盖了︰

- 不同的源代码文件方案 – 处理音频
- 用高级逻辑的工作流
- 图阶段

> [AZURE.VIDEO azure-premium-encoder-workflow-designer-training-videos-day-2]

###第 3 天

第 3 天视频涵盖了︰

- 在工作流中的蓝图内部脚本
- 当前编码器的限制条件
- 问与答
 
> [AZURE.VIDEO azure-premium-encoder-workflow-designer-training-videos-day-3]

##请参见

[Azure 津贴编码器工作流设计器培训视频](http://johndeutscher.com/2015/07/06/azure-premium-encoder-workflow-designer-training-videos/)

测试
