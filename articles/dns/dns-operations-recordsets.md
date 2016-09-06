---
ms.openlocfilehash: c1e6ec9a3e9febceb79bdcc57b089ca89ceb315a
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
   ms.date="08/02/2015"
   ms.author="joaoma"/>

# 如何管理 DNS 记录


> [AZURE.SELECTOR]
- [Azure CLI](dns-operations-recordsets-cli.md)
- [Azure Powershell](dns-operations-recordsets.md)


本指南将显示如何管理记录集和记录的 DNS 区域。

请务必了解 DNS 记录集和单个 DNS 记录之间的差别。  记录集是具有相同名称和相同类型的区域中的记录的集合。  有关详细信息，请参阅[了解记录集和记录](../dns-getstarted-create-recordset#Understanding-record-sets-and-records)。

## 创建记录集

使用 New AzureDnsRecordSet cmdlet 创建记录集。  您需要指定记录集名称、 区域、--生存时间 (TTL) 和记录类型。

>[AZURE.NOTE] 记录集名称必须是一个相对的名称，不包括区域名称。  例如，区域 'contoso.com' 中的记录集名称 www 将创建记录集的完全限定的名称 www.contoso.com。

>对于在区域 apex 设置记录，请使用"@"为该记录集的名称，包括引号。  记录集的完全限定名称就等于区域名称，在此种情况下"contoso.com"。

Azure DNS 支持下列记录类型︰ A、 AAAA、 CNAME、 MX，NS、 SOA、 SRV、 TXT。  与每个区域自动创建 SOA 类型的记录集，它们无法单独制定。

    PS C:\> $rs = New-AzureDnsRecordSet -Name www -Zone $zone -RecordType A -Ttl 300 [-Tag $tags] [-Overwrite] [-Force]

如果记录集已存在，该命令将失败，除非-覆盖开关使用。  -覆盖选项将触发确认提示，它可以使用抑制-Force 开关。

在上面的示例中，指定区域使用区域对象，返回的 Get AzureDnsZone 或新建 AzureDnsZone。 或者，您还可以指定区域的区域名称和资源组的名称︰

    PS C:\> $rs = New-AzureDnsRecordSet -Name www –ZoneName contoso.com –ResourceGroupName MyAzureResourceGroup -RecordType A -Ttl 300 [-Tag $tags] [-Overwrite] [-Force]

新 AzureDnsRecordSet 返回本地的对象，该对象代表在 Azure DNS 中所创建的记录集。

>[AZURE.NOTE] CNAME 记录集与其他具有相同名称的记录集不能共存。  例如，不能创建具有相对名称 www CNAME 和 A 记录具有相对名称 www 在同一时间。  由于区域 apex (名称 = ' @') 始终包含 NS 和 SOA 记录集创建创建该区域时，这意味着您不能创建在区域 apex 设置 CNAME 记录。  这些约束会产生 DNS 标准，它们不是 Azure DNS 的限制。

### 通配符的记录
Azure DNS 支持[通配符的记录](https://en.wikipedia.org/wiki/Wildcard_DNS_record)。  （除非有更匹配从非通配符记录集） 的任何具有匹配名称的查询返回。

>[AZURE.NOTE] 若要创建通配符的记录集，请使用记录集名称"\*"，或名称的第一个标签是"\*"，如"\*.foo"。

>除 NS 和 SOA 的所有记录类型支持通配符的记录集。  

## 获取记录集
若要检索现有的记录集，请使用 Get-AzureDnsRecordSet，指定该记录集相对名称、 记录类型，以及该区域︰

    PS C:\> $rs = Get-AzureDnsRecordSet -Name www –RecordType A -Zone $zone

与新建的 AzureDnsRecordSet 记录名称必须为相对名称，即排除区域名称。  可以使用 （如上所述） 的区域对象指定该区域或区域名称和资源组的名称︰

    PS C:\> $rs = Get-AzureDnsRecordSet –Name www –RecordType A -Zonename contoso.com -ResourceGroupName MyAzureResourceGroup

获得 AzureDnsRecordSet 返回本地的对象，该对象代表在 Azure DNS 中所创建的记录集。

## 列表中的记录集
通过省略 – 名称和/或-RecordType 参数，获取 AzureDnsRecordSet 也可向列表中的记录集︰

### 选项 1 
列出所有的记录集。  这将返回所有的记录集，而不考虑名称或记录类型︰

    PS C:\> $list = Get-AzureDnsRecordSet -Zone $zone
### 选项 2 

列出给定的记录类型的记录集。  这将返回所有匹配给定的记录类型 （在本例中为记录） 的记录集︰

    PS C:\> $list = Get-AzureDnsRecordSet –RecordType A -Zone $zone 

上述两种情况下，该区域可以使用指定任一区域对象 （如所示） 或通过指定的 ZoneName-和-ResourceGroupName 参数。

## 将记录添加到记录集
记录被添加到使用添加 AzureDnsRecordConfig cmdlet 的记录集。 这是一个离线操作 — — 仅表示记录集的本地对象发生更改。

将记录添加到记录集的参数的记录集的类型而异。 例如，使用类型的记录集时 'A' 您才能够使用 IPv4Address 参数中指定的记录。

可以将其他记录添加到还要添加 AzureDnsRecordConfig 调用来设置每个记录。  对任何记录集，您可以添加最多 100 个记录。  但是，CNAME 类型的记录集可以包含最多为一条记录，并且记录集不能包含两个相同的记录。  空记录集 （具有零记录） 可以创建，但不是会出现在 Azure DNS 名称服务器。

一旦该记录集包含所需的记录的集合，它需要被提交使用一组 AzureDnsRecordSet cmdlet，替换现有记录的记录集提供在 Azure DNS 设置。
下面的示例演示如何创建包含一条记录的每个记录类型的记录集。

### 创建与单个记录集的记录

    PS C:\> $rs = New-AzureDnsRecordSet -Name "test-a" -RecordType A -Zone $zone -Ttl 60
    PS C:\> Add-AzureDnsRecordConfig -RecordSet $rs -Ipv4Address "1.2.3.4"
    PS C:\> Set-AzureDnsRecordSet -RecordSet $rs

创建记录的操作的顺序可以也被输送，通过记录集对象使用管道，而不是作为参数。  例如︰

    PS C:\> New-AzureDnsRecordSet -Name "test-a" -RecordType A -Zone $zone -Ttl 60 | Add-AzureDnsRecordConfig -Ipv4Address "1.2.3.4" | Set-AzureDnsRecordSet

### 创建具有单个记录的 AAAA 记录集

    PS C:\> $rs = New-AzureDnsRecordSet -Name "test-aaaa" -RecordType AAAA -Zone $zone -Ttl 60
    PS C:\> Add-AzureDnsRecordConfig -RecordSet $rs -Ipv6Address "2607:f8b0:4009:1803::1005"
    PS C:\> Set-AzureDnsRecordSet -RecordSet $rs

### 使用单个记录创建 CNAME 记录集

    PS C:\> $rs = New-AzureDnsRecordSet -Name "test-cname" -RecordType CNAME -Zone $zone -Ttl 60
    PS C:\> Add-AzureDnsRecordConfig -RecordSet $rs -Cname "www.contoso.com"
    PS C:\> Set-AzureDnsRecordSet -RecordSet $rs

### 创建具有单个记录的 MX 记录集
在此示例中，我们使用记录集名称"@"区域 apex (例如"contoso.com") 在创建 MX 记录。  这是常见的 MX 记录。

    PS C:\> $rs = New-AzureDnsRecordSet -Name "@" -RecordType MX -Zone $zone -Ttl 60
    PS C:\> Add-AzureDnsRecordConfig -RecordSet $rs -Exchange "mail.contoso.com" -Preference 5
    PS C:\> Set-AzureDnsRecordSet -RecordSet $rs

### 使用单个记录创建 NS 记录集

    PS C:\> $rs = New-AzureDnsRecordSet -Name "test-ns" -RecordType NS -Zone $zone -Ttl 60
    PS C:\> Add-AzureDnsRecordConfig -RecordSet $rs -Nsdname "ns1.contoso.com"
    PS C:\> Set-AzureDnsRecordSet -RecordSet $rs
### 使用单个记录创建 SRV 记录集

如果在区域的根目录中创建一条 SRV 记录，只需指定 _service 和 _protocol 中记录名称 — — 没有必要，也包括。 @ 中记录名称

    PS C:\> $rs = New-AzureDnsRecordSet -Name "_sip._tls" -RecordType SRV -Zone $zone -Ttl 60
    PS C:\> Add-AzureDnsRecordConfig -RecordSet $rs –Priority 0 –Weight 5 –Port 8080 –Target "sip.contoso.com"
    PS C:\> Set-AzureDnsRecordSet -RecordSet $rs

### 创建具有单个记录的 TXT 记录集

    PS C:\> $rs = New-AzureDnsRecordSet -Name "test-txt" -RecordType TXT -Zone $zone -Ttl 60
    PS C:\> Add-AzureDnsRecordConfig -RecordSet $rs -Value "This is a TXT record"
    PS C:\> Set-AzureDnsRecordSet -RecordSet $rs

## 修改现有的记录集
修改现有的记录集创建记录到遵循类似的模式。  操作的顺序如下所示︰

1.  检索现有记录集使用 Get AzureDnsRecordSet。
2.  修改记录集，请通过添加记录、 删除记录，更改记录参数或更改该记录设置 TTL。  这些更改将关闭行 — 仅表示记录集的本地对象发生更改。
3.  提交更改，使用一组 AzureDnsRecordSet cmdlet。  这将替换现有记录的记录集提供在 Azure DNS 设置。

这是下面的示例所示︰

### 更新现有记录集中的记录
为了使本示例，我们将更改现有记录的 IP 地址︰

    PS C:\> $rs = Get-AzureDnsRecordSet -name "test-a" -RecordType A -Zone $zone 
    PS C:\> $rs.Records[0].Ipv4Address = "134.170.185.46"
    PS C:\> Set-AzureDnsRecordSet -RecordSet $rs 

设置 AzureDnsRecordSet cmdlet 使用 etag 检查，以确保并发更改不会被覆盖。  使用-覆盖标志，以取消这些检查。  有关详细信息，请参阅 Etag 和标记。

### 修改 SOA 记录

>[AZURE.NOTE] 不能添加或删除在区域 apex 设置自动创建 SOA 记录的记录 (名称 = ' @')，但您可以修改在 SOA 记录中的参数和记录设置 TTL。

下面的示例演示如何更改 SOA 记录的电子邮件属性︰

    PS C:\> $rs = Get-AzureDnsRecordSet -Name "@" -RecordType SOA -Zone $zone
    PS C:\> $rs.Records[0].Email = "admin.contoso.com"
    PS C:\> Set-AzureDnsRecordSet -RecordSet $rs 

### 修改区域 apex NS 记录

>[AZURE.NOTE] 您不能添加、 删除或修改在区域 apex 设置自动创建 NS 记录中的记录 (名称 = ' @')。  允许的唯一更改是修改记录集 TTL。

下面的示例演示如何将 NS 记录集的 TTL 属性更改︰

    PS C:\> $rs = Get-AzureDnsRecordSet -Name "@" -RecordType NS -Zone $zone
    PS C:\> $rs.Ttl = 300
    PS C:\> Set-AzureDnsRecordSet -RecordSet $rs 

### 将记录添加到一个现有的记录集
在此示例中我们添加两个额外的 MX 记录与现有记录集︰

    PS C:\> $rs = Get-AzureDnsRecordSet -name "test-mx" -RecordType MX -Zone $zone
    PS C:\> Add-AzureDnsRecordConfig -RecordSet $rs -Exchange "mail2.contoso.com" -Preference 10
    PS C:\> Add-AzureDnsRecordConfig -RecordSet $rs -Exchange "mail3.contoso.com" -Preference 20
    PS C:\> Set-AzureDnsRecordSet -RecordSet $rs 

## 从现有的记录集中删除记录

可以从记录集使用删除 AzureDnsRecordConfig 删除记录。  请注意，被删除的记录必须与现有记录，精确匹配是跨所有参数。  更改必须提交使用一组 AzureDnsRecordSet。

从记录集中删除的最后一个记录不会删除该记录集。  有关详细信息，请参阅下面的[删除的记录集](#delete-a-record-set)。


    PS C:\> $rs = Get-AzureDnsRecordSet -Name "test-a" -RecordType A –Zone $zone
    PS C:\> Remove-AzureDnsRecordConfig -RecordSet $rs -Ipv4Address "1.2.3.4"
    PS C:\> Set-AzureDnsRecordSet -RecordSet $rs

序列的操作以删除该记录集对象，用管道从记录集可以也被输送，传递的记录，而不是作为参数。  例如︰

    PS C:\> Get-AzureDnsRecordSet -Name "test-a" -RecordType A -Zone $zone | Remove-AzureDnsRecordConfig -Ipv4Address "1.2.3.4" | Set-AzureDnsRecordSet
### 从记录集中删除 AAAA 记录

    PS C:\> $rs = Get-AzureDnsRecordSet -Name "test-aaaa" -RecordType AAAA –Zone $zone
    PS C:\> Remove-AzureDnsRecordConfig -RecordSet $rs -Ipv6Address "2607:f8b0:4009:1803::1005"
    PS C:\> Set-AzureDnsRecordSet -RecordSet $rs

### 从记录集中删除的 CNAME 记录

由于 CNAME 记录集可以包含至多一个记录，删除该记录会留下一个空的记录集。

    PS C:\> $rs =  Get-AzureDnsRecordSet -name "test-cname" -RecordType CNAME –Zone $zone   
    PS C:\> Remove-AzureDnsRecordConfig -RecordSet $rs -Cname "www.contoso.com"
    PS C:\> Set-AzureDnsRecordSet -RecordSet $rs

### 从记录集中删除的 MX 记录

    PS C:\> $rs = Get-AzureDnsRecordSet -name "test-mx" -RecordType 'MX' –Zone $zone    
    PS C:\> Remove-AzureDnsRecordConfig -RecordSet $rs -Exchange "mail.contoso.com" -Preference 5
    PS C:\> Set-AzureDnsRecordSet -RecordSet $rs

### 从记录集中删除的 NS 记录
    
    PS C:\> $rs = Get-AzureDnsRecordSet -Name "test-ns" -RecordType NS -Zone $zone
    PS C:\> Remove-AzureDnsRecordConfig -RecordSet $rs -Nsdname "ns1.contoso.com"
    PS C:\> Set-AzureDnsRecordSet -RecordSet $rs

### 从记录集中删除 SRV 记录

    PS C:\> $rs = Get-AzureDnsRecordSet -Name "_sip._tls" -RecordType SRV -Zone $zone
    PS C:\> Remove-AzureDnsRecordConfig -RecordSet $rs –Priority 0 –Weight 5 –Port 8080 –Target "sip.contoso.com"
    PS C:\> Set-AzureDnsRecordSet -RecordSet $rs

### 从记录集中删除 TXT 记录

    PS C:\> $rs = Get-AzureDnsRecordSet -Name "test-txt" -RecordType TXT -Zone $zone
    PS C:\> Remove-AzureDnsRecordConfig -RecordSet $rs -Value "This is a TXT record"
    PS C:\> Set-AzureDnsRecordSet -RecordSet $rs

## 删除某一记录集
可以使用删除 AzureDnsRecordSet cmdlet 删除记录集。

>[AZURE.NOTE] 您不能删除 SOA 和 NS 记录设置区域 apex (名称 = ' @') 创建该区域时自动创建。  他们将在删除区域时自动删除。

使用删除的记录集的以下三种方法之一︰
### 选项 1
按名称指定的所有参数︰

    PS C:\> Remove-AzureDnsRecordSet -Name "test-a" -RecordType A -Zonename "contoso.com" -ResourceGroupName MyAzureResourceGroup [-Force]
可选的-强制开关可用于取消确认提示。

### 选项 2
指定的名称和类型的记录集，由对象指定的区域︰

    PS C:\> Remove-AzureDnsRecordSet -Name "test-a" -RecordType A -Zone $zone [-Force]

### 选项 3
指定记录集对象︰

    PS C:\> Remove-AzureDnsRecordSet –RecordSet $rs [-Overwrite] [-Force]

指定记录集使用对象启用 etag 检查，以确保并发更改不会被删除。  可选的-覆盖标志禁止这些检查。 有关详细信息，请参阅[Etag 和标记](../dns-getstarted-create-dnszone#Etags-and-tags)。

记录集对象还可以输送而不是作为一个参数传递︰

    PS C:\> Get-AzureDnsRecordSet -Name "test-a" -RecordType A -Zone $zone | Remove-AzureDnsRecordSet [-Overwrite] [-Force]

##请参见

[开始创建记录集和记录](../dns-getstarted-create-recordset)<BR>
[在 DNS 区域上执行操作](../dns-operations-dnszones)<BR>
[自动化操作使用.NET SDK](../dns-sdk)
 

测试
