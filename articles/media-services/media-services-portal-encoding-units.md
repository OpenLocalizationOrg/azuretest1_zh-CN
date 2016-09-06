---
ms.openlocfilehash: 6380b4abfd5f581f0a4f1a8b9c66d108091e1ce0
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="如何扩展编码保留单位" 
    description="了解如何调整通过指定数按需数据流保留单元和编码的预留单位，要将您的帐户以使用配置的介质服务。" 
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


#如何扩展编码

> [AZURE.SELECTOR]
- [.NET](media-services-dotnet-encoding-units.md)
- [门户](media-services-portal-encoding-units.md)
- [REST](https://msdn.microsoft.com/library/azure/dn859236.aspx)
- [Java](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)

##概述

媒体服务帐户程序与保留的设备类型，它确定处理编码作业的速度。 您可以选择以下保留的设备类型之间︰**基本**、**标准**或**津贴**。 例如，同一编码作业运行速度更快时使用**基本**类型与**标准**预留的单位类型比较。 有关详细信息，请参阅[保留单位类型编码](http://azure.microsoft.com/blog/author/milanga)。

除了指定保留的设备类型，您可以指定要设置您的帐户编码保留的单位。 调配的编码保留单位数确定的数目能够在给定帐户中同时处理介质任务。 例如，如果您的帐户具有 5 个预留的单位，将同时只要运行 5 介质任务，然后为没有要处理的任务。 剩余的任务在队列中等待，将按顺序处理正在运行的任务完成时，就立即获得领取。 如果帐户没有设置任何保留的单位，然后任务将选取顺序。 在这种情况下，完成一项任务和下一个启动之间的等待时间将取决于资源的可用性在系统中。

若要更改保留的设备类型和编码保留的单位的数目，请执行以下操作︰

1. 在[管理门户网站](https://manage.windowsazure.com/)中，单击**媒体服务**。 然后，单击媒体服务的名称。

2. 选择**编码**页。 

    若要更改**保留的设备类型**，请按基本、 标准版或津贴。 

    若要更改保留的保留所选的设备类型单位数，使用**编码**滑块。 
    
    
    ![处理器页](./media/media-services-portal-encoding-units/media-services-encoding-scale.png)

      
    >[AZURE.NOTE] 下面的数据中心不能提供最优保留单位类型︰ 新加坡、 香港、 大阪和北京，上海。

3. 按保存按钮保存所做的更改。

    新的编码保留的单位被分配时立即按保存。

    >[AZURE.NOTE] 单位指定的 24 小时内的最高数目用于计算成本。

##配额和限制

有关配额和限制以及如何打开支持票的信息，请参阅[配额和限制](media-services-quotas-and-limitations.md)。




 
测试
