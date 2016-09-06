---
ms.openlocfilehash: 2aca63bc5ccbcf0ec358d6c6a51c61b41895580e
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="Azure DNS 概述 |Microsoft Azure" 
   description="Azure DNS 承载 Microsoft Azure 和开始承载您在 Microsoft Azure 上的域上的服务的概述" 
   services="dns" 
   documentationCenter="na" 
   authors="joaoma" 
   manager="adinah" 
   editor=""/>

<tags
   ms.service="dns"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services" 
   ms.date="08/12/2015"
   ms.author="joaoma"/>

# Azure DNS 概述

任何网站或 Internet 上的服务，背后还有一个 IP 地址。 例如，www.microsoft.com 使用 IP 地址 134.170.185.46。 域名系统或 DNS，负责翻译 （或解析） 为其 IP 地址的网站或服务的名称。

Azure DNS 是 DNS 域的宿主服务提供名称解析使用 Microsoft Azure 基础结构。 通过承载您在 Azure 中的域，您可以管理您的 DNS 记录使用相同的凭据，Api、 工具和作为其他 Azure 服务付费。

在 Azure 的全球网络的 DNS 名称服务器上承载在 Azure DNS 的 DNS 域。  我们使用任意广播网络，以便每个 DNS 查询应答由最接近的可用 DNS 服务器。 这为您的域提供更快的性能和高可用性。

该服务基于 Azure 资源管理器 (ARM)。  通过 Azure 资源管理器的 REST Api、.NET SDK，PowerShell cmdlet 和命令行界面，可以管理您的域和记录。  Azure DNS 当前在预览，在 Azure 管理门户中尚不支持。<BR><BR>
Azure DNS 当前不支持购买的域名。  若要购买域应使用第三方域的域名注册，通常会收取少量的年度费用。  这些域然后 Azure DNS 中用于管理 DNS 记录的宿主--有关详细信息，请参见[委托到 Azure DNS 域](dns-domain-delegation.md)。


## 下一步行动

[开始创建 DNS 区域](dns-getstarted-create-dnszone.md)

[自动化与.NET SDK 的 Azure 操作](dns-sdk.md)

[Azure DNS REST API 参考](https://msdn.microsoft.com/library/azure/mt163862.aspx)




 
测试
