---
ms.openlocfilehash: 0612e3533b6779dc8173274ee841912b4fa66ee1
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="创建自定义的 DNS 记录的 Web 应用程序 |Microsoft Azure  " 
   description="如何创建 Web 应用程序使用 Azure DNS 的 DNS 记录的自定义的域。 分步操作来验证使用 CNAME 或 A 记录您域所有权" 
   services="dns" 
   documentationCenter="na" 
   authors="joaoma" 
   manager="carolz" 
   editor=""/>

<tags
   ms.service="dns"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services" 
   ms.date="08/23/2015"
   ms.author="joaoma"/>

# 为自定义的域中创建 Web 应用程序的 DNS 记录

您可以使用 Azure DNS 承载的 web 应用程序自定义的域。 例如，假设您正在创建的 Azure 的 web 应用程序，并希望您的用户访问它通过以下任一方式使用 contoso.com 或 www.contoso.com 作为 FQDN。 在这种情况下，必须创建两个记录︰ 指向 contoso.com，根的记录和 www 名称，请指向 A 记录的 CNAME 记录。 

> [AZURE.NOTE] 请记住，如果您在 Azure 创建 web 应用程序的 A 记录，A 记录必须手动更新如果底层 IP 地址的 web 应用程序更改。

您可以为您自定义的域中创建记录之前，必须在 Azure DNS 创建 DNS 区域并在注册到 Azure DNS 委派区域。 若要创建 DNS 区域，请按照本文[开始使用 Azure DNS](../dns-getstarted-create-dnszone/#Create-a-DNS-zone)。 若要委派到 Azure DNS DNS，请按照[到 Azure DNS 委派的域](../dns-domain-delegation)。
 
## 创建 A 记录为您自定义的域

A 记录用于将一个名称映射到其 IP 地址。 下面的示例中我们将将作为分配到 IPv4 地址的 A 记录︰

### 第 1 步
 
创建 A 记录和分配给变量 $rs
    
    PS C:\>$rs=New-AzureDnsRecordSet -Name "@" -RecordType "A" -ZoneName "contoso.com" -ResourceGroupName "MyAzureResourceGroup" -Ttl 600 

### 第 2 步

添加以前创建的记录集的 IPv4 值"@"使用 $rs 变量分配。 分配的 IPv4 值将是您的 web 应用程序的 IP 地址。

> [AZURE.NOTE] 要查找 web 应用程序的 IP 地址，请按照中[配置自定义域名在 Azure 应用程序服务](../web-sites-custom-domain-name/#Find-the-virtual-IP-address)的步骤

    PS C:\> Add-AzureDnsRecordConfig -RecordSet $rs -Ipv4Address <your web app IP address>

### 第 3 步

将更改提交到记录集。 使用设置 AzureDnsRecordSet 的更改上载到记录到 Azure DNS 设置︰

    Set-AzureDnsRecordSet -RecordSet $rs

## 为其创建 CNAME 记录您自定义的域

假设您的域已由 Azure DNS （请参阅[DNS 域委派](../dns-domain-delegation)），您可以使用以下示例来创建 contoso.azurewebsites.net 的 CNAME 记录︰

### 第 1 步

打开 powershell 和创建新的 CNAME 记录集指定给变量 $rs:

    PS C:\> $rs = New-AzureDnsRecordSet -ZoneName contoso.com -ResourceGroupName myresourcegroup -Name "www" -RecordType "CNAME" -Ttl 600
 
    Name              : www
    ZoneName          : contoso.com
    ResourceGroupName : myresourcegroup
    Ttl               : 600
    Etag              : 8baceeb9-4c2c-4608-a22c-229923ee1856
    RecordType        : CNAME
    Records           : {}
    Tags              : {}

这将创建具有"生存时间"的记录集类型 CNAME 600 秒的名为"contoso.com"DNS 区域中。

### 第 2 步

创建 CNAME 记录集后，您需要创建一个别名值将指向 web 应用程序。 

使用"$rs"以前分配的变量可用于以下 PowerShell 命令创建 web 应用程序 contoso.azurewebsites.net 的别名。

    PS C:\> Add-AzureDnsRecordConfig -RecordSet $rs -Cname "contoso.azurewebsites.net"
 
    Name              : www
    ZoneName          : contoso.com
    ResourceGroupName : myresourcegroup
    Ttl               : 600
    Etag              : 8baceeb9-4c2c-4608-a22c-229923ee185
    RecordType        : CNAME
    Records           : {contoso.azurewebsites.net}
    Tags              : {}

### 第 3 步

提交使用一组 AzureDnsRecordSet cmdlet 的更改︰

    PS C:\>Set-AzureDnsRecordSet -RecordSet $rs

您可以验证该记录已被正确创建通过查询使用 nslookup，"www.contoso.com"，如下所示︰

    PS C:\> nslookup
    Default Server:  Default
    Address:  192.168.0.1
 
    > www.contoso.com
    Server:  default server
    Address:  192.168.0.1
     
    Non-authoritative answer:
    Name:    <instance of web app service>.cloudapp.net
    Address:  <ip of web app service>
    Aliases:  www.contoso.com
    contoso.azurewebsites.net
    <instance of web app service>.vip.azurewebsites.windows.net

## 创建 Web 应用程序 （仅记录） 的 awverify 记录

如果您决定为您的 web 应用程序中使用 A 记录，必须经过验证流程，以允许 Azure 来确保您自己的自定义的域。 此验证步骤是通过创建名为"awverify"的特殊 CNAME 记录。

在下面的示例中，将为 contoso.com 以验证为自定义的域拥有创建的"awverify"记录︰

### 第 1 步

    PS C:\> $rs = New-AzureDnsRecordSet -ZoneName contoso.com -ResourceGroupName myresourcegroup -Name "awverify" -RecordType "CNAME" -Ttl 600
 
    Name              : awverify
    ZoneName          : contoso.com
    ResourceGroupName : myresourcegroup
    Ttl               : 600
    Etag              : 8baceeb9-4c2c-4608-a22c-229923ee1856
    RecordType        : CNAME
    Records           : {}
    Tags              : {}


### 第 2 步

创建记录集 awverify 后，您必须将别名设置为 awverify.contoso.azurewebsites.net，CNAME 记录分配以下命令中所示︰ 

    PS C:\> Add-AzureDnsRecordConfig -RecordSet $rs -Cname "awverify.contoso.azurewebsites.net"
 
    Name              : awverify
    ZoneName          : contoso.com
    ResourceGroupName : myresourcegroup
    Ttl               : 600
    Etag              : 8baceeb9-4c2c-4608-a22c-229923ee185
    RecordType        : CNAME
    Records           : {awverify.contoso.azurewebsites.net}
    Tags              : {}

### 第 3 步

使用一组 AzureDnsRecordSet cmdlet 将更改提交。 以下命令中所示︰

    PS C:\>Set-AzureDnsRecordSet -RecordSet $rs

现在，您可以继续执行[配置应用程序服务的自定义域名称](../web-sites-custom-domain-name)来配置您的 web 应用程序使用自定义的域中的步骤。

## 请参见

[管理 DNS 区域](../dns-operations-dnszones)

[管理 DNS 记录](../dns-operations-recordsets)

[通讯管理器概述](../traffic-manager-overview)

[自动化与.NET SDK 的 Azure 操作](../dns-sdk)


 
测试
