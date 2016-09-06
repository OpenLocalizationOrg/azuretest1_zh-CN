---
ms.openlocfilehash: 9b913041c7b363be41dc56e32442e10467188144
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="业务线应用程序在 Azure |Microsoft Azure" 
    description="了解业务线应用程序在 Azure 中的值、 设置测试环境，以及部署高可用性配置。" 
    services="virtual-machines" 
    documentationCenter="" 
    authors="JoeDavies-MSFT" 
    manager="timlt" 
    editor=""
    tags="azure-resource-manager"/>

<tags 
    ms.service="virtual-machines" 
    ms.workload="infrastructure-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/11/2015" 
    ms.author="josephd"/>

# Azure 的基础结构服务工作负荷︰ 高可用性业务线应用程序

设置您第一个或下一个基于 web 的、 仅用于内部网的业务线应用程序在 Microsoft Azure 中并利用易于配置和快速展开应用程序以包括新产能的能力。
 
与虚拟机和虚拟网络功能可以快速部署和运行业务应用程序，您的组织的用户都可以访问的行的 Azure 的基础结构服务。 例如，您可以对此进行设置。

![](./media/virtual-machines-workload-high-availability-LOB-application/workload-lobapp-phase4.png)
 
Azure 虚拟网络是所有正确命名和通信路由放置在您的内部网络的扩展，因为您的用户将以相同的方式访问业务线应用程序的服务器，就像它所位于的内部数据中心。

这种配置允许您轻松地扩展应用程序的容量，通过添加新 Azure 的虚拟机的硬件和维护日常开销中比在您的数据中心中运行相当低。

下一步是设置 Azure 中承载的业务应用程序开发/测试行。

## 创建的 Azure 中承载的业务应用程序开发/测试

跨部署虚拟网络连接到一个站点到站点 VPN 或 ExpressRoute 连接内部网络。 如果您想要创建一个模仿的最终配置的开发/测试环境和实验访问应用程序和执行通过 VPN 连接的远程管理，请参阅[设置基于 web 的 LOB 应用程序中用于测试的混合云](../virtual-network/virtual-networks-setup-lobapp-hybrid-cloud-testing.md)。 

![](./media/virtual-machines-workload-high-availability-LOB-application/CreateLOBAppHybridCloud_3.png)
 
您可以使用[MSDN 订阅](http://azure.microsoft.com/pricing/member-offers/msdn-benefits/)或[Azure 试用订阅](http://azure.microsoft.com/pricing/free-trial/)免费创建此开发/测试环境。

下一步是在 Azure 创建高可用性业务线应用程序。

## 部署一套 Azure 中承载的业务应用程序

比较基准，请在 Azure 中的业务应用程序的高可用性线条的代表性配置如下所示。

![](./media/virtual-machines-workload-high-availability-LOB-application/workload-lobapp-phase4.png)
 
这包括︰

- 仅用于内部网的业务线应用程序使用两个服务器在 web 和数据库层。
- 一种 SQL Server AlwaysOn 配置使用两个虚拟机运行在群集中的 SQL Server 和多数节点计算机。
- 两个副本域控制器虚拟网络中的活动目录域服务。

业务线应用程序的概述，请参阅[的业务线应用程序的体系结构蓝图](http://msdn.microsoft.com/dn630664)。

### 物料清单

此基线配置需要以下一组 Azure 服务和组件︰

- 七个虚拟机
- 域控制器和运行 SQL Server 的虚拟机的四个额外的数据磁盘
- 三个可用性设置
- 一个跨部署虚拟网络
- 两个存储帐户

### 部署阶段

若要部署此配置，请使用以下过程︰

- 阶段 1︰ 配置 Azure 

    使用 Azure PowerShell 创建存储帐户、 可用性集和跨内部部署的虚拟网络。 有关的详细的配置步骤，请参阅[第 1 阶段](virtual-machines-workload-high-availability-LOB-application-phase1.md)。

- 阶段 2︰ 配置域控制器 

    配置两个 Active Directory 复制副本域控制器和 DNS 设置为虚拟网络。 有关的详细的配置步骤，请参阅[阶段 2](virtual-machines-workload-high-availability-LOB-application-phase2.md)。

- 阶段 3︰ 配置 SQL Server 基础结构。  

    创建运行 SQL Server 和群集的虚拟机。 有关的详细的配置步骤，请参阅[第 3 阶段](virtual-machines-workload-high-availability-LOB-application-phase3.md)。

- 阶段 4︰ 配置 web 服务器。

    创建 web 服务器的虚拟机并添加您的行的对它的业务应用程序。 有关详细的配置，请参阅[阶段 4](virtual-machines-workload-high-availability-LOB-application-phase4.md)。

- 阶段 5︰ 配置 SQL Server AlwaysOn 可用性组。

    准备应用程序数据库，创建一个 SQL Server AlwaysOn 可用性组，然后向其添加应用程序数据库。 有关的详细的配置步骤，请参阅[第 5 阶段](virtual-machines-workload-high-availability-LOB-application-phase5.md)。

配置完成后，您可以轻松地扩展此业务线应用程序，通过添加多个 web 服务器或虚拟机群集运行 SQL 服务器。

## 其他资源

[部署在 Azure 中的业务应用程序的高可用性线条](virtual-machines-workload-high-availability-LOB-application-overview.md)

[行的业务应用程序的体系结构蓝图](http://msdn.microsoft.com/dn630664)

[设置基于 web 的 LOB 应用程序中用于测试的混合云](../virtual-network/virtual-networks-setup-lobapp-hybrid-cloud-testing.md)

[Azure 的基础结构服务实施指南](virtual-machines-infrastructure-services-implementation-guidelines.md)

[Azure 的基础结构服务工作负荷︰ SharePoint Server 2013 场](virtual-machines-workload-intranet-sharepoint-farm.md)