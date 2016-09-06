---
ms.openlocfilehash: 43debdc3e26522196e92c608d61ffcdf18a3d0ad
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="开始使用 Azure DNS |Microsoft Azure"
   description="了解如何为 Azure DNS 创建 DNS 区域。这是循序渐进以获取第一个 DNS 区域创建启动承载您的 DNS 域。"
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
   ms.date="09/02/2015"
   ms.author="joaoma"/>

# 开始使用 Azure DNS


> [AZURE.SELECTOR]
- [Azure CLI](dns-getstarted-create-dnszone-cli.md)
- [Azure Powershell 步骤](dns-getstarted-create-dnszone.md)

域 'contoso.com' 可能包含大量的 DNS 记录，如 mail.contoso.com （用于邮件服务器） 和 www.contoso.com （对于 web 站点）。  DNS 区域用于承载特定域的 DNS 记录。<BR><BR>
启动承载您的域，需要首先创建 DNS 区域。 为特定的域创建任何 DNS 记录将会在 DNS 区域的域内。<BR><BR>
这些说明使用 Microsoft Azure PowerShell。  请务必更新到最新的 Azure PowerShell，使用 Azure DNS cmdlet。 也可以使用 Microsoft Azure 命令行界面，REST API 或 SDK 执行相同的步骤。<BR><BR>

## 设置 Azure DNS PowerShell

以下步骤需要您可以管理 Azure DNS 使用 Azure PowerShell 之前完成。

### 第 1 步
 Azure 的 DNS 使用 Azure 资源管理器 (ARM)。 请确保您切换使用 ARM cmdlet PowerShell 模式。 [使用 Windows Powershell 与资源管理器](powershell-azure-resource-manager.md)提供了更多的信息。<BR><BR>

        PS C:\> Switch-AzureMode -Name AzureResourceManager

### 第 2 步
 登录到您的 Azure 帐户。<BR><BR>

        PS C:\> Add-AzureAccount

您将被提示到身份验证使用您的凭据。<BR>

### 第 3 步
选择您要使用的 Azure 订阅。 <BR>


        PS C:\> Select-AzureSubscription -SubscriptionName "MySubscription"

若要查看可用的订阅列表，使用 Get AzureSubscription cmdlet。<BR>

### 第 4 步
创建新的资源组 （跳过此步骤如果使用现有资源组）<BR>

        PS C:\> New-AzureResourceGroup -Name MyAzureResourceGroup -location "West US"


Azure 的资源管理器需要的所有资源组都指定一个位置。 这是该资源组中的资源用作默认位置。 但是，由于所有 DNS 资源全局、 没有区域，资源组位置的选择对 Azure DNS 没有任何影响。<BR>

### 第 5 步

由 Microsoft.Network 资源提供程序管理 Azure DNS 服务。 Azure 订阅需要注册才能使用 Azure DNS 使用此资源提供程序。 这是一个操作为每个订阅的时间。

    PS c:\> Register-AzureProvider -ProviderNamespace Microsoft.Network



## Etag 和标记
### Etag
假设两个人或两个进程试图同时修改 DNS 记录。  哪一次为准？  但获胜者知道它们已经不仅仅是覆盖由其他人创建的更改？<BR><BR>
Azure 的 DNS 使用 Etag 来安全地处理对同一资源的并发更改。 （区域或记录集） 的每个 DNS 资源具有与之关联的 Etag。  每当检索资源时，还会检索其 Etag。  当更新资源，您可以选择要传递回 Etag 因此 Azure DNS 可验证的 Etag 的服务器匹配。  由于对资源的每次更新会导致重新生成 Etag，Etag 匹配指示发生了并发更改。  Etag 还使用时创建新的资源，确保资源不已经存在。

默认情况下，Azure DNS PowerShell 使用 Etag 可以阻止并发更改区域和记录集。  可选的-覆盖开关可用于取消 Etag 检查，这种情况下所发生的任何并发更改将被覆盖。<BR><BR>
在 Azure DNS REST API 级别，使用 HTTP 标头指定 Etag。  下表给出了它们的行为︰

|页眉|行为|
|------|--------|
|无|将始终成功 （没有 Etag 检查）|
|如果匹配 <etag>|PUT 只会成功如果资源存在并 Etag 匹配|
|如果匹配 * |如果资源存在 PUT 只会成功|
|如果没有-匹配 |* PUT 只会成功如果资源不存在|

