---
ms.openlocfilehash: beb8f2901713247100092b357f3e3fe809b4c01d
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="如何创建应用程序服务环境" 
    description="应用程序服务环境的创建数据流说明" 
    services="app-service\web" 
    documentationCenter="" 
    authors="ccompy" 
    manager="stefsch" 
    editor=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.date="04/27/2015" 
    ms.author="ccompy"/>

# 如何创建应用程序服务环境 #

应用程序服务环境 (ASE) 是目前正在预览的 Azure 应用程序服务的特优服务选项。  它提供了一种增强的配置能力，它在多租户戳中不可用。  若要更好地理解应用程序服务环境提供的功能请阅读[应用程序服务环境是什么][WhatisASE]文档。

### 概述 ###

ASE 功能实质上是将 Azure 应用程序服务部署到客户的 VNET。  为此客户需要︰ 

- 区域 VNET，则需要使用多个 512 （/ 23） 或多个地址
- 在此 VNET 子网是需要 256 色 （/ 24） 或多个地址
- **不能包含任何其他计算资源**子网。  只有一个应用程序服务环境可以部署到一个子网中。  如果有任何其他已驻留在子网中的计算资源，创建尝试将失败。

如果没有您想要使用来承载您的应用程序服务环境 VNET 您可以创建一个在应用程序服务环境创建过程。

每个 ASE 部署是一种承载服务，Azure 管理和维护。  尽管客户 does 管理实例及其大小的数量，将无法访问客户承载的 ASE 系统角色的计算资源。  

## 创建应用程序服务环境 ##

有两种方法来访问 ASE 创建用户界面。  它可以被找到，通过在 Azure 市场搜索***应用程序服务环境***或通过新建-> Web + 移动。  

### 快速创建 ###
输入创建 UI 后可以快速创建只需输入一个名称为部署 ASE。  反过来，这将创建 VNET 512 地址，子网与 256 地址中的 VNET 和 2 前端和 2 工人工作池 1 中的 ASE 环境。  请务必选择您希望系统所在的位置，并要在该订阅。  可以使用来承载内容 ASE 的唯一帐户必须在用于创建该订阅。

ASE 为指定的名称将用于在 ASE 中创建 web 应用程序。  如果 ASE 的名称是 appsvcenvdemo，则就是域名称。*appsvcenvdemo.p.azurewebsites.net*。  如果因此创建名为 mytestapp，则可在*mytestapp.appsvcenvdemo.p.azurewebsites.net*寻址一个 web 应用程序。  不能在名称中使用空格。  如果在名称中使用大写字符，该域名将总该名称的小写版本。  


![][1]

### 计算资源池 ###

为应用程序服务环境中使用的计算资源管理中计算资源池允许的方式计算资源实例您配置具有池中以及它们的大小。  应用程序服务环境由前端服务器和工作人员组成。  前端服务器处理应用程序的连接负载和工作人员运行的应用程序代码。  前端服务器是专用的计算资源池来管理的。  工作人员又在名为 3 单独计算资源池管理 

- 辅助池 1
- 辅助池 2
- 辅助池 3

如果您有大量的简单的 web 应用程序的请求您很可能会扩大您前端和具有更少的工作人员。  如果您有与光通信的 CPU 或内存密集型 web 应用程序，然后岂不需要很多前结束，但可能需要更多或更大的工作人员。  

不管有多大的计算资源，最小的占地面积都有 2 个前端服务器和 2 的工作人员。  可以配置应用程序服务环境使用达 55 总的计算资源。  这些 55 的计算资源，只有 50 可用于主机的工作负载。 原因是两个折叠。  有至少 2 前端计算资源。  这就给予了达 53，从而支持工作池分配。 尽管提供容错功能，您需要根据以下规则分配更多的计算资源︰

- 每个工作人员池需要至少一个其他计算资源不能分配工作负载
- 然后另一个计算资源池中的计算资源量超过某个特定值时是必需的

在任何一个工作池内容错要求包括分配给工作人员池资源 X 的给定值为︰

- 如果 X 是 2 到 20 之间，可用计算资源可用于工作负载量 X-1
- 如果 X 为 21 到 40 之间，可用计算资源可用于工作负载量 X-2
- 如果 X 是 41 到 53 之间，可用计算资源可用于工作负载量 X-3

