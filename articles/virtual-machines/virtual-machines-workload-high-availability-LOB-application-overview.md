---
ms.openlocfilehash: 8c74b7af2813d590664b7ab9bd814c58d9139db8
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="部署业务线应用程序 |Microsoft Azure" 
    description="部署基于 web 的、 高度可用业务线应用程序与 SQL Server AlwaysOn 可用性组在 Azure 中分五个阶段。" 
    documentationCenter=""
    services="virtual-machines" 
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

# 部署在 Azure 中的业务应用程序的高可用性线条

这篇文章包含指向部署高可用性、 仅用于内部网的、 基于 web 的业务线应用程序与 SQL Server AlwaysOn 可用性组在 Azure 的基础结构服务的分步指导。 在这些计算机上承载应用程序︰

- 两个 web 服务器
- 两个数据库服务器
- 一个群集多数节点服务器
- 两个域控制器

这是具有占位符名称为每个服务器的配置。

![](./media/virtual-machines-workload-high-availability-LOB-application-overview/workload-lobapp-phase4.png) 
 
对于每个角色至少两个机器确保高可用性。 所有虚拟机都在一个 Azure 位置 （也称为区域）。 为某个特定角色的虚拟机的每个组在它们自己的可用性设置。 

在部署此配置中的以下阶段︰

- [第 1 阶段︰ 配置 Azure](virtual-machines-workload-high-availability-LOB-application-phase1.md)。 创建存储帐户、 可用性集和跨内部部署的虚拟网络。
- [第二阶段︰ 将域控制器配置](virtual-machines-workload-high-availability-LOB-application-phase2.md)。 创建和配置复制 Active Directory 域服务 (AD DS) 域控制器。
- [第 3 阶段︰ 配置 SQL Server 基础结构](virtual-machines-workload-high-availability-LOB-application-phase3.md)。 创建和配置运行 SQL Server 的虚拟机创建群集，启用 SQL Server AlwaysOn 可用性组。
- [阶段 4︰ 配置 web 服务器](virtual-machines-workload-high-availability-LOB-application-phase4.md)。 创建和配置两个 web 服务器的虚拟机。
- [阶段 5︰ 将应用程序的数据库添加到 SQL Server AlwaysOn 可用性组](virtual-machines-workload-high-availability-LOB-application-phase5.md)。 准备业务应用程序数据库的行并将它们添加到 SQL Server AlwaysOn 可用性组。

此部署旨在附带[的业务线应用程序的体系结构蓝图](http://msdn.microsoft.com/dn630664)和结合的最新建议。

这是一个规范性、 预定义的体系结构。 请牢记以下︰

- 如果您的业务应用程序实施者有经验基于 web 的行，请随意调整至第 5 阶段 3 中的说明和构建最适合您需要的应用程序基础结构。 
- 如果您已经拥有现有 Azure 混合云实施，请随意调整或跳过主机上相应的子网的新应用程序的虚拟机的阶段 1 和 2 中的说明进行操作。
- 所有服务器均位于 Azure 的虚拟网络中的单个子网。 如果您想要提供额外的安全性等价于子网隔离，您可以使用[网络安全组](../virtual-networks/virtual-networks-nsg.md)。

若要生成开发/测试环境或证明概念的这种配置，请参阅[设置基于 web 的 LOB 应用程序中用于测试的混合云](../virtual-network/virtual-networks-setup-lobapp-hybrid-cloud-testing.md)。

设计的 Azure 的 IT 工作负荷有关的其他信息，请参见[Azure 的基础结构服务实施准则](virtual-machines-infrastructure-services-implementation-guidelines.md)。

## 下一步

若要启动此工作负载的配置，请转到[第 1 阶段︰ 配置 Azure](virtual-machines-workload-high-availability-LOB-application-phase1.md)。

## 其他资源

[行的业务应用程序的体系结构蓝图](http://msdn.microsoft.com/dn630664)

[设置基于 web 的 LOB 应用程序中用于测试的混合云](../virtual-network/virtual-networks-setup-lobapp-hybrid-cloud-testing.md)

[Azure 的基础结构服务实施指南](virtual-machines-infrastructure-services-implementation-guidelines.md)

[Azure 的基础结构服务工作负荷︰ SharePoint Server 2013 场](virtual-machines-workload-intranet-sharepoint-farm.md)