---
ms.openlocfilehash: b19d984e92734f19c1dfb707dc7ec3faa57c3abb
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="使用 Azure 门户站点到站点 VPN 连接时创建虚拟网络 |Microsoft Azure"
   description="对于跨部署和混合站点到站点 VPN 连接时创建虚拟网络配置。"
   services="vpn-gateway"
   documentationCenter=""
   authors="cherylmc"
   manager="carolz"
   editor=""/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/11/2015"
   ms.author="cherylmc"/>

# 使用 Azure 门户站点到站点 VPN 连接时创建虚拟网络

> [AZURE.SELECTOR]
- [Azure 门户](vpn-gateway-site-to-site-create.md)
- [PowerShell 的 Azure 的资源管理器](vpn-gateway-create-site-to-site-rm-powershell.md)

这篇文章将引导您完成创建传统的虚拟网络和站点到站点 VPN 连接到内部网络。

Azure 目前有两种部署模型︰ 经典的部署模型和 Azure 资源管理器的部署模型。 配置步骤有所不同，具体取决于用于部署虚拟网络的模型。

这些说明适用于传统的部署模型。 如果您想要创建使用 Azure 资源管理器模型的站点到站点 VPN 网关连接，请参阅[创建使用 Azure 资源管理器和 PowerShell 的站点到站点 VPN 连接](vpn-gateway-create-site-to-site-rm-powershell.md)。


## 在开始之前

- 验证您要使用的 VPN 设备满足创建跨部署虚拟网络连接所必需的要求。 有关更多信息，请参见[关于 VPN 虚拟网络连接的设备](vpn-gateway-about-vpn-devices.md)。

- 获得对外 IPv4 IP VPN 设备。 此 IP 地址对于站点的配置是必需的用于您的 VPN 设备，不能位于后面 nat。

>[AZURE.IMPORTANT] 如果您不熟悉配置 VPN 设备或熟悉位于内部网络配置的 IP 地址范围，必须配合可以为您提供这些详细信息的人。

## 创建虚拟网络

1. 登录到**Azure 的门户**。

2. 在屏幕的左下角，单击**新建**。 在导航窗格中，单击**网络服务**，然后单击**虚拟网络**。 单击**创建自定义**启动配置向导。

3. 填写在以下页面中创建您的 VNet 的信息。

## 虚拟网络详细信息页

输入以下信息。

- **名称**︰ 命名您的虚拟网络。 例如， *EastUSVNet*。 部署您的虚拟机和 PaaS 的实例，因此您可能不想要太复杂的名称时，您将使用此虚拟网络名称。
- **位置**︰ 位置直接关系到希望资源 (Vm) 所在的物理位置 （区域）。 例如，如果您希望将 Vm 部署到物理上位于*东亚美国*此虚拟网络，选择该位置。 不能更改所创建与虚拟网络的地区。

## DNS 服务器和 VPN 连接页
输入以下信息，然后单击右下角上的下箭头。

- **DNS 服务器**︰ 输入 DNS 服务器名称和 IP 地址，或从快捷菜单中选择先前已注册的 DNS 服务器。 此设置不会创建一个 DNS 服务器，它允许您指定要使用的虚拟网络名称解析的 DNS 服务器。
- **配置站点到站点 VPN**︰**站点到站点 VPN 配置**选中的复选框。
- **本地网络**︰ 本地网络表示内部部署物理位置。 您可以选择以前创建的本地网络或您可以创建新的本地网络。 但是，如果您选择使用您以前创建的本地网络，您需要转到**本地网络**配置页，并确保用于 VPN 设备正在使用该连接的 VPN 设备 IP 地址 （公用对开 IPv4 地址） 是准确。

## 站点到站点连接页
如果您正在创建新的本地网络，则您将看到**站点对站点连接**页。 如果您想要使用以前创建的本地网络，在向导中将不显示此页面，您可以将移动到下一节。

输入以下信息，然后单击下一步的箭头。

-   **名称**︰ 想要调用本地 （内部部署） 的名称的网络站点。
-   **VPN 设备 IP 地址**︰ 这是公共面临内部部署 VPN 设备将用来连接到 Azure 的 IPv4 地址。 VPN 设备不能位于后面 nat。
-   **地址空间**︰ 包括启动 IP 和 CIDR （地址计数）。 这是您在其中指定 range(s)，所需的地址发送到本地内部部署位置的虚拟网络网关通过。 如果目标 IP 地址落在此处指定的范围内，它将通过虚拟网络网关路由。
-   **添加地址空间**︰ 如果必须要通过虚拟网络网关发送的多个地址范围，这是您在其中指定每个额外的地址范围。 您可以添加或删除**本地网络**页面上以后的范围。

## 虚拟网络地址空间页
指定您想要使用的虚拟网络地址范围。 这些都是动态的 IP 地址 (DIP) 将分配给虚拟机，该虚拟网络部署其他角色实例。

它是特别重要，要选择具有任何内部网络使用的范围不会重叠的范围。 您需要与网络管理员联系，协调一致。 您的网络管理员可能需要划分从内部网络地址空间供您使用的虚拟网络范围内的 IP 地址。

输入以下信息，然后单击右下方以配置您的网络上的选中标记。

- **地址空间**︰ 包括启动 IP 和地址数。 请验证您指定的地址空间不覆盖任何对您的内部网络的地址空间。
- **添加子网**︰ 包括起始 IP 和地址数。 其他的子网不是必需的但要为将具有静态 DIP 的虚拟机创建一个单独的子网。 或者，您可能想要您的 Vm 中其他角色实例与不同的子网。
- **添加网关网**︰ 单击以添加网关网。 网关网仅用于虚拟网络网关和对于这种配置是必需的。

单击页面底部的选中标记和虚拟网络将开始创建。 完成后，您将看到**创建**Azure 门户中的**网络**页上列出**状态**下。 在创建 VNet 之后，您可以配置虚拟网络网关。

## 配置虚拟网络网关

接下来，您将创建安全的站点对站点连接配置虚拟网络网关。 请参阅[配置虚拟网络网关在 Azure 的门户](vpn-gateway-configure-vpn-gateway-mp.md)。

## 下一步行动

您可以了解有关虚拟网络跨本地连接在这篇文章︰[关于安全的跨内部连接的虚拟网络](vpn-gateway-cross-premises-options.md)。

如果您想要配置点对站点的 VPN 连接，请参阅[配置点到站点 VPN 连接](vpn-gateway-point-to-site-create.md)。

可以将虚拟机添加到虚拟网络。 了解[如何创建一个自定义的虚拟机](../virtual-machines/virtual-machines-create-custom.md)。

如果您想要配置使用 RRAS 的 VNet 连接，请参阅[配置站点到站点 VPN 使用 Windows Server 2012 路由和远程访问服务 (RRAS)](https://msdn.microsoft.com/library/dn636917.aspx)。

如果您想要配置传统虚拟网络和虚拟网络使用 Azure 资源管理器模式创建之间的连接，请参阅[连接到 Azure 资源管理器 VNets 的经典 VNets](../virtual-network/virtual-networks-arm-asm-s2s-howto.md)。
