---
ms.openlocfilehash: 290ee7ec6232a9de06072f7f72021068d32703f0
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="禁用、 启用或删除一个流量管理器的配置文件 |Microsoft Azure"
   description="这篇文章将帮助您使用您的流量管理器配置文件。"
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

# 禁用、 启用或删除配置文件


可以禁用现有的流量管理器配置文件，以便它将引用其配置终结点用户的请求。 当您禁用一个流量管理器配置文件本身的配置文件，配置文件中包含的信息将保持不变，和通信管理器界面中可以进行编辑。 当您想要重新启用该配置文件时，您可以轻松地做到管理门户中，并且会继续引用。 管理门户中创建一个流量管理器的配置文件时，会自动启用。

## 若要禁用配置文件

1. 修改 Internet DNS 服务器在 Internet 上使用的适当的记录类型和指针，它指向另一个名称或特定位置的 IP 地址上的 DNS 资源记录。 换句话说，更改在 Internet 的 DNS 服务器上的 DNS 资源记录，以使其不再使用 CNAME 资源记录指向您的流量管理器配置文件的域名。
1. 通信将停止被定向到的流量管理器配置文件设置到的终结点。
1. 选择您想要禁用配置文件。 选择通讯管理器页上的配置文件，请通过单击配置文件名称旁边的列中选中的配置文件。 因为这会将您带到设置页面的配置文件，请不要单击名称旁边的箭头或配置文件的名称。
1. 在选择配置文件后，单击禁用底部的页上。

## 若要启用配置文件

1. 选择您要启用的配置文件。 选择通讯管理器页上的配置文件，请通过单击配置文件名称旁边的列中选中的配置文件。 因为这会将您带到设置页面的配置文件，请不要单击名称旁边的箭头或配置文件的名称。
1. 在选择配置文件后，单击启用页面底部。
1. 修改在 Internet 的 DNS 服务器上使用 CNAME 记录类型，将您公司的域名映射到您的流量管理器配置文件的域名的 DNS 资源记录。 有关详细信息，请参阅[点公司 Internet 域添加到通讯管理器域](traffic-manager-point-internet-domain.md)。
1. 通信将开始再次被定向到的终结点。

## 删除配置文件


1. 确保在 Internet 的 DNS 服务器上的 DNS 资源记录不再使用 CNAME 资源记录指向您的流量管理器配置文件的域名称。
1. 选择要删除的配置文件。 若要选择通讯管理器页上的配置文件，选中的配置文件的 
1. 单击配置文件旁边的列。 因为这会将您带到设置页面的配置文件，请不要单击名称旁边的箭头或配置文件的名称。
1. 在选择配置文件后，单击删除页面底部。

## 下一步行动

[通信管理器-禁用或启用端点](disable-or-enable-an-endpoint.md)

[通信管理器是什么？](traffic-manager-overview.md)

 