---
ms.openlocfilehash: b84c0793c81cceb087590f18d535ca5e6a080f42
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="如何配置应用程序服务环境" 
    description="配置、 管理和监视应用程序服务环境" 
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

# 配置应用程序服务环境 #

## 概述 ##

应用程序服务的环境是新特优层功能被提供在预览。  它提供了新的扩展和网络访问能力。  这一新缩放功能可放置到您 VNET Azure 应用程序服务的实例。  如果您熟悉的应用程序服务环境 (ASE) 功能然后阅读此处的文档 [是什么应用程序服务 Environment]/app-service-app-service-environment-intro.md）。 有关如何创建 ASE 读取文档这里[如何创建应用程序的服务环境](app-service-web-how-to-create-an-app-service-environment.md)。 

在较高级别应用程序服务环境包括几个主要组件︰

- 计算运行在 Azure 应用程序环境承载服务资源
- Storage
- 数据库
- 具有至少一个子网的虚拟网络
- 与在其中运行的 Azure 应用程序环境承载服务的子网

以帮助管理和监视您可以为此目的从浏览访问用户界面的应用程序服务环境-> 应用程序服务环境 Azure 预览门户中。 初始版本无需要管理系统，并且将继续提高在未来几周中的附加功能。  

![][1]

## 监视 ##

没有初始的预览版本中的许多指标功能，但它们将推出不久。  这些度量标准功能将帮助系统管理员进行系统缩放和操作上的决策。

即使现在在门户中可以列出所有的应用程序服务计划中 ASE 以及所有 web 应用程序的应用程序服务环境中。  若要查看这两个列表，请转到设置并选择您感兴趣的项目。  

![][3]

在这两个列表中，您可以看到辅助池分配实例数和正在使用的计算资源的大小。  在单独的应用程序服务计划的性能的详细信息将提供与正常情况下是打开的应用程序服务计划 UI 的相同。  

![][4]

## 计算资源 ##

计算资源，存储和数据库所有由 Azure 应用程序服务操作。  不过是由用户来决定的数量和大小的计算资源。  

不管有多大的计算资源，最小的占地面积都有 2 个前端服务器和 2 的工作人员。  可以配置应用程序服务环境使用达 55 总的计算资源。  这些 55 的计算资源，只有 50 可用于主机的工作负载。 原因是两个折叠。  有至少 2 前端计算资源。  这就给予了达 53，从而支持工作池分配。 尽管提供容错功能，您需要根据以下规则分配更多的计算资源︰

- 每个工作人员池需要至少一个其他计算资源不能分配工作负载
- 然后另一个计算资源池中的计算资源量超过某个特定值时是必需的

在任何一个工作池内容错要求包括分配给工作人员池资源 X 的给定值为︰

- 如果 X 是 2 到 20 之间，可用计算资源可用于工作负载量 X-1
- 如果 X 为 21 到 40 之间，可用计算资源可用于工作负载量 X-2
- 如果 X 是 41 到 53 之间，可用计算资源可用于工作负载量 X-3

除了能够管理计算资源可以分配给指定的池的数量也有大小进行控制。  与应用程序服务环境中，您可以选择从四种不同大小标签为 P4 通过 P1。  周围的那些尺寸和其定价的请参见详细信息的[应用程序服务定价](../app-service/app-service-value-prop-what-is.md)为 P3 计算资源规模的 P1 是相同内容通常是可用。  P4 和计算 14 gb 的 RAM 资源提供 8 核，才可用的应用程序服务环境中。

如前所述，应用程序服务环境功能目前在预览中，这种情况下其仍有增长空间。  除了额外的监视功能，更多的管理功能将陆续推出应用程序服务环境移动到上市  现在有几点只可以管理此接口中︰

- 每个池中的计算资源的数量
- 每个池中的计算资源的大小
- 可用的 IP 地址数

若要控制这些操作选择顶部的比例配置项。  

![][2]

这里可以调整每个池和它们的大小的计算资源的数量。  但是进行任何更改之前务必注意几件事︰

- 所做的更改可能需要数小时才能完成根据大小更改请求
- 在已有工作中的应用程序服务环境配置更改，您无法启动另一个更改
- 如果您更改计算使用的资源在辅助池大小可能会导致该人员池中运行的 web 应用程序的停机时间

向工作人员池中添加更多实例是一个良性的操作并不会引起对系统的影响。  但更改计算资源在辅助池的大小是另外一回事。  为了避免任何应用程序停机时间对辅助池的大小更改时，最好︰

- 使用未使用的工作池调出所需的大小所需的实例
- 扩展到新的工作人员池的应用程序服务计划。  
 
这是少得多比将计算资源大小与运行工作负载的应用程序运行中断。  缩放的应用程序服务环境中的 web 应用程序的详细信息此[扩展 Web 应用程序的应用程序服务环境中](app-service-web-scale-a-web-app-in-an-app-service-environment.md)获取  

## 虚拟网络 ##

[虚拟网络][virtualnetwork]和子网的所有受用户的控制。  应用程序服务环境确实有几个网络的要求，但是其余部分将由用户能够控制。  这些 ASE 要求如下︰

- 具有至少 512 地址 VNET
- 具有至少 256 地址子网 
- VNET 必须是区域 VNET  
 
管理您的 VNET 是通过正常的虚拟网络用户界面完成的。

由于此功能将放在它意味着您的应用程序中您 ASE 承载您 VNET Azure 应用程序服务现在可以访问可通过 ExpressRoute 或站点到站点 Vpn 直接资源。  在您的应用程序服务环境中的应用程序不需要其他的联网功能来访问宿主应用程序服务环境 VNET 的可用资源。  

如果需要您现在还可以控制使用网络安全组的访问权限。  此功能允许您锁定您的应用程序服务环境为只是您希望用于限制到的 IP 地址。  围绕如何查看文档的详细信息这里[如何控制应用程序服务环境中的入站流量](app-service-app-service-environment-control-inbound-traffic.md)。

## 删除应用程序服务环境 ##

如果您想要删除一个应用程序的服务环境，然后只是使用的应用程序服务环境刀片式服务器顶部的删除操作。  虽然，有内容，您不能删除 ASE。  一定要删除所有 web 应用程序和应用程序服务计划要删除您的应用程序的服务环境。  

## 入门教程

要开始使用服务的应用程序环境，请参阅[如何为创建应用程序服务环境](app-service-web-how-to-create-an-app-service-environment.md)

关于 Azure 应用程序服务平台的详细信息，请参阅[Azure 应用程序服务](../app-service/app-service-value-prop-what-is.md)。

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!--Image references-->
[1]: ./media/app-service-web-configure-an-app-service-environment/configureaseblade.png
[2]: ./media/app-service-web-configure-an-app-service-environment/configurescale.png
[3]: ./media/app-service-web-configure-an-app-service-environment/configureasplist.png
[4]: ./media/app-service-web-configure-an-app-service-environment/configurewebapplist.png

<!--Links-->
[WhatisASE]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[Appserviceplans]: http://azure.microsoft.com/documentation/articles/azure-web-sites-web-hosting-plans-in-depth-overview/
[HowtoCreateASE]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-an-app-service-environment/
[HowtoScale]: http://azure.microsoft.com/documentation/articles/app-service-web-scale-a-web-app-in-an-app-service-environment/
[ControlInbound]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-control-inbound-traffic/
[virtualnetwork]: https://azure.microsoft.com/documentation/articles/virtual-networks-faq/
[AppServicePricing]: http://azure.microsoft.com/pricing/details/app-service/ 
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/
 

测试
