---
ms.openlocfilehash: 5c7626e76de3183b089466db3cd2c95b83800862
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="指向一个流量管理器域公司 Internet 域 |Microsoft Azure"
   description="这篇文章将有助于您公司的域名指向一个流量管理器的域名。"
   services="traffic-manager"
   documentationCenter=""
   authors="joaoma"
   manager="adinah"
   editor="tysonn" />
<tags
   ms.service="traffic-manager"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/19/2015"
   ms.author="joaoma" />

# 点到 Azure 流量管理器域公司 Internet 域

为您公司的域名指向一个流量管理器的域名，修改在 Internet 的 DNS 服务器上使用 CNAME 记录类型，将您公司的域名映射到您的流量管理器配置文件的域名的 DNS 资源记录。 通信管理器配置文件的配置页上，可以看到在**常规**部分中的流量管理器域名称。

例如，指向通信管理器域名称 contoso.trafficmanager.net 公司域的名称 www.contoso.com，您更新您的 DNS 资源记录，为下面︰

    www.contoso.com IN CNAME contoso.trafficmanager.net

对*www.contoso.com*的所有通信请求现在将都转向*contoso.trafficmanager.net*。

>[AZURE.IMPORTANT] 不能将第二层域，如*contoso.com*，指向流量管理器的域中。 这是不允许的二级域名的 CNAME 记录 DNS 协议的限制。

## 下一步行动

[有关通信量管理器通信路由方法](traffic-manager-load-balancing-methods.md)

[通信管理器-禁用，请启用或删除配置文件](disable-enable-or-delete-a-profile.md)

[通信管理器-禁用或启用端点](disable-or-enable-an-endpoint.md)

[通信管理器是什么？](traffic-manager-overview.md)
