---
ms.openlocfilehash: d7eee7979938c6bf4ee6f39cc603fa672187ac37
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="自动执行 DNS 和记录设置使用.net SDK 操作 |Microsoft Azure" 
   description=" 使用.NET SDK 来自动化 Azure DNS 的所有 DNS 操作。 " 
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
   ms.date="08/20/2015"
   ms.author="joaoma"/>
# 创建 DNS 区域和使用.NET SDK 的记录集
您可以自动化操作，以创建、 删除或更新 DNS 区域，记录集和记录 DNS SDK 中使用.NET DNS 管理库。 完整的 Visual Studio 项目可[这里。](http://download.microsoft.com/download/2/A/C/2AC64449-1747-49E9-B875-C71827890126/AzureDnsSDKExample_2015_05_05.zip)

## NuGet 程序包和 Namespace 声明
若要使用 DNS 客户端，就需要安装"Azure DNS 管理库"NuGet 程序包，并将 DNS 管理命名空间添加到您的项目。 转到 Visual Studio 中打开项目或新项目，转到工具，Nuget 程序包管理器控制台。 下载的 Azure DNS 管理库︰

    using Microsoft.Azure;
    using Microsoft.Azure.Management.Dns;
    using Microsoft.Azure.Management.Dns.Models;

## 正在初始化 DNS 管理客户端

DnsManagementClient 包含的方法和属性所需的管理 DNS 区域和记录集。  为了使客户端能够访问您的订阅就需要设置正确的权限，并生成 AWT 令牌，请参阅"Authenticating Azure 资源管理器的请求"的更多详细信息。

    // get a token for the AAD application (see linked article for code)
    string jwt = GetAToken();

    // make the TokenCloudCredentials using subscription ID and token
    TokenCloudCredentials tcCreds = new TokenCloudCredentials(subID, jwt);

    // make the DNS management client
    DnsManagementClient dnsClient = new DnsManagementClient(tcCreds);

## 创建或更新 DNS 区域

若要创建 DNS 区域，一个区域对象创建并传递给 dnsClient.Zones.CreateOrUpdate。  为 DNS 区域未链接到某一特定的区域，位置将被设置为"全局"。<BR>

创建 DNS 区域︰

    // create a DNS zone
    Zone z = new Zone("global");
    z.Properties = new ZoneProperties();
    z.Tags.Add("dept", "shopping");
    z.Tags.Add("env", "production");
    ZoneCreateOrUpdateParameters zoneParams = new ZoneCreateOrUpdateParameters(z);
    ZoneCreateOrUpdateResponse responseCreateZone = 
    dnsClient.Zones.CreateOrUpdate("myresgroup", "myzone.com", zoneParams);


Azure DNS 支持开放式并发调用[Etag](dns-getstarted-create-dnszone.md#Etags-and-tags) Etag 是该区域的属性，并且 IfNoneMatch 是 ZoneCreateOrUpdateParameters 中的属性。

## 创建或更新 DNS 记录
记录集作为管理 DNS 记录。  记录集是一套具有相同名称和区域中的记录类型的记录。  若要创建或更新记录集时，记录集对象是创建并传递给 dnsClient.RecordSets.CreateOrUpdate。  请注意，该记录集名称相对于区域名称而不是被完全限定的 DNS 名称。  再次的位置被设置为"全局"。
    
使某些记录集

    // make some records sets
    RecordSet rsWwwA = new RecordSet("global");
    rsWwwA.Properties = new RecordProperties(3600);
    rsWwwA.Properties.ARecords = new List<ARecord>();
    rsWwwA.Properties.ARecords.Add(new ARecord("1.2.3.4"));
    rsWwwA.Properties.ARecords.Add(new ARecord("1.2.3.5"));
    RecordCreateOrUpdateParameters recordParams = 
                                new RecordCreateOrUpdateParameters(rsWwwA);
    RecordCreateOrUpdateResponse responseCreateA = 
                                dnsClient.RecordSets.CreateOrUpdate("myresgroup", 
    "myzone.com", "www", RecordType.A, recordParams);
    
    
Azure DNS 支持开放式并发[Etag](dns-getstarted-create-dnszone.md#Etags-and-tags)。  Etag 是记录集和 IfNoneMatch 的属性是 RecordSetCreateOrUpdateParameters 中的属性。

## 获取区域和记录集
区域和记录集集合提供了获取区域，并分别记录集的能力。  记录集由其类型名称和该区域 （和资源组） 存在于标识。  区域进行标识的名称和它们存在于资源组。

    ZoneGetResponse getZoneResponse = 
    dnsClient.Zones.Get("myresgroup", "myzone.com");
    RecordGetResponse getRSResp = 
    dnsClient.RecordSets.Get("myresgroup", 
    "myzone.com", "www", RecordType.A);

##列表区域和记录集

以列表区域中，使用列表中的区域集合。  若要列出记录集记录集集合中使用的列表或 ListAll 方法。  列表方法与 ListAll 方法不同，因为它只返回指定类型的记录集。

下面的示例演示如何获取 DNS 区域的列表，记录集︰


    ZoneListResponse zoneListResponse = dnsClient.Zones.List("myresgroup", new ZoneListParameters());
    foreach (Zone zone in zoneListResponse.Zones)
    {
        RecordListResponse recordSets = 
                            dnsClient.RecordSets.ListAll("myresgroup", "myzone.com", new RecordSetListParameters());

    // do something like write out each record set
    }
## 下一步行动

[通信管理器是什么？](traffic-manager-overview.md)

[什么是 Azure DNS？](dns-overview.md)

[Visual Studio SDK 示例项目](http://download.microsoft.com/download/2/A/C/2AC64449-1747-49E9-B875-C71827890126/AzureDnsSDKExample_2015_05_05.zip)测试
