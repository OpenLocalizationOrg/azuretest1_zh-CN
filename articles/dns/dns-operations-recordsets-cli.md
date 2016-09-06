---
ms.openlocfilehash: 563833b8409a2729ed2c7bb6a04aaf9c06a05837
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="管理 DNS 记录集和记录在 Azure DNS |Microsoft Azure" 
   description="当承载在 Azure DNS 域管理 DNS 记录集和 Azure DNS 记录。 在记录集和记录的操作的所有 PowerShell 命令。" 
   services="dns" 
   documentationCenter="na" 
   authors="joaoma" 
   manager="Adinah" 
   editor=""/>

<tags
   ms.service="dns"
   ms.devlang="en"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services" 
   ms.date="08/20/2015"
   ms.author="joaoma"/>

# 如何管理 DNS 记录

> [AZURE.SELECTOR]
- [Azure CLI](dns-operations-recordsets-cli.md)
- [Azure Powershell](dns-operations-recordsets.md)

本指南将显示如何管理记录集和记录的 DNS 区域。

请务必了解 DNS 记录集和单个 DNS 记录之间的差别。  记录集是具有相同名称和相同类型的区域中的记录的集合。  有关详细信息，请参阅[了解记录集和记录](dns-getstarted-create-recordset.md#Understanding-record-sets-and-records)。

## 创建记录集

记录集使用创建的`azure network record-set create`命令。  您需要指定记录集名称、 区域、--生存时间 (TTL) 和记录类型。

>[AZURE.NOTE] 记录集名称必须是一个相对的名称，不包括区域名称。  例如，区域 'contoso.com' 中的记录集名称 www 将创建记录集的完全限定的名称 www.contoso.com。

>对于在区域 apex 设置记录，请使用"@"为该记录集的名称，包括引号。  记录集的完全限定名称就等于区域名称，在此种情况下"contoso.com"。

Azure DNS 支持下列记录类型︰ A、 AAAA、 CNAME、 MX，NS、 SOA、 SRV、 TXT。  与每个区域自动创建 SOA 类型的记录集，它们无法单独制定。

    azure network record-set create myresourcegroup contoso.com  www  A --ttl 300


>[AZURE.IMPORTANT] CNAME 记录集与其他具有相同名称的记录集不能共存。  例如，不能创建具有相对名称 www CNAME 和 A 记录具有相对名称 www 在同一时间。  由于区域 apex (名称 = ' @') 始终包含 NS 和 SOA 记录集创建创建该区域时，这意味着您不能创建在区域 apex 设置 CNAME 记录。  这些约束会产生 DNS 标准，它们不是 Azure DNS 的限制。

### 通配符的记录

Azure DNS 支持[通配符的记录](https://en.wikipedia.org/wiki/Wildcard_DNS_record)。  （除非有更匹配从非通配符记录集） 的任何具有匹配名称的查询返回。

>[AZURE.NOTE] 若要创建通配符的记录集，请使用记录集名称"\*"，或名称的第一个标签是"\*"，如"\*.foo"。

>除 NS 和 SOA 的所有记录类型支持通配符的记录集。  

## 获取记录集
若要检索现有的记录集，请使用`azure network dns-record-set show`，指定资源组、 区域名称、 记录集相对名称和记录类型︰

    azure network dns-record-set show myresourcegroup contoso.com www A


## 列表中的记录集

您可以列出所有记录在 DNS 区域中使用`azure network dns-record-set list`命令︰

### 选项 1 
列出所有的记录集。  这将返回所有的记录集，而不考虑名称或记录类型︰

    azure network dns-record-set list myresourcegroup contoso.com

### 选项 2 

列出给定的记录类型的记录集。  这将返回所有匹配给定的记录类型 （在本例中为记录） 的记录集︰


    azure network dns-record-set list myresourcegroup contoso.com A 

在这两种情况下将指定的资源组名称和区域名称。

## 将记录添加到记录集

记录被添加到记录集使用`azure network dns-record-set add-record`。

将记录添加到记录集的参数的记录集的类型而异。 例如，使用类型 A 记录集时只可以来使用参数指定记录"- `<IPv4 address>`"。

下面的示例演示如何创建包含一条记录的每个记录类型的记录集。

### 创建与单个记录集的记录

若要创建记录集，请使用`azure network dns-record-set create`，指定资源组、 区域名称、 记录实时 (ttl) 设置相对名称、 记录类型和时间︰
    
    azure network dns-record-set create myresourcegroup  contoso.com "test-a"  A --ttl 300

>[AZURE.NOTE] 如果未定义 — ttl 参数，则缺省值为 4 （以秒为单位）。


创建一个记录集，添加 IPv4 后记录的地址设置为`azure network dns-record-set add-record`:

    azure network dns-record-set add-record myresourcegroup contoso.com "test-a" A -a 192.168.1.1 

### 创建具有单个记录的 AAAA 记录集

    azure network dns-record-set create myresourcegroup contoso.com "test-aaaa" AAAA --ttl 300

    azure network dns-record-set add-record myresourcegroup contoso.com "test-aaaa" AAAA -b "2607:f8b0:4009:1803::1005"

### 使用单个记录创建 CNAME 记录集

    azure network dns-record-set create -g myresourcegroup contoso.com  "test-cname" CNAME --ttl 300
    
    azure network dns-record-set add-record  myresourcegroup contoso.com  test-cname CNAME -c "www.contoso.com"

>[AZURE.NOTE] CNAME 记录只允许一个单个字符串值。 

### 创建具有单个记录的 MX 记录集

在此示例中，我们使用记录集名称"@"区域 apex (例如"contoso.com") 在创建 MX 记录。  这是常见的 MX 记录。

    azure network dns-record-set create myresourcegroup contoso.com  "@"  MX --ttl 300

    azure network dns-record-set add-record -g myresourcegroup contoso.com  "@" MX -e "mail.contoso.com" -f 5


### 使用单个记录创建 NS 记录集

    azure network dns-record-set create myresourcegroup contoso.com test-ns  NS --ttl 300
    
    azure network dns-record-set add-record myresourcegroup  contoso.com  "test-ns" NS -d "ns1.contoso.com" 
    
### 使用单个记录创建 SRV 记录集

如果在区域的根目录中创建一条 SRV 记录，只需指定 _service 和 _protocol 中记录名称 — — 没有必要，也包括。 @ 中记录名称

    
    azure network dns-record-set create myresourcegroup contoso.com "_sip._tls" SRV --ttl 300 

    azure network dns-record-set add-record myresourcegroup contoso.com  "_sip._tls" SRV -p 0 - w 5 -o 8080 -u "sip.contoso.com" 

### 创建具有单个记录的 TXT 记录集

    azure network dns-record-set create myresourcegroup contoso.com "test-TXT" TXT --ttl 300

    azure network dns-record-set add-record myresourcegroup contoso.com "test-txt" TXT -x "this is a TXT record" 


## 修改现有的记录集


这是下面的示例所示︰

### 更新现有记录集中的记录

对于本示例，我们将添加另一个 IP 地址 (1.2.3.4) 到某个现有记录集 (www): 

    azure network dns-record-set add-record  myresourcegroup contoso.com  A
    -a 1.2.3.4
    info:    Executing command network dns-record-set add-record
    Record set name: www
    + Looking up the dns zone "contoso.com"
    + Looking up the DNS record set "www"
    + Updating DNS record set "www"
    data:    Id                              : /subscriptions/################################/resourceGroups/myresourcegroup/providers/Microsoft.Network/dnszones/contoso.com/a/www
    data:    Name                            : www  
    data:    Type                            : Microsoft.Network/dnszones/a
    data:    Location                        : global
    data:    TTL                             : 4
    data:    A records:
    data:        IPv4 address                : 192.168.1.1
    data:        IPv4 address                : 1.2.3.4
    data:
    info:    network dns-record-set add-record command OK


您将使用`azure network dns-record-set delete-record`若要从记录集中删除现有值︰
 
    azure network dns-record-set delete-record myresourcegroup contoso.com www A -a 1.2.3.4
    info:    Executing command network dns-record-set delete-record
    + Looking up the DNS record set "www"
    Delete DNS record? [y/n] y
    + Updating DNS record set "www"
    data:    Id                              : /subscriptions/################################/resourceGroups/myresourcegroup/providers/Microsoft.Network/dnszones/contoso.com/A/www
    data:    Name                            : www
    data:    Type                            : Microsoft.Network/dnszones/A
    data:    Location                        : global
    data:    TTL                             : 4
    data:    A records:
    data:        IPv4 address                : 192.168.1.1
    data:
    info:    network dns-record-set delete-record command OK



## 从现有的记录集中删除记录

记录可从记录集使用`azure network dns-record-set delete-record`被删除的记录，必须在所有参数中精确匹配与现有记录，请注意。

从记录集中删除的最后一个记录不会删除该记录集。  有关详细信息，请参阅下面的[删除的记录集](#delete-a-record-set)。


    azure network dns-record-set delete-record myresourcegroup contoso.com www A -a 192.168.1.1

    azure network dns-record-set delete myresourcegroup contoso.com www A

### 从记录集中删除 AAAA 记录

    azure network dns-record-set delete-record myresourcegroup contoso.com test-aaaa  AAAA -b "2607:f8b0:4009:1803::1005"

### 从记录集中删除的 CNAME 记录

    azure network dns-record-set delete-record myresourcegroup contoso.com test-cname CNAME -c www.contoso.com
    

### 从记录集中删除的 MX 记录

    azure network dns-record-set delete-record myresourcegroup contoso.com "@" MX -e "mail.contoso.com" -f 5

### 从记录集中删除的 NS 记录
    
    azure network dns-record-set delete-record myresourcegroup contoso.com  "test-ns" NS -d "ns1.contoso.com"

### 从记录集中删除 SRV 记录

    azure network dns-record-set delete-record myresourcegroup contoso.com  "_sip._tls" SRV -p 0 -w 5 -o 8080 -u "sip.contoso.com" 

### 从记录集中删除 TXT 记录

    azure network dns-record-set delete-record myresourcegroup contoso.com  "test-TXT" TXT -x "this is a TXT record"

## 删除某一记录集
可以使用删除 AzureDnsRecordSet cmdlet 删除记录集。

>[AZURE.NOTE] 您不能删除 SOA 和 NS 记录设置区域 apex (名称 = ' @') 创建该区域时自动创建。  他们将在删除区域时自动删除。

在下面的示例中，将从 contoso.com DNS 区域中删除一个记录集，"测试 a":

    azure network dns-record-set delete myresourcegroup contoso.com  "test-a" A 

可选的-q 开关可用于取消确认提示。


##请参见

[开始创建记录集和记录](dns-getstarted-create-recordset-cli.md)<BR>
[在 DNS 区域上执行操作](dns-operations-dnszones-cli.md)<BR>
[自动化操作使用.NET SDK](dns-sdk.md)
 

测试
