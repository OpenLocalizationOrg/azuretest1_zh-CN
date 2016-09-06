---
ms.openlocfilehash: 1d663439a948c35fcea9af7483a29f0a78cae060
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="安全地连接到后端资源的应用程序服务环境中" 
    description="了解如何安全地连接到后端资源的应用程序服务环境中。" 
    services="app-service\web" 
    documentationCenter="" 
    authors="ccompy" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="06/30/2015" 
    ms.author="stefsh"/>    

# 安全地连接到后端资源的应用程序服务环境中 #

## 概述 ##
[Virtualnetwork]区域的[虚拟网络]的子网中始终创建一个应用程序的服务环境，因为应用程序服务环境中的其他后端资源的出站连接可以流动以独占方式通过虚拟网络。  

例如，可能会在群集中使用端口 1433 锁定的虚拟机上运行的 SQL Server。  终结点可能 ACLd 只允许相同的虚拟网络上的来自其他资源的访问。  

举一个例子，敏感的终结点可能运行内部和通过[到网站][SiteToSite]或[Azure ExpressRoute][ExpressRoute]连接连接到 Azure。  因此，只有 ExpressRoute 隧道的站点到站点连接的虚拟网络中的资源将能够访问本地终结点。

对于所有这些情况下，在应用程序服务环境上运行的应用程序将能够安全地连接到不同的服务器和资源。  从相同的虚拟网络中的专用端点的应用程序服务环境中运行的应用程序的出站通信或连接到相同的虚拟网络，通过虚拟网络将只流。  出站通信到专用的终结点不会在公共 Internet 上。

## 出站连接和 DNS 要求 ##
请注意，对于正常的应用程序服务环境，它要求对 Azure 存储 Sql 数据库在同一个 Azure 地区的出站访问权限。  如果出站 Internet 访问被阻止在虚拟的网络中，应用程序服务的环境不能访问这些 Azure 的终结点。

客户还可以自定义在虚拟的网络中配置的 DNS 服务器。  应用程序服务环境需要能够解决下的 Azure 终结点*。 database.windows.net， *。 file.core.windows.net 和 *。 blob.core.windows.net。  

此外建议在虚拟网络上的任何自定义 DNS 服务器是安装程序创建一个应用程序服务环境之前提前。  如果虚拟网络的 DNS 配置更改时要创建的应用程序服务环境，这将导致应用程序服务环境创建过程失败。

## 连接到 SQL Server
常见的 SQL Server 配置已在端口 1433年上侦听终结点︰

![SQL Server 终结点][SqlServerEndpoint]

有两种方法限制到此终结点的通信︰


- [网络访问控制列表][NetworkAccessControlLists](网络 Acl)

- [网络安全组][NetworkSecurityGroups]


## 限制与网络 ACL 的访问

可以使用网络访问控制列表保护端口 1433年。  白名单客户端下面的示例中解决源自于在虚拟的网络，并阻止对其他所有客户端的访问。

![网络访问控制列表示例][NetworkAccessControlListExample]

在同一个虚拟网络中为 SQL Server 应用程序服务环境中运行的任何应用程序将能够连接到 SQL Server 实例的 SQL Server 虚拟机使用的**VNet 内部**IP 地址。  

下面的示例连接字符串引用 SQL Server 使用专用 IP 地址。

    Server=tcp:10.0.1.6;Database=MyDatabase;User ID=MyUser;Password=PasswordHere;provider=System.Data.SqlClient

尽管虚拟机具有一个公共的端点，将由于网络 ACL 拒绝使用的公用 IP 地址的连接尝试。 

## 限制访问与网络安全组
保护访问另一种方法是通过网络安全组。  网络安全组可以应用到单个虚拟机，或包含虚拟机的子网。

第一次网络安全组将需要创建︰

    New-AzureNetworkSecurityGroup -Name "testNSGexample" -Location "South Central US" -Label "Example network security group for an app service environment"

限制访问 VNet 内部通信是通过网络安全组非常简单。  网络安全组中的默认规则只允许从相同的虚拟网络中其他网络的客户端的访问。

因此锁定访问 SQL Server 是简单，只需将具有其默认规则的网络安全组应用于用来运行 SQL Server 或子网包含虚拟机的虚拟机。

下面的示例将应用于包含子网的网络安全组︰

    Get-AzureNetworkSecurityGroup -Name "testNSGExample" | Set-AzureNetworkSecurityGroupToSubnet -VirtualNetworkName 'testVNet' -SubnetName 'Subnet-1'
    
最终结果是一组拦截的外部访问，同时允许 VNet 内部访问的安全规则︰

![默认网络安全规则][DefaultNetworkSecurityRules]


## 入门教程

要开始使用应用程序服务的环境，请参阅[应用程序服务环境简介][IntroToAppServiceEnvironment]

控制入站的流量到您的应用程序服务环境周围的详细信息，请参见[控制对应用程序服务环境的入站的通信][ControlInboundASE]

关于 Azure 应用程序服务平台的详细信息，请参阅[Azure 应用程序服务][AzureAppService]。

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]
 

<!-- LINKS -->
[virtualnetwork]: https://azure.microsoft.com/documentation/articles/virtual-networks-faq/
[ControlInboundTraffic]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-control-inbound-traffic/
[SiteToSite]: https://azure.microsoft.com/documentation/articles/vpn-gateway-site-to-site-create/
[ExpressRoute]: http://azure.microsoft.com/services/expressroute/
[NetworkAccessControlLists]: https://azure.microsoft.com/documentation/articles/virtual-networks-acl/
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[IntroToAppServiceEnvironment]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/ 
[ControlInboundASE]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-control-inbound-traffic/ 

<!-- IMAGES -->
[SqlServerEndpoint]: ./media/app-service-app-service-environment-securely-connecting-to-backend-resources/SqlServerEndpoint01.png
[NetworkAccessControlListExample]: ./media/app-service-app-service-environment-securely-connecting-to-backend-resources/NetworkAcl01.png
[DefaultNetworkSecurityRules]: ./media/app-service-app-service-environment-securely-connecting-to-backend-resources/DefaultNetworkSecurityRules01.png 

测试
