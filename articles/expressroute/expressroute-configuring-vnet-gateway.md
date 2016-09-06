---
ms.openlocfilehash: 94d47d6ecffe92a20be8c3c8830e0b63c8c5cc63
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="配置虚拟网络的 Expressroute |Microsoft Azure"
   description="这篇文章将引导您完成设置虚拟网络 (VNet) ExpressRoute"
   documentationCenter="na"
   services="expressroute"
   authors="cherylmc"
   manager="carolz"
   editor="tysonn" />

<tags 
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="article" 
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services" 
   ms.date="07/28/2015"
   ms.author="cherylmc"/>

#  配置为 ExpressRoute 的虚拟网络

下列步骤将指导您完成配置为 ExpressRoute 的虚拟网络和网关。

1. 登录到**管理门户**中。
2. 在屏幕的左下角，单击**新建**。 在导航窗格中，单击**网络服务**，然后单击**虚拟网络**。 单击**创建自定义**启动配置向导。
3. 在**虚拟的网络详细信息**页上，输入以下信息。

    - **名称**– 虚拟网络名称。 部署您的虚拟机和 PaaS 的实例，因此您可能不想要太复杂的名称时，您将使用此虚拟网络名称。
    - **位置**– 位置直接关系到希望资源 (Vm) 所在的物理位置 （区域）。 例如，如果您希望将 Vm 部署到物理上位于东亚美国此虚拟网络，选择该位置。 不能更改所创建与虚拟网络的地区。

4. 在**DNS 服务器和 VPN 连接**页面上，输入以下信息，然后单击右下方的下箭头。 

    - **DNS 服务器**的 DNS 服务器的名称和 IP 地址，请输入或从下拉列表中选择以前已注册的 DNS 服务器。 此设置不会创建一个 DNS 服务器，它允许您指定要使用的虚拟网络名称解析的 DNS 服务器。
    - **配置站点到站点 VPN**的**站点到站点 VPN 配置**为选择复选框。
    - **选择 ExpressRoute** – 选中此复选框，**使用 ExpressRoute**。 如果上一步中选择了***站点到站点 VPN 配置***，此选项才会显示。
    - **本地网络**的本地网络代表内部部署物理位置。 您可以选择以前创建的本地网络或您可以创建新的本地网络。

    如果您选择一个现有的本地网络，请跳过步骤 5。

5. 如果您正在创建新的本地网络，则您将看到**站点对站点连接**页。 如果您选择以前创建的本地网络，在向导中将不显示此页面，您可以将移动到下一节。 要配置您的本地网络，请输入以下信息，然后单击向下箭头。 

    - **名称**-想要调用本地 （内部部署） 的名称的网络站点。
    - **地址空间**-包括起始 IP 和 CIDR （地址计数）。 只要它不与虚拟网络的地址范围重叠，您可以指定任何地址范围。
    - **添加地址空间**-此设置不适用于 ExpressRoute。
**注意︰**您需要为 ExpressRoute 中创建一个本地网络站点。 为本地网络站点指定的地址前缀将被忽略。 将为进行路由而使用 ExpressRoute 电路通过广告给 Microsoft 的地址前缀。

6. 在**虚拟的网络地址空间**页上，输入以下信息，然后单击右下方以配置您的网络上的选中标记。 

    - **地址空间**-包括启动 IP 和地址计数。 请验证您指定的地址空间不重叠任何您有在您的本地网络的地址空间。
    - **添加子网**-包括起始 IP 和地址数。 其他的子网不是必需的但您可以为虚拟机的动态 IP 地址 (DIP) 创建一个单独的子网。 或者，您可能想要您的 Vm 是 PaaS 实例分开的子网中。
    - **添加网关网**-单击以添加网关网。 网关网仅用于虚拟网络网关和对于这种配置是必需的。 
    ***重要︰*** ExpressRoute 的网关子网必须是 /28 或更大。

7. 单击页面底部的选中标记和虚拟网络将开始创建。 完成后，您将看到**创建**管理门户中的**网络**页上列出**状态**下。

8. 在**网络**页面中，单击刚创建的虚拟网络，然后单击**仪表板**。
9. 在仪表板页的底部，单击**创建网关**，然后单击**是**。

10. 网关在启动时创建，您将看到消息让您知道网关已启动。 可能需要 15 分钟的时间来创建网关。
11. **将您的网络链接到电路。** 仅在您已确认您电路已移动到以下状态和状态后，才能继续下面的说明︰ 

    - ServiceProviderState︰ 设置
    - 状态︰ 启用

    请与网关创建确认有至少一个 Azure 的虚拟网络。 网关网必须是 /28 或更大，为了使用 ExpressRoute 连接，必须启动并运行。

            PS C:\> $Vnet = "MyTestVNet"
            New-AzureDedicatedCircuitLink -ServiceKey $ServiceKey -VNetName $Vnet
            Provisioned

## 下一步行动
如果您想要将虚拟机添加到虚拟网络，请参阅[如何创建一个自定义虚拟机](../virtual-machines-create-custom.md)。

如果您想要了解有关 ExpressRoute 的详细信息，请参阅[ExpressRoute 技术概述](expressroute-introduction.md)。


 

测试
