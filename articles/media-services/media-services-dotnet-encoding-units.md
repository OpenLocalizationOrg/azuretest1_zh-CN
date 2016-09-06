---
ms.openlocfilehash: 5fa4e699d7fc0a56007d8dd082bb278dadad27e0
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="如何添加编码单元" 
    description="了解如何添加.net 的编码单位如何"  
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




#如何调整与.NET SDK 编码


> [AZURE.SELECTOR]
- [门户](media-services-portal-encoding-units.md)
- [.NET](media-services-dotnet-encoding-units.md)
- [REST](https://msdn.microsoft.com/library/azure/dn859236.aspx)

##概述

媒体服务帐户程序与保留的设备类型，它确定处理编码作业的速度。 您可以选择以下保留的设备类型之间︰ 基本、 标准版或津贴。 例如，同一编码作业运行速度更快时使用标准保留的单位类型比较的基本类型。 有关详细信息，请参阅由[米兰 Gada](http://azure.microsoft.com/blog/author/milanga/)写入"保留单位类型编码"博客。

除了指定保留的设备类型，您可以指定要设置您的帐户编码保留的单位。 调配的编码保留单位数确定的数目能够在给定帐户中同时处理介质任务。 例如，如果您的帐户具有 5 个预留的单位，将同时只要运行 5 介质任务，然后为没有要处理的任务。 剩余的任务在队列中等待，将按顺序处理正在运行的任务完成时，就立即获得领取。 如果帐户没有设置任何保留的单位，然后任务将选取顺序。 在这种情况下，完成一项任务和下一个启动之间的等待时间将取决于资源的可用性在系统中。

若要更改保留的设备类型和编码保留使用.NET SDK 的单位数，请执行以下操作︰

    IEncodingReservedUnit encodingBasicReservedUnit = _context.EncodingReservedUnits.FirstOrDefault();
    encodingBasicReservedUnit.ReservedUnitType = ReservedUnitType.Basic;
    encodingBasicReservedUnit.Update();
    Console.WriteLine("Reserved Unit Type: {0}", encodingBasicReservedUnit.ReservedUnitType);
    
    encodingBasicReservedUnit.CurrentReservedUnits = 2;
    encodingBasicReservedUnit.Update();
    
    Console.WriteLine("Number of reserved units: {0}", encodingBasicReservedUnit.CurrentReservedUnits);

##打开支持票

默认情况下每个介质服务帐户可以扩展到最多 25 中，编码和 5 点播流保留单位。 您可以通过打开支持票请求更高的限制。

###打开支持票

若要打开支持票据执行以下操作︰

1. 单击[获取支持](https://manage.windowsazure.com/?getsupport=true)。 如果尚未登录，将提示您输入凭据。

1. 选择您的订购。
 
1. 在支持类型下，选择"技术"。
 
1. 单击"创建票证"。 
 
1. 选择"Azure 媒体服务"在产品列表中显示在下一页上。
 
1. "问题类型"中选择适合于您的问题。
 
1. 单击继续。
 
1. 按照第二页上的说明进行操作，然后输入有关您的问题的详细信息。   
 
1. 单击提交以打开该票证。
 

 
测试
