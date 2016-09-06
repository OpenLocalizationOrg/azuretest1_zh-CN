---
ms.openlocfilehash: f4f3ba0ce99045c8413f2825d6a2826b9f52c078
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="在 Azure 中的 SharePoint Server 2013 场 |Microsoft Azure"
    description="查找文章描述如何设置开发/测试环境或 Microsoft Azure 中的 SharePoint Server 2013 生产场。"
    documentationCenter=""
    services="virtual-machines"
    authors="JoeDavies-MSFT"
    manager="timlt"
    editor=""
    tags="azure-service-management,azure-resource-manager"/>

<tags
    ms.service="virtual-machines"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows-sharepoint"
    ms.devlang="na"
    ms.topic="index-page"
    ms.date="07/21/2015"
    ms.author="josephd"/>

# 在 Azure 的基础结构服务承载 SharePoint 服务器场

设置您的第一个或下一个开发/测试或生产 SharePoint 群中 Microsoft Azure 基础结构服务，您可以利用易于配置和快速展开场，包括新建产能的能力或优化的关键功能。

## 基本的 SharePoint 开发/测试场

在服务管理中创建的虚拟机，使用 Azure 预览门户网站的[SharePoint 服务器场](virtual-machines-sharepoint-farm-azure-preview.md)功能创建面向 Internet 的 SharePoint 网站的基本开发/测试场。

自动创建的环境包含三个域控制器的服务器，SQL 服务器，并仅云 Azure 的虚拟网络中的 SharePoint 服务器。

若要创建与资源管理器中创建的虚拟机配置相似，可以使用模板。 请参见[部署三个服务器 SharePoint 服务器场](virtual-machines-workload-template-sharepoint.md#deploy-a-three-server-sharepoint-farm)。

## 高可用性 SharePoint 开发/测试场

在服务管理中创建的虚拟机，使用 Azure 预览门户网站的[SharePoint 服务器场](virtual-machines-sharepoint-farm-azure-preview.md)功能创建高可用性面向 Internet 的 SharePoint 网站的 SharePoint 开发/测试场。

自动创建的环境由九个仅云 Azure 的虚拟网络中的服务器组成︰ 两个域控制器、 三个 SQL 服务器群集、 两个应用程序层 SharePoint 服务器，和两个 web 层 SharePoint 服务器。

若要创建与资源管理器中创建的虚拟机配置相似，可以使用模板。 请参见[部署九个服务器 SharePoint 服务器场](virtual-machines-workload-template-sharepoint.md#deploy-a-nine-server-sharepoint-farm)。

## 混合云开发/测试场

与[SharePoint intranet 场混合云开发/测试环境中的](../virtual-network/virtual-networks-setup-sharepoint-hybrid-cloud-testing.md)，您将创建承载简单、 双层 SharePoint 服务器场，可以用于测试您在 Internet 上的位置从 Azure 中承载 intranet SharePoint 场模拟的混合云配置。

这种配置使用服务管理中创建的虚拟机。

## 高可用性 intranet SharePoint 生产服务器场

与[SharePoint 2013 在 Azure 中的 SQL Server AlwaysOn 可用性组](virtual-machines-workload-intranet-sharepoint-overview.md)的部署，您将构建出一个生产就绪、 高可用性的 intranet SharePoint Server 2013 场 Azure 中。

这种配置使用服务管理中创建的虚拟机。

## 其他资源

对于在 Azure 的信息和配置其他 SharePoint，请参阅以下资源︰

- [SharePoint 2013 的 Microsoft Azure 结构](https://technet.microsoft.com/library/dn635309.aspx)

- [在使用 SharePoint Server 2013 Microsoft Azure 中的 Internet 站点](https://technet.microsoft.com/library/dn635307.aspx)

- [在 Microsoft Azure 中的 SharePoint 服务器 2013年灾难恢复](https://technet.microsoft.com/library/dn635313.aspx)

- [使用 Microsoft Azure Active Directory SharePoint 2013 身份验证](https://technet.microsoft.com/library/dn635311.aspx)

- [部署 Microsoft Azure 中的 Office 365 目录同步目录 （同步）](https://technet.microsoft.com/library/dn635310.aspx)