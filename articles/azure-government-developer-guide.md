---
ms.openlocfilehash: f424f09d6abc1e66c6b24ab57d423f2e98188dec
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="Azure 政府开发人员指南" 
    description="这为 Azure 政府开发应用程序提供功能和指导的比较" 
    services="" 
    documentationCenter="" 
    authors="Joharve2" 
    manager="carolz" 
    editor=""/>

<tags 
    ms.service="multiple" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="azure-government" 
    ms.date="01/21/2014" 
    ms.author="jharve"/>


#  Microsoft Azure 政府开发指南 

<p> Microsoft 的 Azure 政府实际网络的 Microsoft Azure 的独立实例。  此开发人员指南将提供详细信息的差异，应用程序开发人员和管理员需要进行交互并使用 Azure 的这些独立的区域。

<!--Table of contents for topic, the words in brackets must match the heading wording exactly-->


## 本主题中


+ [概述](#Overview)
+ [开发人员指南](#Guidance)
+ [在 Microsoft Azure 政府当前可用的功能](#Features)
+ [终结点映射](#Endpoint)
+ [下一步行动](#next)


## <a name="Overview"></a>概述

Microsoft Azure 政府为解决美国联邦机构、 地方政府、 国家及与其解决方案提供商的安全和法规遵从性需要 Microsoft Azure 服务的单独实例。 Azure 政府提供物理和网络从非美国政府部署的隔离和屏蔽的美国人员。 

Microsoft 提供了多种工具来创建和部署到 Microsoft 的全球 Microsoft Azure 服务 （"全球服务"） 和 Microsoft Azure 政府服务的云应用程序。

当创建和部署应用程序到 Azure 的政府服务，而不是全局服务，开发人员需要知道这两项服务的主要区别。  特别是在建立和配置它们的编程环境，配置终结点，编写应用程序，并将它们部署到 Azure 政府服务。

此文档中的信息概述了这些不同之处，并补充了[Azure 政府](http://www.azure.com/gov "Azure 政府")网站和[Microsoft Azure 技术库]在 MSDN 上的(http://msdn.microsoft.com/cloud-app-development-msdn "MSDN")上提供的信息。 在许多场合如[微软 Azure 信任中心](http://azure.microsoft.com/support/trust-center/ "微软 Azure 信任中心")， [Azure 文档中心](http://azure.microsoft.com/documentation/)和[Azure 博客](http://azure.microsoft.com/blog/ "Azure 博客")官方信息可能还会提供。 

此内容适用于合作伙伴和开发人员将部署到 Microsoft Azure 政府。



## <a name="Guidance"></a>开发人员指南
由于大部分目前可用的技术内容假定为全球服务而不是 Microsoft Azure 政府，正在开发应用程序，务必确保开发人员知道的开发可以在 Azure 政府中承载的应用程序的主要区别。

- 首先，没有服务和功能上的差异，这意味着全球服务的特定区域中的某些功能可能不可用 Azure 的政府机构。

- 第二，在 Azure 政府提供的功能，还有从全球服务配置差异。  因此，您应该检查您的示例代码、 配置和步骤，以确保您构建和 Azure 云服务政府环境中执行。


## <a name="Features"></a> 在 Microsoft Azure 政府当前可用的功能
Azure 政府当前在美国 GOV 爱荷华州及美国 GOV 弗吉尼亚地区具有可用的以下服务︰

- 虚拟机
- 云服务
- Storage
- 活动目录
- 计划程序
- 虚拟网络
- SQL 数据库

我们提供其他服务，并将持续不断地添加更多的服务。  有关服务的最新列表，请参阅[区域页](http://azure.microsoft.com/regions/#services)将突出显示每个可用的地区，他们的服务。  

目前，美国 GOV 爱荷华州和美国 GOV 弗吉尼亚州是支持政府 Azure 数据中心。  请参阅为当前数据中心和可用的服务之上的区域页面。

## <a name="Endpoint"></a>终结点映射

使用下表可指导您将公用的 Microsoft Azure 和 SQL 数据库终结点映射到 Azure 政府特定终结点时。


服务类型|Azure 公共|Azure 的政府
---|---|---
Azure 政府主页|windowsazure.com|microsoftazure.us
管理门户|manage.windowsazure.com|manage.windowsazure.us
常规|*。 windows.net|*。 usgovcloudapi.net
核心|*。 core.windows.net|*。 core.usgovcloudapi.net
计算|*。 cloudapp.net|*。 usgovcloudapp.net
Blob 存储|*。 blob.core.windows.net|   *。 blob.core.usgovcloudapi.net
队列存储|*。 queue.core.windows.net|*。 queue.core.usgovcloudapi.net
表存储|*。 table.core.windows.net|*。 table.core.usgovcloudapi.net
服务管理|management.core.windows.net|management.core.usgovcloudapi.net
SQL 数据库|*。 database.windows.net|*。 database.usgovcloudapi.net

## <a name="next"></a>下一步行动
如果您想学习更多知识和 Azure 政府以及您的组织可以限定要访问，请转至<A href="http://azure.com/gov">http://www.azure.com/gov</a>

<!--Anchors-->



<!-- Images. -->

[1]: ./media/azure-government-developer-guide/publisherguide.png


<!--Link references-->
[链接 1 到另一个 azure.microsoft.com 文档主题]: virtual-machines-windows-tutorial.md
[另一 azure.microsoft.com 文档主题链接 2]: web-sites-custom-domain-name.md
[链接到另一个 azure.microsoft.com 文档主题的 3]: storage-whatis-account.md

testsssssss sss
