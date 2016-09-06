---
ms.openlocfilehash: 59ab8c0cdeb93e27360b96deab28740ee7f91ffa
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="如何设置在 Azure API 管理 VPN 连接"
    description="学习如何设置在 Azure API 管理和访问 web 服务，它通过 VPN 连接。"
    services="api-management"
    documentationCenter=""
    authors="antonba"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="api-management"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/16/2015"
    ms.author="antonba"/>

# 如何设置在 Azure API 管理 VPN 连接

API 管理对 VPN 的支持允许您将您的 API 管理代理服务器连接到 Azure 虚拟网络。 这使 API 管理客户安全地连接到相应的后端 web 服务内部或在其他方面不能访问公用互联网。

## <a name="enable-vpn"> </a>启用 VPN 连接

>VPN 连接才可用**特优**级中。 若要切换到它，[管理门户][]中打开您的 API 管理服务，然后打开**缩放比例**选项卡。 在**常规**部分下选择特优层，并单击保存。

若要启用 VPN 连接，请在[管理门户][]中打开您的 API 管理服务并切换到**配置**选项卡。 

在 VPN 部分中，切换到**上**的**VPN 连接**。

![配置 API 管理实例的选项卡][api-management-setup-vpn-configure]

现在，您将看到配置 API 管理服务时的所有地区的列表。

选择 VPN 和每个区域的子网。 Vpn 的列表填充基于 Azure 订购可在您正在配置的区域中设置的虚拟网络。

![选择 VPN][api-management-setup-vpn-select]

单击**保存**在屏幕的底部。 您将不能从 Azure 管理门户中执行其他操作的 API 管理服务，虽然它更新。 服务代理将保持可用，应不会影响运行时调用。

请注意的 VIP 地址的代理服务器将更改每次启用或禁用 VPN。

## <a name="connect-vpn"> </a>连接到 web 服务背后 VPN

API 管理服务连接到 VPN 访问虚拟网络中的 web 服务后，访问公共服务相比没有什么不同。 只需键入本地地址或主机名 （如果 DNS 服务器被配置为 Azure 虚拟网络） 的 web 服务**的 Web 服务 URL**字段中创建一个新的 API 时或编辑一个现有。

![从 VPN 添加 API][api-management-setup-vpn-add-api]


## <a name="related-content"> </a>相关的内容


 * [教程︰ 创建站点到站点连接的跨部署虚拟网络][]
 * [在 Azure API 管理中如何使用 API 跟踪检查呼叫][]

[api 的管理-设置的 vpn 的配置]: ./media/api-management-howto-setup-vpn/api-management-setup-vpn-configure.png
[api 的管理-设置的 vpn 的选择]: ./media/api-management-howto-setup-vpn/api-management-setup-vpn-select.png
[api-management-setup-vpn-add-api]: ./media/api-management-howto-setup-vpn/api-management-setup-vpn-add-api.png

[启用 VPN 连接]: #enable-vpn
[连接到 web 服务背后 VPN]: #connect-vpn
[相关的内容]: #related-content

[管理门户]: https://manage.windowsazure.com/

[教程︰ 创建站点到站点连接的跨部署虚拟网络]: ../virtual-networks-create-site-to-site-cross-premises-connectivity
[在 Azure API 管理中如何使用 API 跟踪检查呼叫]: api-management-howto-api-inspector.md
 
测试
