---
ms.openlocfilehash: cfb2ca695309e6d0ad3b9dd98b70b8b98c1880b4
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="通信管理器配置圆形复用通信路由方法 |Microsoft Azure"
   description="这篇文章将帮助您配置循环负载平衡管理器通信的终结点。"
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

# 配置循环路由方法

一种常见的通信路由方法模式是提供一套相同的端点，包括云服务和网站，并将通信发送到每一轮循机制方式。 以下步骤概述了如何配置通信管理器来执行这种类型的通信量路由方法。 不同的通信路由方法的详细信息，请参阅[关于通信管理器通信路由方法](traffic-manager-load-balancing-methods.md)。

>[AZURE.NOTE] Azure 网站已经提供了循环负载平衡功能的数据中心 （也称为区域） 内的网站。 通信管理器允许您在不同数据中心中指定循环流量路由方法的网站。

## 在一组终结点的路由流量均匀 （循环）︰

1. 在管理门户中，在左窗格中，单击以打开通讯管理器窗格中的**流量管理器**图标。 如果尚未创建您的流量管理器配置文件，请参阅[管理通信管理器配置文件](traffic-manager-manage-profiles.md)的步骤来创建一个基本的流量管理器配置文件。
2. 在管理门户中，在通讯管理器窗格中，定位包含要修改的设置的流量管理器配置文件，然后单击配置文件名称右侧的箭头。 这将打开设置页面的配置文件。
3. 在您的配置文件页上，单击顶部的页上的**终结点**并检查存在您想要在您的配置中包含的服务终结点。 若要添加或删除终结点的步骤，请参阅[管理终结点通信管理器中](traffic-manager-endpoints.md)。
4. 个人资料页，单击**配置**以打开配置页的顶部。
5. 对于**通信路由方法设置**，验证通信量路由方法**循环复用**。 如果不是这样，在下拉列表中单击**循环**。
6. 验证正确配置了这些**监视设置**。 监视可以确保处于离线状态的终结点不会发送通信。 为了监视终结点，您必须指定的路径和文件名。 请注意，使用正斜杠"/"是相对路径的有效条目，表示该文件位于根目录下 （默认值）。 有关监视的详细信息，请参阅[通信量管理器监视有关](traffic-manager-monitoring.md)。
7. 完成配置更改后，单击页面底部的**保存**。
8. 测试您的配置中所做的更改。 有关详细信息，请参阅[测试流量管理器设置](traffic-manager-testing-settings.md)。
9. 一旦您的流量管理器配置文件已安装并正在运行，编辑在权威 DNS 服务器上为您公司的域名指向的流量管理器的域名的 DNS 记录。 有关如何执行此操作的详细信息，请参阅[点公司 Internet 域添加到通讯管理器域](traffic-manager-point-internet-domain.md)。

## 下一步行动

[有关通信量管理器通信路由方法](traffic-manager-load-balancing-methods.md)

[通信管理器-禁用，请启用或删除配置文件](disable-enable-or-delete-a-profile.md)

[通信管理器-禁用或启用端点](disable-or-enable-an-endpoint.md)

[通信管理器是什么？](../traffic-manager-overview.md)

[云服务](http://go.microsoft.com/fwlink/?LinkId=314074)

[网站](http://go.microsoft.com/fwlink/p/?LinkId=393327)

[操作上流量管理器 （REST API 参考）](http://go.microsoft.com/fwlink/?LinkId=313584)

[Azure 的流量管理器 Cmdlet](http://go.microsoft.com/fwlink/p/?LinkId=400769)
 