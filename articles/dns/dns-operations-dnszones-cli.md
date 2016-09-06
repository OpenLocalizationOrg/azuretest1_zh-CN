---
ms.openlocfilehash: 8099946511dfaea30b9d06c576f593228390d38f
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="DNS 区域上的操作 |Microsoft Azure" 
   description="您可以管理使用 Azure Powershell cmdlet 或 CLI 的 DNS 区域。 如何更新、 删除和创建在 Azure DNS 的 DNS 区域" 
   services="dns" 
   documentationCenter="na" 
   authors="joaoma" 
   manager="Adinah" 
   editor=""/>

<tags
   ms.service="dns"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services" 
   ms.date="09/02/2015"
   ms.author="joaoma"/>

# 如何管理 DNS 区域

> [AZURE.SELECTOR]
- [Azure CLI](dns-operations-dnszones-cli.md)
- [Azure Powershell](dns-operations-dnszones.md)

本指南将显示如何管理您的 DNS 区域。 它将帮助了解操作以管理您的 DNS 区域的顺序。

## 创建新的 DNS 区域

若要创建新的 DNS 区域来承载您的域，请使用`azure network dns zone create`:

        Azure network dns zone create -n contoso.com -g myresourcegroup -t "project=demo";"env=test"

该操作在 Azure DNS 中创建新的 DNS 区域。 有关详细信息请参阅[Etag 和标记](dns-getstarted-create-dnszone.md#Etags-and-tags)，您可以选择指定数组的 Azure 资源管理器标记。

区域名称中必须是唯一的资源组中，并且区域必须已经存在，否则操作将失败。

同一区域名称可以重复使用不同的资源组或其他 Azure 订阅中。  多个区域共享相同的名称，每个实例都将分配到不同的名称服务器地址，并可以从父域委派只能有一个实例。 有关更多信息，请参见[委托到 Azure DNS 域](dns-domain-delegation.md)。

## 得到一个 DNS 区域

若要检索的 DNS 区域，请使用`azure network dns zone show`:

    azure network dns zone show myresourcegroup contoso.com

操作返回其 id、 数记录集和标记与 DNS 区域。


## 列表中的 DNS 区域

若要检索资源组内的 DNS 区域，请使用`azure network dns zone list`:

    azure network dns zone list myresourcegroup


## 更新 DNS 区域

可以使用所做更改为 DNS 区域资源`azure network dns zone set`。  这不会更新任何 DNS 区域中的记录集 （请参阅[如何管理 DNS 记录](dns-operations-recordsets.md)）。 它仅用于更新区域资源本身的属性。 这是当前限制到 Azure 资源管理器区域资源的标记。 有关详细信息，请参阅[Etag 和标记](dns-getstarted-create-dnszone.md#Etags-and-tags)。

    azure network dns zone set myresourcegroup contoso.com -t prod=value2

## 删除 DNS 区域

可以使用删除 DNS 区域`azure network dns zone delete`。
 
在删除之前在 Azure DNS 的 DNS 区域，将需要删除所有记录集，在该区域的根目录创建区域时自动创建 NS 和 SOA 记录除外。  

    azure network dns zone delete myresourcegroup contoso.com 

该操作有一个可选-q 禁止提示您确认要删除 DNS 区域的开关。


## 下一步行动


[管理 DNS 记录](dns-operations-recordsets-cli.md)

[使用.NET SDK 的自动化操作](dns-sdk.md)测试
