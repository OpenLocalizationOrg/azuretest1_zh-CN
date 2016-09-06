---
ms.openlocfilehash: 3eb8f418232739e0076142d0a94300a38f173719
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="部署 SharePoint Server 2013 场 |Microsoft Azure"
    description="部署高可用性 SharePoint Server 2013 场分五个阶段在 Azure 使用 SQL Server AlwaysOn 可用性组。"
    documentationCenter=""
    services="virtual-machines"
    authors="JoeDavies-MSFT"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows-sharepoint"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/22/2015"
    ms.author="josephd"/>

# 在 Azure 中的 SQL Server AlwaysOn 可用性组部署 SharePoint

本主题包含指向部署与使用 Azure 服务管理 SQL Server AlwaysOn 可用性组仅用于内部网的 SharePoint 2013 场的分步说明。 服务器场中包含这些计算机︰

- 两个 SharePoint web 服务器
- 两个 SharePoint 应用程序服务器
- 两个数据库服务器
- 一个群集多数节点服务器
- 两个域控制器

这是使用占位符名称为每个服务器配置︰

![](./media/virtual-machines-workload-intranet-sharepoint-overview/workload-spsqlao_05.png)

两台计算机的每个角色确保高可用性。 所有虚拟机都在单个区域。 为某个特定角色的虚拟机的每个组在它自己的可用性设置。

在部署此配置中的以下阶段︰

- [第 1 阶段︰ 配置 Azure](virtual-machines-workload-intranet-sharepoint-phase1.md)。 创建存储帐户、 云服务和跨内部部署的虚拟网络。
- [第二阶段︰ 将域控制器配置](virtual-machines-workload-intranet-sharepoint-phase2.md)。 创建和配置复制 Active Directory 域服务 (AD DS) 域控制器。
- [第 3 阶段︰ 配置 SQL Server 基础结构](virtual-machines-workload-intranet-sharepoint-phase3.md)。 创建并配置 SQL Server 虚拟机、 做好与 SharePoint，使用和创建群集。
- [阶段 4︰ 配置 SharePoint 服务器](virtual-machines-workload-intranet-sharepoint-phase4.md)。 创建和配置四个 SharePoint 虚拟机。
- [阶段 5︰ 创建可用性组并添加 SharePoint 数据库](virtual-machines-workload-intranet-sharepoint-phase5.md)。 准备数据库并创建一个 SQL Server AlwaysOn 可用性组。

此部署的 SharePoint 与 SQL Server AlwaysOn 旨在伴随[SharePoint 与 SQL Server AlwaysOn infographic](http://go.microsoft.com/fwlink/?LinkId=394788) ，结合的最新建议。

此配置是预定义的体系结构的规范性、 阶段的阶段指南在 Azure 的基础结构服务中创建功能、 高可用性的 intranet SharePoint 服务器场。 在 Azure 实现 SharePoint 2013 的其他体系结构指南，请参阅[Microsoft Azure 的 SharePoint 2013 的体系结构](https://technet.microsoft.com/library/dn635309.aspx)。

请牢记以下︰

- 如果您是经验丰富的 SharePoint 实施者，请随意调整阶段 3 至 5 说明并构建最适合您需要的服务器场。
- 如果您已经拥有现有 Azure 混合云实施，随意调整或跳过在阶段 1 和 2 以承载新的 SharePoint 场在相应的子网中的说明。
- 所有服务器均位于 Azure 的虚拟网络中的单个子网。 如果您想要提供额外的安全性等价于子网隔离，您可以使用[网络安全组](virtual-networks-nsg.md)。

若要生成开发/测试环境或证明概念的这种配置，请参阅[设置 SharePoint intranet 服务器场中用于测试的混合云](../virtual-network/virtual-networks-setup-sharepoint-hybrid-cloud-testing.md)。

SharePoint 与 SQL Server AlwaysOn 可用性组有关的其他信息，请参阅[配置 SQL Server 2012 AlwaysOn 可用性组为 SharePoint 2013](https://technet.microsoft.com/library/jj715261.aspx)。

## 下一步

若要启动此工作负载的配置，请转到[第 1 阶段︰ 配置 Azure](virtual-machines-workload-intranet-sharepoint-phase1.md)。


## 其他资源

[SharePoint 与 SQL Server AlwaysOn infographic](http://go.microsoft.com/fwlink/?LinkId=394788)

[SharePoint 2013 的 Microsoft Azure 结构](https://technet.microsoft.com/library/dn635309.aspx)

[在 Azure 的基础结构服务承载 SharePoint 服务器场](virtual-machines-sharepoint-infrastructure-services.md)

[Azure 的基础结构服务实施指南](virtual-machines-infrastructure-services-implementation-guidelines.md)

[Azure 的基础结构服务工作负荷︰ 高可用性业务线应用程序](virtual-machines-workload-high-availability-lob-application.md)
