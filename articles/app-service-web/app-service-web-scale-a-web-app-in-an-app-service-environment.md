---
ms.openlocfilehash: 40a3f795804fcfebb24c70966ee96c277269bbed
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="如何扩展 Web 应用程序的应用程序服务环境中" 
    description="缩放的应用程序服务环境中的 web 应用程序" 
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
    ms.topic="article" 
    ms.date="04/27/2015" 
    ms.author="ccompy"/>

# 缩放的应用程序服务环境中的 web 应用程序 #

在较高级别应用程序服务环境是本质上是个人的部署 Azure 应用程序服务到您的 VNET 和仅可管理您的订阅。 它们提供新的网络功能，因为它们是在您的 VNET，也远超通常在 Azure 应用程序服务的环境中进行缩放。  如果您需要更多信息围绕哪些应用程序服务 Environment(ASE) 是然后看[什么是应用程序服务环境][WhatisASE]。  创建一个应用程序的服务环境，或在应用程序服务环境中创建 web 应用程序的详细信息请参阅[如何创建应用程序服务环境][HowtoCreateASE]和[如何创建 web 应用程序的应用程序服务环境中][CreateWebappinASE]

作为快速提示，当您通常将 web 应用程序中，缩放属性您正在更改的规划应用程序服务级别。  围绕缩放应用程序服务计划的详细信息，或者只是为了在应用程序之外的应用程序服务环境计划的服务的详细信息，请参阅[ScaleWebapp]的[比例在 Azure 应用程序服务 web 应用程序]和[应用程序服务计划，在深度概述][Appserviceplans]。

扩展 web 应用程序的应用程序服务环境中是非常类似于正常缩放 web 应用程序。  在 Azure 应用程序服务有通常可以扩展三个操作︰

- 定价计划
- 工作人员大小 （用于专用实例）
- 实例的数目。

在 ASE 选择或更改定价计划没有必要。  在功能方面它已珍贵定价功能级别。  在应用程序服务环境中还有没有共享的工作人员。  它们是专用的所有工作人员。  而不是固定大小，ASE 管理员可以分配计算资源用于工作人员的每个池的大小。  这意味着具有 P4 计算资源池 1 工作人员和工作人员与 P1 的池 2 计算资源，如果需要。  他们不需要按大小顺序排列。  有关详细说明周围大小和其定价请参阅此处的文档[Azure 应用程序服务定价][AppServicePricing]。  这样，web 应用程序和应用程序服务计划的缩放选项中为应用程序服务环境︰

- 工作人员池选择
- 实例数

通过适当的用户界面显示与您的应用程序服务规划完成任何一项的更改。

![][1]

### 扩展实例的数 ###

当首次创建 web 应用程序的应用程序服务环境中应将它扩展到至少 2 实例，以提供容错功能。   

如果您 ASE 有足够的空间，这是相当简单。  转到包含您想要扩大规模，选择规模的网站您的应用程序服务计划。  这将打开用户界面只需滑动到所需的值的实例标记并保存。  

![][2]

您不能扩展您的应用程序服务计划超出可用的计算资源，在您的应用程序服务的规划是在辅助池数。  如果您需要更多您需要获取 ASE 管理员联系以添加多个计算辅助池，您需要在它们的资源。  有关重新配置您的 ASE 围绕信息阅读此处的信息︰[如何配置应用程序服务环境][HowtoConfigureASE] 
 

### 工作人员池选择 ###

工作人员池选择访问应用程序服务计划用户界面中。  打开您想要扩展并选择辅助池的应用程序服务计划。  您将看到所有已配置应用程序服务环境中的辅助池。  如果您有一个辅助池然后将只能看到列出的一个池。  若要更改您的应用程序服务的规划是在何种工作池，您只需选择您希望您的应用程序服务计划将移到辅助池。  

![][3]

执行此操作之前请务必确保会有足够的容量，为您的应用程序服务的规划。  在辅助池的列表，列出了辅助池名称不仅还可以看到多少工作人员提供了该辅助池。  请确保有足够多的实例可用来包含您的应用程序服务的规划。  如果更需要计算中您要移到辅助池的资源，然后获得 ASE 管理员联系以添加它们。  

从一个工作池移动 web 应用程序将导致重新启动 web 应用程序。  这可能导致您的应用程序，具体取决于重新启动需要多长时间的停机时间的一小部分。  

## 入门教程

要开始使用服务的应用程序环境，请参阅[如何为创建应用程序服务环境][HowtoCreateASE]

关于 Azure 应用程序服务平台的详细信息，请参阅[Azure 应用程序服务][AzureAppService]。

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!--Image references-->
[1]: ./media/app-service-web-scale-a-web-app-in-an-app-service-environment/scaleasp.png
[2]: ./media/app-service-web-scale-a-web-app-in-an-app-service-environment/scaleinstances.png
[3]: ./media/app-service-web-scale-a-web-app-in-an-app-service-environment/scalepool.png

<!--Links-->
[WhatisASE]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[ScaleWebapp]: http://azure.microsoft.com/documentation/articles/web-sites-scale/
[HowtoCreateASE]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-an-app-service-environment/
[HowtoConfigureASE]: http://azure.microsoft.com/documentation/articles/app-service-web-configure-an-app-service-environment/
[CreateWebappinASE]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-a-web-app-in-an-ase/
[Appserviceplans]: http://azure.microsoft.com/documentation/articles/azure-web-sites-web-hosting-plans-in-depth-overview/
[AppServicePricing]: http://azure.microsoft.com/pricing/details/app-service/ 
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/
 
测试
