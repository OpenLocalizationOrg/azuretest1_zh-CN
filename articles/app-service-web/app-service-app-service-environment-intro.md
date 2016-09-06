---
ms.openlocfilehash: f6095f9e5bc386689f18174b8109222f90cfcf13
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="应用程序服务环境简介" 
    description="了解提供了安全、 加入 VNet 的、 专用的比例单位运行您的应用程序的所有应用程序服务环境功能。" 
    services="app-service\web" 
    documentationCenter="" 
    authors="ccompy" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/31/2015" 
    ms.author="stefsh"/>    

# 应用程序服务环境简介

## 概述 ##
应用程序服务环境是 Azure 应用程序服务，提供一个完全独立和专门的环境，以便安全地运行您的应用程序的所有[高级][PremiumTier]服务计划选项。  这包括[WebApps]的[Web 应用程序]、[移动应用程序][MobileApps]、 [API 应用程序][APIApps]和[逻辑应用程序][LogicApps]与扩展的扩展选项。  

计算资源的应用程序服务环境专门专门用来运行仅您的应用程序。  始终在区域的虚拟网络，这将使您的应用程序新的网络隔离选项创建一个应用程序的服务环境。  此外，应用程序服务环境支持其他缩放比例选项，达五十 （50） 计算的可用资源来运行您的应用程序。  应用程序服务环境之外还有 20 来承载您的应用程序的计算资源的限制。 

## 虚拟网络支持 ##
既可以预先存在的区域虚拟网络或新的区域虚拟网络 ([在虚拟网络上的多个信息][MoreInfoOnVirtualNetworks]) 中创建应用程序服务环境。  由于在区域虚拟网络中，始终存在一个应用程序的服务环境，可以更精确地内区域的虚拟网络的子网，利用虚拟的网络来控制这两个入站和出站网络通信的安全功能。  

可以使用[网络安全组][NetworkSecurityGroups]限制到子网的入站的网络通信应用程序服务环境所在。  这允许您运行背后上游设备和服务，如 web 应用程序防火墙和网络 SaaS 提供商的应用程序。  

应用程序还经常需要访问公司资源，如内部数据库和 web 服务。  一种常见的方法是使这些终结点仅供内部流动在 Azure 的虚拟网络中的网络通信。  一旦应用程序服务环境已连接到同一个虚拟网络的内部服务，在该环境中运行的应用程序可以访问它们，包括无法通过连接[到网站][SiteToSite]和[Azure ExpressRoute][ExpressRoute]连接连接到其他终结点。

## 专用的计算资源 ##
所有的应用程序服务环境中的计算资源被专门用于单个订阅。  应用程序服务环境组成单一前端的计算资源池，以及一至三个工作人员计算资源池。 

前端的池包含负责为很好地自动负载平衡的应用程序服务环境中的应用程序请求 SSL 端接的计算资源。 

每个辅助池包含分配给[应用程序服务计划][AppServicePlan]，其中又包含一个或多个 Azure 应用程序服务应用程序的计算资源。  因为应用程序服务环境中可以有最多三个不同的辅助池，您必须能够灵活地选择不同的计算资源供每个辅助池。  

例如这允许您创建一个辅助池与较弱的计算资源供开发或测试的应用程序的应用程序服务计划。  第二个 （或甚至第三） 辅助池可以使用功能更强大的计算资源供生产应用程序运行的应用程序服务计划。

在单个辅助池达五十 （50） 计算资源可以配置应用程序服务环境。  对前端和辅助池可用的计算资源的数量的详细信息，请参阅[如何配置应用程序服务环境][HowToConfigureanAppServiceEnvironment]。  

有关支持的应用程序服务环境中的可用计算资源大小的详细信息，请查阅[应用程序服务定价][AppServicePricing]页面并查看的应用程序服务环境最优定价层中可用的选项。


## 入门教程

要开始使用服务的应用程序环境，请参阅[如何为创建应用程序服务环境][HowToCreateAnAppServiceEnvironment]

关于 Azure 应用程序服务平台的详细信息，请参阅[Azure 应用程序服务][AzureAppService]。

应用程序服务环境网络体系结构的概述，请参阅[网络体系结构概述][NetworkArchitectureOverview]文章。

在 ExpressRoute 中使用的应用程序服务环境的详细信息，请参阅以下文章上[NetworkConfigDetailsForExpressRoute]的[快速通道和服务应用程序的环境]。

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- LINKS -->
[PremiumTier]: http://azure.microsoft.com/pricing/details/app-service/
[MoreInfoOnVirtualNetworks]: https://azure.microsoft.com/documentation/articles/virtual-networks-faq/
[AppServicePlan]: http://azure.microsoft.com/documentation/articles/azure-web-sites-web-hosting-plans-in-depth-overview/
[Azure 预览门户]: http://portal.azure.com
[HowToCreateAnAppServiceEnvironment]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-an-app-service-environment/
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/
[WebApps]: http://azure.microsoft.com/documentation/articles/app-service-web-overview/
[MobileApps]: http://azure.microsoft.com/documentation/articles/app-service-mobile-value-prop-preview/
[APIApps]: http://azure.microsoft.com/documentation/articles/app-service-api-apps-why-best-platform/
[LogicApps]: http://azure.microsoft.com/documentation/articles/app-service-logic-what-are-logic-apps/
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[SiteToSite]: https://azure.microsoft.com/documentation/articles/vpn-gateway-site-to-site-create/
[ExpressRoute]: http://azure.microsoft.com/services/expressroute/
[HowToConfigureanAppServiceEnvironment]:  http://azure.microsoft.com/documentation/articles/app-service-web-configure-an-app-service-environment/
[NetworkArchitectureOverview]:  https://azure.microsoft.com/documentation/articles/app-service-app-service-environment-network-architecture-overview/
[NetworkConfigDetailsForExpressRoute]:  https://azure.microsoft.com/documentation/articles/app-service-app-service-environment-network-configuration-expressroute/
[AppServicePricing]: http://azure.microsoft.com/pricing/details/app-service/ 

<!-- IMAGES -->

 

测试
