---
ms.openlocfilehash: 296757230ae26ef0fb26f3c15e8c23e45d64b114
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="禁用或启用一个流量管理器终结点 |Microsoft Azure"
   description="这篇文章将帮助禁用或启用您的流量管理器配置终结点。"
   services="traffic-manager"
   documentationCenter="na"
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

# 禁用或启用一个流量管理器终结点

您还可以禁用各个终结点的通信管理器配置文件的一部分。 终结点包括云服务和网站。 禁用某个终结点会使其作为一部分的配置文件，但该配置文件的行为就像终结点不包含在其中。 此操作会非常有用暂时删除处于维护模式或重新配置的终结点。 再次运行后该终结点，可以启用

>[AZURE.NOTE] **禁用某个终结点具有与 Azure 中的部署状态无关。 一个健康的终结点将保持向上的能够接收通信，即使在通讯管理器中禁用。 此外，禁用一个配置文件中的终结点不会影响在另一个配置文件中的状态。**

## 若要禁用某个终结点

1. 在管理门户中的通讯管理器窗格中，定位包含要修改的终结点设置的流量管理器配置文件，然后单击配置文件名称右侧的箭头。 这将打开设置页面的配置文件。
1. 在页面的顶部，请单击以查看包括在您的配置的终结点的**终结点**。 
1. 单击您想要禁用，该终结点，然后单击页面底部的**禁用**。
1. 通信量将停止流动到基于 DNS 的生存时间 (TTL) 的流量管理器的域名为配置的终结点。 从通信管理器配置文件的配置页，您可以更改 TTL。

## 若要启用终结点


1. 在管理门户中的通讯管理器窗格中，定位包含要修改的终结点设置的流量管理器配置文件，然后单击配置文件名称右侧的箭头。 这将打开设置页面的配置文件。
1. 在页面的顶部，请单击以查看包括在您的配置的终结点的**终结点**。
1. 单击您想要启用，请终结点，然后单击页面底部的**启用**。
1. 通信将开始流入再次以听写的配置文件服务。

## 下一步行动

[通信管理器-禁用，请启用或删除配置文件](disable-enable-or-delete-a-profile.md)

[通信管理器是什么？](../traffic-manager.md)

[云服务](http://go.microsoft.com/fwlink/?LinkId=314074)

[网站](http://go.microsoft.com/fwlink/p/?LinkId=393327)


[操作上流量管理器 （REST API 参考）](http://go.microsoft.com/fwlink/?LinkId=313584)
 