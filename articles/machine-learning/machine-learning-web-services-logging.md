---
ms.openlocfilehash: d1ff14b6f76199f9739d41661b6578339c8c6ea0
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="机器学习的 web 服务的日志记录 |Microsoft Azure" 
    description="了解如何启用日志记录的机器学习的 web 服务。 日志提供了其他信息以帮助解决 Api。" 
    services="machine-learning" 
    documentationCenter="" 
    authors="raymondlaghaeian" 
    manager="paulettm" 
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data" 
    ms.date="06/30/2015"
    ms.author="raymondl;garye"/>

#对机器学习的 web 服务启用日志记录  

本文档提供有关 Azure ML Web 服务的日志记录功能的信息。 启用日志记录功能在 web 服务中提供了其他信息以帮助解决超过 Api 只、 错误号和消息。  

-   如何启用日志记录在 Web 服务︰   
    -   若要启用日志记录，您需要访问 Web 服务面板从 Azure 门户，然后单击要通过单击是按钮来启用它，然后单击保存。  
-   什么是启用日志记录的效果︰  
    -   当启用日志记录时，所有的默认终结点从诊断/错误都会记录到 Azure 存储帐户链接在一起，用户的工作区。 您可以看到此存储帐户的 Azure 门户的仪表板视图中 （快速概览部分的底部） 他们的工作区。  
-   其中它是可用的︰  
    -   我们当前的 Web 服务的默认终结点中启用日志记录。 我们很快就会提供此功能的用户可以在 Azure 的门户网站中创建其他终结点。  
-   如何查看日志︰  
    -   可以使用任何可用于探索 Azure 存储帐户的几个工具查看日志。 最简单的方法可能只需导航到 Azure 门户中存储帐户，然后单击容器选项卡上。 然后，您会看到一个名为**ml 诊断**的容器。 此容器包含所有 web 服务终结点与此存储帐户关联的所有工作区的所有诊断的信息。  
-   什么是日志 blob 详细信息︰  
    -   每个容器中的 blob 包含完全下列情况之一的诊断信息︰
        -   执行批处理执行方法  
        -   在请求-响应方法执行  
        -   初始化的请求-响应容器  
    -   每个 blob 的名称有以下形式的前缀: {区 Id} {Web 服务 Id} 的 {端点 Id} {日志类型} /  
-   日志类型采用以下值之一︰  
    批处理  
    分数/请求  
    初始分数 /  

 

测试
