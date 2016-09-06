---
ms.openlocfilehash: d070995166e16371c97901ce8477b04572a6f668
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="配置性能通信路由方法 |Microsoft Azure"
   description="这篇文章将帮助您配置通信量管理器中的性能通信路由方法"
   services="traffic-manager"
   documentationCenter=""
   authors="joaoma"
   manager="adinah"
   editor="tysonn" />
<tags 
   ms.service="traffic-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/19/2015"
   ms.author="joaoma" />

# 配置性能通信路由方法

以流量为云服务和全球 （也称为区域） 位于不同数据中心的网站 （终结点），则可以直接传入通信指向该终结点和最低的延迟从发出请求的客户端。 数据中心和最低的延迟通常情况下，对应于最接近的地理距离。 性能通信路由方法允许您将分发基于最低的延迟，但是不能考虑实时更改网络配置或负载中的帐户。 了解更多有关不同的通信路由方法的 Azure 流量管理器提供，ses[通信管理器关于通信路由方法](traffic-manager-load-balancing-methods.md)。

## 根据最低的延迟在一组终结点的通信路由︰

1. 在管理门户中，在左窗格中，单击以打开通讯管理器窗格中的**流量管理器**图标。 如果尚未创建您的流量管理器配置文件，请参阅[管理通信管理器配置文件](traffic-manager-manage-profiles.md)的步骤来创建一个基本的流量管理器配置文件。
2. 在管理门户中，在通讯管理器窗格中，定位包含要修改的设置的流量管理器配置文件，然后单击配置文件名称右侧的箭头。 这将打开设置页面的配置文件。
3. 在您的配置文件页上，单击顶部的页上的**终结点**并检查存在您想要在您的配置中包含的服务终结点。 若要添加或删除终结点从您的配置文件的步骤，请参阅[管理终结点通信管理器中](traffic-manager-endpoints.md)。
4. 在您的配置文件页上，在顶部以打开配置页中单击**配置**。
5. 对于**通信路由方法设置**，请验证通信量路由方法**性能*。如果不是，请单击**性能** 在下拉列表中。
6. 验证正确配置了这些**监视设置**。 监视可以确保处于离线状态的终结点不会发送通信。 为了监视终结点，您必须指定的路径和文件名。 请注意，使用正斜杠"/"是相对路径的有效条目，表示该文件位于根目录下 （默认值）。 有关监视的详细信息，请参阅[通信量管理器监视有关](traffic-manager-monitoring.md)。
7. 完成配置更改后，单击页面底部的**保存**。
8. 测试您的配置中所做的更改。 有关详细信息，请参阅[测试流量管理器设置](traffic-manager-testing-settings.md)。
9. 一旦您的流量管理器配置文件已安装并正在运行，编辑在权威 DNS 服务器上为您公司的域名指向的流量管理器的域名的 DNS 记录。 有关如何执行此操作的详细信息，请参阅[点公司 Internet 域添加到通讯管理器域](traffic-manager-point-internet-domain.md)。

## 下一步行动

[有关通信量管理器通信路由方法](traffic-manager-load-balancing-methods.md)

[通信管理器-禁用，请启用或删除配置文件](disable-enable-or-delete-a-profile.md)

[通信管理器-禁用或启用端点](disable-or-enable-an-endpoint.md)

[通信管理器是什么？](../traffic-manmager-overview.md)

[云服务](http://go.microsoft.com/fwlink/?LinkId=314074)

[网站](http://go.microsoft.com/fwlink/p/?LinkId=393327)

[操作上流量管理器 （REST API 参考）](http://go.microsoft.com/fwlink/?LinkId=313584)

[Azure 的流量管理器 Cmdlet](http://go.microsoft.com/fwlink/p/?LinkId=400769)

 