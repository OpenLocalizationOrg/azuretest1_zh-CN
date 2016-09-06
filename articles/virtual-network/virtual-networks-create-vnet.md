---
ms.openlocfilehash: c91505c228be7d78d38de699e592a104adf120e6
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="如何创建虚拟网络 (VNet)"
   description="了解如何创建虚拟网络 (VNet)"
   services="virtual-network"
   documentationCenter="na"
   authors="telmosampaio"
   manager="carolz"
   editor="tysonn" />
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="06/08/2015"
   ms.author="telmos" />

# 如何创建虚拟网络 (VNet)

VNet 创建时，您的服务和 VNet 中的虚拟机可以安全地相互通信而无需通过互联网走出去。 创建 Azure VNet 是一个相对快速和简单的过程，如果 VNet 不打算到其他 VNets 连接或连接到您的内部网络，因为您将不需要获取并配置 VPN 设备，或协调与其他 VNets 或本地网络您选择的 IP 地址。

>[AZURE.WARNING] 不要使用此过程可以创建以后可以 VNet 连接其他 VNets 或您的内部网络。 如果您想要创建安全的跨场所或混合连接，请参阅[有关虚拟网络安全跨内部连接](vpn-gateway-cross-premises-options.md)。 如果您想要创建连接到另一个 VNet VNet，请参阅[配置 VNet 连接到 VNet](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md)。

## 配置您的 VNet

1. 登录到**Azure 的管理门户**。

1. 在屏幕的左下角，单击**新建**。 在导航窗格中，单击**网络服务**，然后单击**虚拟网络**。 单击**创建自定义**启动配置向导。

1. 在**虚拟的网络详细信息**页上，输入以下信息︰

    ![虚拟网络详细信息](./media/virtual-networks-create-vnet/IC736054.png)

    - **名称-**命名您的 VNet。 例如，我们创建了*EastUSVNet*的名称。 您可以创建任何想要的名称。 在部署您的虚拟机和服务，因此最好不能太复杂的名称时，您将使用此 VNet 名。

    - **位置 –**从下拉列表中选择的位置 （区域）。 位置与您的资源 (Vm) 时将它们部署到此 VNet 所在的物理位置直接相关。 例如，如果您希望您的 Vm 可以物理上位于*美国东部*，选择该位置区域。 不能更改所创建与您的 VNet 相关联的区域。

1. 在**DNS 服务器和 VPN 连接**页上未做任何更改。 只是向前移动到下一页面通过单击箭头。 默认情况下，Azure 为您 VNet 提供基本名称解析。 很可能您的名称解析需求非常复杂，不是由基本的 Azure 名称解析。 在这种情况下，可能以后要添加到您的 VNet 运行 DNS 的虚拟机。 有关 Azure 的名称解析和 DNS 的详细信息，请参阅[域名解析 (DNS)](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md)。

1. **虚拟网络地址空间**页面是您在其中输入要用于此 VNet 的地址空间。 除非您为您的 Vm 需要将某些内部 IP 地址范围或您想要创建的虚拟机将接收静态 DIP 特定的子网，您不必在此页进行任何更改。 如果要创建多个子网，则可以这样在此页上单击**添加子网**。

    附加信息︰ 

    - 在此页上的区域包含在将它们部署到此 VNet 时，将收到您的虚拟机的动态内部 IP 地址 (Dip)。 这些 IP 地址被用于在 VNet 仅内部通讯。 它们不是互联网的终结点的 IP 地址。

    - 因为您不打算使用跨部署 VPN 配置此专用的 VNet 连接到内部网络，不需要调整这些设置与您现有的内部网络 IP 地址范围。 如果您认为要创建跨部署配置以后，您将需要协调现在与已经存在于本地站点来避免路由问题的范围的地址空间。 更改范围以后会很有点复杂，尤其是如果您有重叠的地址范围。

    ![地址空间](./media/virtual-networks-create-vnet/IC716778.png)

1. 单击右下角的**虚拟网络地址空间**页面上的复选标记，将创建您的 VNet。 创建您的 VNet 之后，您将看到**创建**管理门户中的**网络**页上列出**状态**下。

1. 一旦已经创建了您的 VNet，您可以创建在 VNet 中的虚拟机和 PaaS 实例。 请务必选择**剪辑库中**时创建新的虚拟机以有选择您 VNet 的选项。 请注意，是否您有现有的虚拟机和 PaaS 实例部署，不能简单地将它们移到您新的 VNet。 这是因为他们需要的网络配置设置会在部署过程中。

## 将虚拟机添加到您的 VNet

创建您 VNet 后，您可以向它添加新虚拟机。 首先，创建 VNet，然后将 VM 部署至关重要。 在部署虚拟机后，不能移动它到 VNet 无需重新部署它。 如果您使用管理门户创建虚拟机，该接口可将 VM 部署到 VNet 时才可用选择**新建 / 计算 / 虚拟机从库**。 经过向导来创建在**虚拟机配置**页上，您的 VM，您将看到**区域中的相似性组/虚拟网络**。 从下拉列表中，选择已创建的 VNet。 有关创建虚拟机的详细信息，请参阅[Azure 的虚拟机](../virtual-machines)。

## 下一步行动

[如何管理虚拟网络 (VNet) 属性](../virtual-networks-settings)

[如何管理 DNS 服务器使用的虚拟网络 (VNet)](../virtual-networks-manage-dns-in-vnet)

[如何在虚拟的网络中使用公用 IP 地址](../virtual-networks-public-ip-within-vnet)

[如何删除虚拟网络 (VNet)](../virtual-networks-delete-vnet)
 
