---
ms.openlocfilehash: bb8fa11b855cef93412b59cd8de3b60a2bff4584
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="监视使用情况和 Azure 搜索服务中的统计信息" 
   description="跟踪的 Azure 搜索的资源消耗和索引大小" 
   services="search" 
   documentationCenter="" 
   authors="HeidiSteen" 
   manager="mblythe" 
   editor=""
   tags="azure-portal"/>

<tags
   ms.service="search"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="required" 
   ms.date="07/08/2015"
   ms.author="heidist"/>

# 监视使用情况和 Azure 搜索服务中的统计信息

跟踪索引和文档大小的增长可以帮助您达到的上限已建立您的服务之前主动调整产能。 

监视资源使用情况，计数和统计都轻松查看在[Azure 的门户](https://portal.azure.com)，但还可以获取信息以编程方式生成自定义的服务管理工具。 本文介绍了这两种方法的步骤。

##在门户中查看统计和指标 

1. 登录到[Azure 的门户](https://portal.azure.com)。 

2. 打开 Azure 搜索服务的服务操控板。 在主页页面上找不到服务的拼贴或从 JumpBar 上浏览您可以浏览到该服务。 有关分步说明，请参阅[创建服务](search-create-service-portal.md)。

使用情况部分包括仪表盘告诉您可用资源的哪个部分正在使用中。

  ![][1]

撤回的共享的服务都有一个复制副本的最大，每个分区。 此外，它仅在总或 50 MB 的数据支持 10000 个文档，准。

##获取使用 REST API 索引统计信息

Azure 搜索 REST API 和.NET SDK 提供了以编程方式访问服务标准。  如果您使用[索引器](https://msdn.microsoft.com/library/azure/dn946891.aspx)从 SQL Azure 数据库或 DocumentDB，其他 API 加载索引是可用来获取您需要的数字。 

  + [获取索引统计信息](https://msdn.microsoft.com/library/azure/dn798942.aspx)
  + [计数的文档](https://msdn.microsoft.com/library/azure/dn798924.aspx)
  + [获取索引器状态](https://msdn.microsoft.com/library/azure/dn946884.aspx)

## 下一步行动

检查[限制和容量](https://msdn.microsoft.com/library/azure/dn798934.aspx)来确定分区和复制副本如果现有产能不足，您将需要的组合。 

服务管理的详细信息，请访问[管理您在 Microsoft Azure 上的搜索服务](search-manage.md)。

<!--Image references-->
[1]: ./media/search-monitor-usage/AzureSearch-Monitor1.PNG




 