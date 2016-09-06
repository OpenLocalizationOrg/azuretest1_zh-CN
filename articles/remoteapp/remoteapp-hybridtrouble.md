---
ms.openlocfilehash: 2cb3e2eef844590bd61751d9d584ce32f24771af
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---

<properties 
    pageTitle="创建远程应用程序混合集合的疑难解答"
    description="了解如何解决远程应用程序混合集合创建失败" 
    services="remoteapp" 
    documentationCenter="" 
    authors="vkbucha" 
    manager="mbaldwin" />

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/12/2015" 
    ms.author="elizapo" />



# 疑难解答创建 Azure RemoteApp 混合集合

混合集合中承载并将数据存储在 Azure 云还允许用户访问数据存储在您的本地网络上的资源。 用户可以通过同步或使用 Azure Active Directory 联合其企业凭据登录访问应用程序。 您可以部署一个混合集合，使用现有的 Azure 虚拟网络，或者可以创建新的虚拟网络。 我们建议您创建或使用 CIDR 范围足够大，Azure RemoteApp 的预期未来增长的虚拟网络子网。

尚未创建您收藏？ 有关的步骤，请参阅[创建混合集合](remoteapp-create-hybrid-deployment.md)。

如果您无法创建您的集合，或者如果该集合不能正常工作的方式看法不一致，请检查出下面的信息。

## VNET 使用强制隧道的那样？ ##
远程应用程序当前不支持使用具有强制隧道启用的 VNETs。 如果您需要此功能，请联系[远程应用程序团队](mailto:remoteappforum@microsoft.com)请求支持。

您的申请被批准后，请确保子网选择 Azure RemoteApp 和子网中的虚拟机上打开下列端口。 您的子网中的虚拟机应能够访问有关网络安全组一节中提到的 Url。

出站︰ TCP: 443，TCP: 10101 10175

## 您 VNET 是否有网络安全组定义？ ##
如果您有网络子网将作为您的集合上定义的安全组，请确保以下 Url 从内部来访问您的子网︰ 

    https://management.remoteapp.windowsazure.com  
    https://opsapi.mohoro.com  
    https://telemetry.remoteapp.windowsazure.com  
    https://*.remoteapp.windowsazure.com  
    https://login.windows.net (if you have Active Directory)  
    https://login.microsoftonline.com  
    Azure storage *.remoteapp.windowsazure.com  
    *.core.windows.net  
    https://www.remoteapp.windowsazure.com  
    https://www.remoteapp.windowsazure.com  

打开下列端口上的虚拟网络子网︰

入站-TCP: 3030，TCP: 443  
出站-TCP: 443  

在严格控制的子网中部署您的 Vm 中可以添加其他网络的安全组。

## 您正在使用自己的 DNS 服务器？ 他们可以访问和从 VNET 网？ ##
混合收集您使用您自己的 DNS 服务器。 它们在您的网络配置架构或指定通过管理门户创建虚拟网络时。 使用 DNS 服务器故障转移的方式 （而不是循环复用） 指定它们的顺序。  

请确保您收藏的 DNS 服务器都可以访问，并且此集合指定 VNET 子网中可用。

例如︰

    <VirtualNetworkConfiguration>
    <Dns>
      <DnsServers>
        <DnsServer name="" IPAddress=""/>
      </DnsServers>
    </Dns>
    </VirtualNetworkConfiguration>

![定义您的 DNS](./media/remoteapp-hybridtrouble/dnsvpn.png)

## 您是否在集合中使用 Active Directory 域控制器？ ##
当前只有一个 Active Directory 域可以与 Azure 远程应用程序相关联。 混合集合支持使用 Windows 服务器的 Active Directory 部署; 从目录同步工具同步的 Azure Active Directory 帐户具体而言，或者使用密码同步选项同步与 Active Directory 联合身份验证服务 (AD FS) 联合身份验证配置同步。 您需要创建一个自定义的域相匹配为您的内部域的 UPN 域名后缀并设置目录集成。 

有关更多信息，请参见[配置 Azure RemoteApp 的活动目录](remoteapp-ad.md)。

请确保域的详细信息提供的有效并且从 Azure 远程应用程序所使用的子网中创建虚拟机可以访问域控制器。 还要确保提供的服务帐户凭据有权限将计算机添加到提供域和提供 AD 名称，可以从 VNET 中提供的 DNS 解析。

## 创建集合时未指定域的名称？ ##

创建或添加的域名必须是内部域名 （非 Azure AD 域名），并且必须是可解析 DNS 格式 (contoso.local)。 例如，您有一个 Active Directory 内部名称 (contoso.local) 和活动目录 UPN (contoso.com)-您需要在创建集合时使用的内部名称。