### Tags
标记是不同的 Etag。 标记的名称-值对列表，并且使用 Azure 资源管理器标签的资源进行计费或分组。 有关标记的详细信息请参阅使用标签来组织您的 Azure 资源。
Azure DNS PowerShell 支持区域和使用选项指定的记录集上标记的标记参数。  下面的示例演示如何使用两个标记，创建 DNS 区域项目 = 演示和环境 = 测试:

    PS C:\> New-AzureDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup -Tag @( @{ Name="project"; Value="demo" }, @{ Name="env"; Value="test" } )


## 创建 DNS 区域
使用 New AzureDnsZone cmdlet 创建 DNS 区域。 在下面的示例中，我们将创建一个名为 'contoso.com' 的资源组名为 MyResourceGroup 的 DNS 区域︰<BR>

        PS C:\> New-AzureDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup

>[AZURE.NOTE] 在 Azure DNS 区域名称应指定而终止。。  例如，作为 'contoso.com' 而不是 'contoso.com。'。<BR>


现在已在 Azure DNS 创建 DNS 区域。  创建 DNS 区域还将创建以下 DNS 记录︰<BR>



- 起始授权机构 (SOA) 记录。  这是存在于每个 DNS 区域根目录。
- 授权的名称服务器 (NS) 记录。  这些显示哪些名称服务器承载的区域。  Azure 的 DNS 使用的名称服务器池，因此不同名称服务器可能会分配到 Azure DNS 中的不同区域。  [委派到 Azure DNS 域](dns-domain-delegation.md)的详细信息，请参阅。<BR>

若要查看这些记录，请使用 Get AzureDnsRecordSet:

        PS C:\> Get-AzureDnsRecordSet -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup

    Name              : @
    ZoneName          : contoso.com
    ResourceGroupName : MyResourceGroup
    Ttl               : 3600
    Etag              : 2b855de1-5c7e-4038-bfff-3a9e55b49caf
    RecordType        : SOA
    Records           : {[ns1-01.azure-dns.com,msnhst.microsoft.com,900,300,604800,300]}
    Tags              : {}

    Name              : @
    ZoneName          : contoso.com
    ResourceGroupName : MyResourceGroup
    Ttl               : 3600
    Etag              : 5fe92e48-cc76-4912-a78c-7652d362ca18
    RecordType        : NS
    Records           : {ns1-01.azure-dns.com, ns2-01.azure-dns.net, ns3-01.azure-dns.org,
                  ns4-01.azure-dns.info}
    Tags              : {}

>[AZURE.NOTE] 记录在 DNS 区域使用的根 （或 apex） 集"@"为该记录集的名称。


创建您的第一个 DNS 区域后，您可以测试它使用 DNS 如 nslookup，挖或为[解决 DnsName PowerShell cmdlet](https://technet.microsoft.com/en-us/library/jj590781.aspx)的工具。<BR>

如果尚没有委托您使用新在 Azure DNS 区域的域，您需要直接直接到您的区域的名称服务器的 DNS 查询。 您的区域的名称服务器中给出了 NS 记录，获取 AzureDnsRecordSet 上面所列 — — 请确保替换您的区域分成以下命令的正确值。<BR>

        C:\> nslookup
        > set type=SOA
        > server ns1-01.azure-dns.com
        > contoso.com

        Server: ns1-01.azure-dns.com
        Address:  208.76.47.1

        contoso.com
                primary name server = ns1-01.azure-dns.com
                responsible mail addr = msnhst.microsoft.com
                serial  = 1
                refresh = 900 (15 mins)
                retry   = 300 (5 mins)
                expire  = 604800 (7 days)
                default TTL = 300 (5 mins)


## 下一步行动


[开始创建记录集和记录](dns-getstarted-create-recordset.md)<BR>
[如何管理 DNS 区域](dns-operations-dnszones.md)<BR>
[如何管理 DNS 记录](dns-operations-recordsets.md)<BR>
[自动化与.NET SDK 的 Azure 操作](dns-sdk.md)<BR>
[Azure DNS REST API 参考](https://msdn.microsoft.com/library/azure/mt163862.aspx)
 

测试
