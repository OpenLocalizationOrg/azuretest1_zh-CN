---
ms.openlocfilehash: f06b1224534c714bc6a465daabcb993c6cca6d1d
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="虚拟网络使用配置表 |Microsoft Azure"
    description="了解如何配置跨场所 Azure 中的设置配置表具有预先确定设置虚拟网络。"
    documentationCenter=""
    services="virtual-machines"
    authors="JoeDavies-MSFT"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/21/2015"
    ms.author="josephd"/>

# 通过使用配置表创建跨部署虚拟网络

此主题的步骤您通过创建跨部署虚拟网络，通过使用设置以前指定下面的一组配置表中︰

- 表 v 部分︰ 跨部署虚拟网络配置
- S︰ 表中虚拟网络的子网
- 表格 d︰ 内部 DNS 服务器
- L︰ 表的本地网络地址的前缀

通常主题介绍了 IT 工作负载在 Azure 中的配置，包括跨部署虚拟网络中填写这些表格。 请参阅[第 1 阶段︰ 配置 Azure](virtual-machines-workload-intranet-sharepoint-phase1.md)的示例。

下面的过程引用了这些表可指导您完成虚拟网络配置过程中的信息。 如果您还没有指定设置另一个主题，在这些表中，但您仍希望跨部署虚拟网络配置，请参阅[配置跨部署站点对站点连接到 Azure 的虚拟网络](../vpn-gateway/vpn-gateway-site-to-site-create.md)。

> [AZURE.NOTE] 此过程将指导您完成创建虚拟网络使用站点到站点 VPN 连接。 有关使用 Azure ExpressRoute 站点到站点连接的信息，请参阅[ExpressRoute 技术概述](../expressroute/expressroute-introduction.md)。

## 创建新的跨场所 Azure 使用您配置的表设置虚拟网络

1. 登录到[Azure 的门户](https://manage.windowsazure.com/)。
2. 在任务栏中，单击**新建 > 网络服务 > 虚拟网络 > 创建自定义**。
3. 在**虚拟的网络详细信息**页上︰
    - 在**名称**中，键入表 V 中第 1 项中的名称。
    - 在**位置**从物料表 V 2 中选择的地区。
4. 单击下一步以继续。
5. 在**DNS 服务器和 VPN 连接**页中︰
    - 在**DNS 服务器**表格 D 中的每个条目的配置好记的名称和您的内部 DNS 服务器的 IP 地址。
    - 在**站点到站点连接**，选择**配置站点到站点 VPN**。
    - 如果您已配置了本地网络并想使用它，请在**本地网络**中选择其名称。 如果您想要创建新的本地网络，在**本地网络**中选择**指定一个新的本地网络**。
    - 如果尚未为您的订阅配置本地网络，您将看不到**本地网络**字段。
6. 单击下一步以继续。
7. 在**站点对站点连接**页面 （如果您在步骤 5 中选择**指定一个新的本地网络**）︰
    - 在**名称**中，键入从物料表 V （本地网络名称） 中的 3 名。
    - 在**VPN 设备 IP 地址**中，键入表 V 中的项目 4 中的地址。
    - 在**地址空间**中，表 L 中的每项输入 （在**起始 IP**) 前缀和前缀长度 （以**CIDR （地址计数）**) 方面您组织的网络的 IP 地址空间。
8. 单击下一步以继续。
9. 在**虚拟的网络地址空间**页上︰
    - 在**地址空间**中，输入虚拟网络的专用 IP 地址空间从表 V 的项目 5 （在**起始 IP**) 前缀和前缀长度 （以**CIDR （地址计数）**)。
    - 在**子网**，每个条目中表 s:
        - 子网名称列中键入**子网**，覆盖默认的名称，如果需要。
        - 输入的子网，（在**起始 IP**) 前缀和前缀长度 （以**CIDR （地址计数）**) 专用的 IP 地址空间。
    - 单击**添加子网网关**。
10. 单击复选标记以完成配置。

## 其他资源

[虚拟网络概述](../virtual-network/virtual-networks-overview.md)

[虚拟网络配置任务](../documentation/services/virtual-machines/)

[配置跨部署站点对站点连接到 Azure 的虚拟网络](../vpn-gateway/vpn-gateway-site-to-site-create.md)
