---
ms.openlocfilehash: 2317b1ae9f03b1e7cacc5ae9227f17c85a2e7cce
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="创建记录集和记录的 DNS 区域 |Microsoft Azure"
   description="如何创建用于 Azure DNS 主机记录。设置记录设置和记录使用 PowerShell 或 CLI"
   services="dns"
   documentationCenter="na"
   authors="joaoma"
   manager="Adinah"
   editor=""/>

<tags
   ms.service="dns"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/02/2015"
   ms.author="joaoma"/>


# 创建 DNS 记录

> [AZURE.SELECTOR]
- [Azure CLI](dns-getstarted-create-recordset-cli.md)
- [Azure Powershell 步骤](dns-getstarted-create-recordset.md)



在创建您的 DNS 区域之后, 您需要添加您的域的 DNS 记录。  为此，首先需要了解 DNS 记录和记录集。


## 了解记录集和记录
每个 DNS 记录都有一个名称和一个类型。

_完全限定_名包括区域名称，而_相对_的名称则不。  例如，相对的记录名称区域 'contoso.com' www 提供完全限定的记录名称 www.contoso.com。

>[AZURE.NOTE] 在 Azure DNS 记录使用相对名称进行指定。

记录在根据它们所包含的数据的各种来源。  最常见的类型是 A 记录，它将名称映射到 IPv4 地址。  另一种类型是 MX 的记录，它将名称映射到的邮件服务器。

Azure DNS 支持所有常见的 DNS 记录类型︰ A、 AAAA、 CNAME、 MX，NS、 SOA、 SRV 和 TXT。

有时，您需要创建具有给定名称和类型的多个 DNS 记录。  例如，假设 www.contoso.com 网站位于两个不同的 IP 地址。  这就要求两个不同的记录，另一个用于每个 IP 地址︰

    www.contoso.com.        3600    IN  A   134.170.185.46
    www.contoso.com.        3600    IN  A   134.170.188.221

这是一种记录集。  记录集是在具有相同名称和相同类型的区域中 DNS 记录的集合。  大多数的记录集包含单个记录，但如上图中的记录集包含多个记录的示例并非少见。  (记录集类型的 SOA 和 CNAME 是个例外，不允许对这些类型同名的多个记录 DNS 标准。)

-生存时间，或 TTL 指定多长时间的每条记录被缓存客户端之前对其进行重新查询。  在上面的示例中，TTL 是 3600 秒或 1 小时。  TTL 指定为记录集，不是为每条记录，因此为该记录集内的所有记录中使用相同的值。

>[AZURE.NOTE] Azure DNS 管理 DNS 记录使用记录集。



## 创建记录集和记录使用 CLI

下面的示例中我们将介绍如何创建记录集和记录。  我们将使用 DNS A 记录类型，请为其他记录类型，请参阅[如何管理 DNS 记录](dns-operations-recordsets-cli.md)


### 第 1 步

创建记录集使用 CLI:

    Usage: network dns record-set create <resource-group> <dns-zone-name> <name> <type> <ttl>

    azure network dns record-set create myresourcegroup  contoso.com  www A  60

记录集 'contoso.com' 的 DNS 区域中有相对名称 www，因此记录的完全合格的名称将为 www.contoso.com。  记录类型为 A 和 TTL 是 60 秒。

>[AZURE.NOTE] 若要创建记录集，在该区域的顶点 (在这种情况下，'contoso.com')，使用记录名称"@"，包括引号。 这是一个常见的 DNS 约定。

记录集为空，我们必须添加要能够使用新创建的"www"记录集的记录。<BR>

### 第 2 步

将添加到"www"记录 IPv4 A 记录集使用下面的命令︰

    Usage: network dns record-set add-record <resource-group> <dns-zone-name> <record-set-name> <type>

    azure network dns record-set add-record myresourcegroup contoso.com  www A  -a 134.170.185.46
    

更改已完成。  您可以检索记录集从 Azure DNS 使用"azure 网络 dns 记录集显示":


    azure network dns record-set show myresourcegroup "contoso.com" www A
    
    info:    Executing command network dns-record-set show
    + Looking up the DNS record set "www"
    data:    Id                              : /subscriptions/########################/resourceGroups/myresourcegroup/providers/Microsoft.Network/dnszones/contoso.com/A/www
    data:    Name                            : www
    data:    Type                            : Microsoft.Network/dnszones/A
    data:    Location                        : global
    data:    TTL                             : 300
    data:    A records:
    data:        IPv4 address                : 134.170.185.46
    data:
    info:    network dns record-set show command OK


您可以使用 nslookup 或其他 DNS 的工具来查询新的记录集。  

>[AZURE.NOTE] 根据创建区域，如果您有不委派到 Azure DNS 名称服务器的域时，需要显式指定区域的名称服务器地址。


    C:\> nslookup www.contoso.com ns1-01.azure-dns.com

    Server: ns1-01.azure-dns.com
    Address:  208.76.47.1

    Name:    www.contoso.com
    Addresses:  134.170.185.46
                



## 下一步行动
[如何管理 DNS 区域](dns-operations-dnszones-cli.md)

[如何管理 DNS 记录](dns-operations-recordsets-cli.md)<BR>

[自动化与.NET SDK 的 Azure 操作](dns-sdk.md)
 
测试