除了能够管理计算资源可以分配给指定的池的数量也有大小进行控制。  与应用程序服务环境中，您可以选择从四种不同大小标签为 P4 通过 P1。  周围的那些尺寸和其定价请详细信息请参阅此处[应用程序服务定价][AppServicePricing]。  P3 计算资源规模到 P1 是相同的多租户环境中可用。  P4 和计算 14 gb 的 RAM 资源提供 8 核，才可用的应用程序服务环境中。  

为应用程序服务环境定价是根据分配的计算资源。  为计算资源分配对您的应用程序服务环境不管怎样如果它们承载的工作负载或不支付您。 



### VNET 创建 ###
虽然快速创建会自动创建一个新的 VNET 的功能，功能还支持所选的现有的 VNET 和 VNET 手动创建。  您可以选择现有的 VNET，如果它足够大，以支持应用程序服务环境中部署。  VNET 必须具有 512 地址或更多。  如果您选择现有的 VNET 您还需要指定要使用或创建一个新子网。  子网必须有 256 地址或更多。  

如果经过 VNET 创建用户界面需要提供︰

- VNET 名称
- CIDR 表示法中的 VNET 地址范围
- 子网名称
- 在 CIDR 表示法中的子网范围

如果您不熟悉使用 CIDR 表示法它采用的 10.0.0.0/22 /22 中指定的范围。  在此示例中的 /22 意味着的 1024年地址或 10.0.0.0 范围-10.0.3.255。  /23 意味着 512 地址，等等。  

![][2]

### 应用程序服务环境大小定义 ###

若要配置的下一个项目是系统的规模。  默认情况下有 2 前结束 P2 计算资源、 2 P1 工作人员和 1 个 IP 地址。  有 2 前端以提供高可用性和分配负载。  前端的最小尺寸是 P2，以确保他们有足够的容量来支持适度的系统。  如果您知道系统需要支持大量的请求，您可以调整前端，服务器大小已使用的数量。

如前所述，ASE 内有 3 工作池的客户可以定义。  计算资源大小可以从 P1 为 P4。  默认情况是仅在辅助池 1 中配置的 2 P1 工作人员。  这是不足以支持一个单一的应用程序服务计划，具有 1 个实例。  

滑块会自动进行调整，以反映应用程序服务环境中可用的总的计算能力。  如在任何一个池内调整滑块其它滑块将更改，以反映达到 55 之前的计算资源剩余的可用数量。  
 
![][3]

添加新的实例，可不会发生快速。  如果您知道您将需要更多的计算资源，应该提前配置它们，然后。  资源调配时间可能需要多个小时的时间多少会被添加到系统中。  请记住，若要确保您的系统具有可以满足容错要求，每个 ASE 需要每个工作人员池中有可用的备用实例。  

默认情况下 ASE 附带了 1 个 IP 地址可用于 IP SSL。  如果您知道您将需要更多可以在此处指定的也可以创建后对其进行管理。
  
### 应用程序服务环境创建之后 ###

ASE 创建后，您可以调整︰

- 前端的数量 (最小值︰ 2)
- 工作人员的数量 (最小值︰ 2)
- IP 地址的数量
- 计算资源规模的前端或工作人员使用 （前端的最小大小为 P2）

您不能更改︰

- 位置
- 订阅
- 资源组
- 使用 VNET
- 使用子网

有管理和监视的应用程序服务环境这里周围的更多详细信息︰[如何配置应用程序服务环境][ASEConfig] 

有没有可供自定义数据库和存储等的附加依赖项。  这些是由 Azure，随系统。  系统存储支持整个应用程序服务环境达 500GB。  


## 入门教程

要开始使用服务的应用程序环境，请参阅[应用程序服务环境简介][WhatisASE]

关于 Azure 应用程序服务平台的详细信息，请参阅[Azure 应用程序服务][AzureAppService]。

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]
 

<!--Image references-->
[1]: ./media/app-service-web-how-to-create-an-app-service-environment/createaseblade.png
[2]: ./media/app-service-web-how-to-create-an-app-service-environment/createasenetwork.png
[3]: ./media/app-service-web-how-to-create-an-app-service-environment/createasescale.png

<!--Links-->
[WhatisASE]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[ASEConfig]: http://azure.microsoft.com/documentation/articles/app-service-web-configure-an-app-service-environment/
[AppServicePricing]: http://azure.microsoft.com/pricing/details/app-service/ 
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/ 

测试
