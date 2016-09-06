---
ms.openlocfilehash: 08fdbadd8c0cdf85ac0544fd7f55352c47fc49f7
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="将流量管理器故障转移通信路由方法配置 |Microsoft Azure"
   description="这篇文章将帮助您在通讯管理器中配置故障转移通信路由方法"
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

# 配置故障切换的路由方法

经常组织希望提供其服务的可靠性。 这是通过提供备份服务，其主要服务出现故障的情况下实现的。 服务故障转移的常见模式是提供一组相同的服务并将通信发送到主服务，同时维护一个或多个备份服务的配置的列表。 您可以按照下面的过程使用 Azure 的云服务和网站配置这种类型的备份。

请注意 Azure 网站已经不考虑网站模式提供故障转移通信路由方法在数据中心 （也称为地区），网站的功能。 通信管理器允许您在不同数据中心中指定故障转移通信路由方法的网站。

## 要配置故障转移通信路由方法︰

1. 在管理门户中，在左窗格中，单击以打开通讯管理器窗格中的**流量管理器**图标。 如果尚未创建您的流量管理器配置文件，请参阅[管理通信管理器配置文件](traffic-manager-manage-profiles.md)的步骤来创建一个基本的流量管理器配置文件。
2. 在管理门户中的通讯管理器窗格中，定位包含要修改的设置的流量管理器配置文件，然后单击配置文件名称右侧的箭头。 这将打开设置页面的配置文件。
3. 在您的配置文件页上，单击顶部的页上的**终结点**，并验证两个云服务以及您想要在您的配置中包含的网站 （终结点） 存在。 若要添加或删除终结点的步骤，请参阅[管理终结点通信管理器中](traffic-manager-endpoints.md)。
4. 个人资料页，单击**配置**以打开配置页的顶部。
5. 对于**通信路由方法设置**，请验证通信量路由方法是**故障转移**。 如果不是，请从下拉列表中单击**故障转移**。
6. 有关**故障转移优先级列表**，调整完终结点的故障转移顺序。 选择**故障转移**通信路由方法时，所选端点的顺序很重要。 主要终点是在最前面。 使用向上键和向下箭头键根据需要更改的顺序。 有关如何使用 Windows PowerShell 设置故障转移优先级的信息，请参阅[设置 AzureTrafficManagerProfile](http://go.microsoft.com/fwlink/p/?LinkId=400880)。
7. 验证正确配置了这些**监视设置**。 监视可以确保处于离线状态的终结点不会发送通信。 为了监视终结点，您必须指定的路径和文件名。 请注意，使用正斜杠"/"是相对路径的有效条目，表示该文件位于根目录下 （默认值）。 有关监视的详细信息，请参阅[通信量管理器监视](traffic-manager-monitoring.md)。
8. 完成配置更改后，单击页面底部的**保存**。
9. 测试您的配置中所做的更改。 有关更多信息，请参见[测试流量管理器设置](traffic-manager-testing-settings.md)。
10. 一旦您的流量管理器配置文件已安装并正在运行，编辑在权威 DNS 服务器上为您公司的域名指向的流量管理器的域名的 DNS 记录。 有关如何执行此操作的详细信息，请参阅[点公司 Internet 域添加到通讯管理器域](traffic-manager-point-internet-domain.md)。

## 下一步行动

[有关通信量管理器通信路由方法](traffic-manager-load-balancing-methods.md)

[通信管理器是什么？](traffic-manager-overview.md)

[通信管理器-禁用，请启用或删除配置文件](disable-enable-or-delete-a-profile.md)

[通信管理器-禁用或启用端点](disable-or-enable-an-endpoint.md)

[云服务](http://go.microsoft.com/fwlink/?LinkId=314074)

[网站](http://go.microsoft.com/fwlink/p/?LinkId=393327)

[操作上流量管理器 （REST API 参考）](http://go.microsoft.com/fwlink/?LinkId=313584)

[Azure 的流量管理器 Cmdlet](http://go.microsoft.com/fwlink/p/?LinkId=400769)
 