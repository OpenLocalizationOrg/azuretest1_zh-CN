---
ms.openlocfilehash: 6bd9f623c1da7e5518d4fc4d98e3ea52c2c51524
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="ExpressRoute 采用的系统必备组件 |Microsoft Azure"
   description="此页列出了您可以订购 Azure ExpressRoute 电路之前必须满足的要求。"
   documentationCenter="na"
   services="expressroute"
   authors="cherylmc"
   manager="carolz"
   editor="tysonn"/>
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="07/28/2015"
   ms.author="cherylmc"/>


# Azure 的 ExpressRoute 系统必备组件  

要连接到 Microsoft 云服务使用 ExpressRoute，您需要验证以下先决条件已得到满足。

## 用于连接的系统必备组件

- 一个有效且活动的 Microsoft Azure 帐户。
- 与网络服务提供商 (NSP) 或从[支持列表](expressroute-locations.md)连接需要推动通过其 exchange 提供商 (EXP) 的关系。 您必须与网络服务供应商或 exchange 提供程序具有现有业务关系。 您需要确保您使用的服务是与 ExpressRoute 兼容。
- 如果您要使用的网络服务提供商和网络服务供应商不是受支持的列表中，您仍然可以创建连接到 Azure。
    - 请与您的网络提供商，以查看是否存在任何受支持的列表中显示的交换位置。
    - 具有网络供应商扩展您的网络选择的交换位置。
    - 订购 ExpressRoute 电路通过 Exchange 提供程序连接到 Azure。
- 连接到服务提供商的基础结构。 您必须满足以下各项中至少一个条件︰
    - 网络服务提供商是 VPN 客户和具有至少一个连接到网络服务提供商的 VPN 基础结构的内部网站。 请与网络服务供应商，以查看您的 VPN 服务是否符合 ExpressRoute 的要求。
    - 您的基础架构是 exchange 提供商的数据中心中共存。
    - 已通过以太网连接到 exchange 提供的以太网交换基础结构。
- IP 地址和路由选择的配置进行编号。
    - 您可以使用专用的数字作为连接到 Azure 专用的对等路由域。 如果您选择这样做，它必须是 > 65000。 与数字有关的详细信息，请参阅[自治系统 (AS) 编号](http://www.iana.org/assignments/as-numbers/as-numbers.xhtml)。
    - 若要配置路由的 IP 地址。 一个/28 的子网是必需的。 这必须与您在内部或在 Azure 中使用任何 IP 地址范围不重叠。
    - 您必须使用您自己的公共使用 Azure 公共服务配置 BGP 的会话数。

## 下一步行动

- 有关 ExpressRoute 的详细信息，请参阅[ExpressRoute 的常见问题解答](expressroute-faqs.md)。
- 有关如何配置 ExpressRoute 连接的信息，请参阅︰
    - [配置网络服务供应商通过 ExpressRoute 连接](expressroute-configuring-nsps.md)。
    - [配置通过 Exchange 提供程序连接到 ExpressRoute](expressroute-configuring-exps.md)。

测试
