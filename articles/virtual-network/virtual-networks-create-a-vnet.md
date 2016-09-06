---
ms.openlocfilehash: 5ce9eac285e444c13e4fc4ca5abeec06ddbe91b9
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="创建虚拟网络"
   description="演练可以轻松地创建基本的虚拟网络的步骤。"
   services="virtual-network"
   documentationCenter=""
   authors="telmos"
   manager="carolz"
   editor="tysonn"/>

<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="07/06/2015"
   ms.author="telmosampaio"/>

# 创建虚拟网络

当创建虚拟网络时，您的服务和 VNet 中的虚拟机可以安全地相互通信而无需通过互联网走出去。 创建专用的云专用虚拟网络是相对快速和容易的过程。 仅云的虚拟网络并不是为了跨内部连接，因为不需要获取并配置 VPN 设备或身份验证证书。

创建虚拟网络时，您可以将新的虚拟机和 PaaS 实例添加到它。 请注意，如果您使用管理门户创建虚拟机，请务必选择**剪辑库中**，以便您可以指定虚拟网络。 这很重要，因为您不能返回并置于虚拟网络虚拟机创建虚拟机之后。

[Azure 的注释]**使用此过程可以创建专用的云专用虚拟网。** 由于更大的复杂性涉及创建跨部署配置，不要使用此过程可以创建以后将连接到内部网络的虚拟网络。 如果您想要创建 Azure 和内部网络之间的安全跨本地连接，请参阅[有关安全跨内部连接](vpn-gateway-cross-premises-options.md)。

## 创建虚拟网络

1. 登录到**管理门户**中。
2. 在屏幕的左下角，单击**新建**。 在导航窗格中，单击**网络服务**，然后单击**虚拟网络**。 单击**创建自定义**启动配置向导。
3. 在**虚拟的网络详细信息**页上，输入以下信息，然后单击右下角上的下箭头。 在详细信息页上的设置的详细信息，请参阅[如何管理 VNet 属性](../virtual-networks-settings)的**虚拟网络详细信息**一节。
    -  **名称︰**命名您的虚拟网络。 在部署您的虚拟机和服务，因此最好不能太复杂的名称时，您将使用此虚拟网络名称。

    -  **位置︰**从下拉列表中，选择所需的区域。 在 Azure 数据中心位于指定的区域中，将创建虚拟网络。



4. 在**DNS 服务器和 VPN 连接**页上未做任何更改。 只是向前移动到下一页面通过单击箭头。 默认情况下，Azure 的虚拟网络提供基本名称解析。 很可能您的名称解析需求非常复杂，不是由基本的 Azure 名称解析。 在这种情况下，可能以后要添加到虚拟网络运行 DNS 的虚拟机。 有关 Azure 的名称解析和 DNS 的详细信息，请参阅[名称解析](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md)。
5. **虚拟网络地址空间**页面是您在其中输入要用于此 VNet 的地址空间。 除非您为您的 Vm 需要将某些内部 IP 地址范围或您想要创建的虚拟机将接收静态 DIP 特定的子网，您不必在此页进行任何更改。 如果要创建多个子网，则可以这样在此页上单击**添加子网**。 在详细信息页上的设置的详细信息，请参阅[如何管理 VNet 属性](../virtual-networks-settings)的**虚拟网络详细信息**一节。

    -  在详细信息页上的设置的详细信息，请参阅[如何管理 VNet 属性](../virtual-networks-settings)的**虚拟网络详细信息**一节。
    -  因为您不打算使用跨部署 VPN 配置连接到内部网络这个虚拟的专用网络，不需要调整这些设置与您现有的内部网络 IP 地址范围。 如果您认为要创建跨部署配置以后，您将需要协调现在与已经存在于本地站点来避免路由问题的范围的地址空间。 以后更改范围可以是有点复杂并通常会在无需重新部署您


6. 单击右下角的虚拟网络地址空间页面上的选中标记和虚拟网络将开始创建。 创建虚拟网络后，您将看到创建状态下管理门户中的网络页上列出。
7. 一旦创建虚拟网络，您可以将其部署到您的 VNet。 例如，如果您想要将虚拟机部署到您的 VNet，了解[如何创建自定义 VM](../virtual-machines/virtual-machines-create-custom.md)。 请务必选择**剪辑库中**时创建新的虚拟机以有选择虚拟网络的选项。 请注意，是否您有现有的虚拟机和 PaaS 实例部署，不能简单地将它们移到您新的 VNet。 这是因为在部署过程中为它们配置网络配置设置。 您必须重新将其部署到新的 VNet。



## 下一步行动
-  了解有关在 Azure 中的[虚拟网络](../virtual-network/virtual-networks-overview.md)。 

-  [虚拟机添加](../virtual-machines/virtual-machines-create-custom.md)到虚拟网络。
