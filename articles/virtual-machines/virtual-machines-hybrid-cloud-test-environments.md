---
ms.openlocfilehash: 2bb21a4b812a922feabb2f797dab460c16a06ac3
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="在 Azure 中的混合云测试环境 |Microsoft Azure"
    description="找到的文章，介绍了如何构建开发/测试或基于 Azure 的混合云的概念证明专业的 IT 环境。"
    documentationCenter=""
    services="virtual-machines"
    authors="JoeDavies-MSFT"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/09/2015"
    ms.author="josephd"/>

# Azure 混合云测试环境

开发/测试或概念证明，混合云测试环境使用本地互联网连接和一个公用 IP 地址和分步指导您完成设置工作，跨场所 Azure 虚拟网络 (VNet)。 完成后，可以在开发和测试应用程序、 简化 IT 工作负荷，试验和测量站点到站点虚拟专用网络 (VPN) 连接的性能相对于您在 Internet 上的位置。

> [AZURE.NOTE] 这些文章目前在服务管理中创建虚拟机、 虚拟网络和其他资源

## 混合云基本配置

[混合云的基本配置](../virtual-network/virtual-networks-setup-hybrid-cloud-environment-testing.md)包括︰

- 简化现有内部网络与四个虚拟机 （，域控制器、 应用程序服务器、 客户端计算机，并运行 Windows 服务器和路由和远程访问 VPN 设备）
- 副本域控制器使用 Azure 虚拟网络
- 站点到站点 VPN 连接

## 混合云在 SharePoint intranet 服务器场

[混合云在 SharePoint intranet 场测试环境](../virtual-network/virtual-networks-setup-sharepoint-hybrid-cloud-testing.md)将 SQL Server 2014年服务器和 SharePoint Server 2013 服务器添加到混合云的基本配置。 这将创建可以从简化的本地网络上的客户端计算机访问的两级 SharePoint 场。

## 基于 web 的业务线 (LOB) 应用程序中混合云

[混合云测试环境中基于 web 的 LOB 应用程序](../virtual-network/virtual-networks-setup-lobapp-hybrid-cloud-testing.md)添加到混合云基本配置 SQL Server 2014年服务器和 Internet Information Services (IIS) 服务器。 这将创建在其中您可以部署和测试分层的、 基于 web 的 LOB 应用程序的基础结构。

## 混合云在 office 365 （目录同步） 的目录同步服务器

[Office 365 目录同步服务器中混合云测试环境](../virtual-network/virtual-networks-setup-dirsync-hybrid-cloud-testing.md)添加混合云基本配置目录同步服务器和演示 Office 365 目录同步到试用 Office 365 的密码同步。

## 模拟的混合云测试环境

有关组织和个人为其直接的 Internet 连接和公用 IP 地址不是易于使用，[模拟的混合云测试环境](../virtual-network/virtual-networks-setup-simulated-hybrid-cloud-environment-testing.md)生成单独的 Azure 虚拟网络中简化的本地网络，然后将两个虚拟网络连接 VNet 到 VNet VPN 连接时。


## 其他资源

[在 Azure 的基础结构服务承载 SharePoint 服务器场](virtual-machines-sharepoint-infrastructure-services.md)

[Pdf 格式的三维的业务线应用程序的体系结构蓝图](http://download.microsoft.com/download/2/C/8/2C8EB75F-AC45-4A79-8A63-C1800C098792/MS_Arch_LOB_App_3D_pdf.pdf)

[部署 Microsoft Azure 中的 Office 365 目录同步目录 （同步）](https://technet.microsoft.com/library/dn635310.aspx)

[Azure 的基础结构服务实施指南](virtual-machines-infrastructure-services-implementation-guidelines.md)
