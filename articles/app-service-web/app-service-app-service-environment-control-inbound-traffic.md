---
ms.openlocfilehash: c3f9084e564a1c7d8c3d77096ba2591482440b98
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="如何控制对应用程序服务环境的入站的通信" 
    description="了解有关如何配置网络安全规则来控制对应用程序服务环境的入站的通信。" 
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

# 如何控制对应用程序服务环境的入站的通信

## 概述 ##
[Virtualnetwork]区域的[虚拟网络]的子网中始终创建一个应用程序的服务环境。  创建应用程序服务环境时，可以定义新区域的虚拟网络和新的子网。  或者，可以预先存在的区域虚拟网络和现有的子网中创建一个应用程序的服务环境。  在创建应用程序服务环境的更多详细信息请参阅[如何创建应用程序服务环境][HowToCreateAnAppServiceEnvironment]。

应用程序服务环境始终必须创建在一个子网，因为子网提供了可以用来锁定背后上游设备和服务的入站通信，HTTP 和 HTTPS 通信量仅接受来自上游的特定 IP 地址的网络边界。

子网上的入站和出站网络流量控制使用的[网络安全组][NetworkSecurityGroups]。  控制入站的通信，需要创建网络安全组中，网络安全规则，然后将分配网络安全组包含应用程序服务环境的子网。

一旦网络安全组分配给子网时，在应用程序服务环境中的应用程序的入站的通信被允许/阻止基于允许和拒绝网络安全组中定义的规则。

## 在应用程序服务环境中使用的网络端口 ##
锁定通过网络安全组的入站网络通信之前, 是一定要知道的一套使用的应用程序服务环境必需和可选的网络端口。  意外关闭关闭某些端口的通信可能会导致应用程序服务环境中的功能丧失。

下面是应用程序服务环境使用的端口的列表︰

- 454︰**所需端口**的 Azure 基础结构用于管理和维护的应用程序服务的环境。  不会阻止与此端口的通信。
- 455︰**所需端口**的 Azure 基础结构用于管理和维护的应用程序服务的环境。  不会阻止与此端口的通信。
- 80︰ 默认为应用程序服务计划中的应用程序服务环境中运行的应用程序的入站 HTTP 通信量的端口
- 443︰ 默认为应用程序服务计划中的应用程序服务环境中运行的应用程序的入站 SSL 通信端口
- 21: FTP 的控制通道。  如果不使用 FTP，可以放心地阻止此端口。
- 10001 10020︰ 用于 FTP 数据信道。  如与控制通道中，这些端口可以放心地阻止如果不使用 FTP (**注︰**在预览过程中可能会更改 FTP 数据信道。)
- 4016︰ 用于远程调试使用 Visual Studio 2012。  如果不使用该功能，可以放心地阻止此端口。
- 4018︰ 用于远程调试与 Visual Studio 2013年。  如果不使用该功能，可以放心地阻止此端口。
- 4020︰ 用于远程调试与 Visual Studio 2015年。  如果不使用该功能，可以放心地阻止此端口。

## 出站连接和 DNS 要求 ##
请注意，对于正常的应用程序服务环境，它还要求对 Azure 存储以及 Sql 数据库在同一个 Azure 地区出站访问。  如果出站 Internet 访问被阻止在虚拟的网络中，应用程序服务的环境不能访问这些 Azure 的终结点。

客户还可以自定义在虚拟的网络中配置的 DNS 服务器。  应用程序服务环境需要能够解决下的 Azure 终结点*。 database.windows.net， *。 file.core.windows.net 和 *。 blob.core.windows.net。  

此外建议在 vnet 上的任何自定义 DNS 服务器是安装程序创建一个应用程序服务环境之前提前。  如果虚拟网络的 DNS 配置更改时要创建的应用程序服务环境，这将导致应用程序服务环境创建过程失败。

## 创建网络安全组 ##
网络安全组的工作原理的完整详细信息请参阅以下[信息][NetworkSecurityGroups]。  网络安全组，配置和应用网络安全组与子网包含的应用程序服务环境为重点的突出显示在触摸屏输入下面的详细信息。

首先，网络安全组被创建为与订阅相关的独立实体。 由于 Azure 的区域中创建网络安全组，请确保网络安全组被创建在同一区域作为应用程序服务环境。

下面演示如何创建网络安全组︰

    New-AzureNetworkSecurityGroup -Name "testNSGexample" -Location "South Central US" -Label "Example network security group for an app service environment"

创建网络安全组后，给它添加一个或多个网络安全规则。  因为随着时间的推移可能会更改的规则集，建议空间出用于规则的优先级来方便地随着时间的推移插入其他规则的编号方案。

下面的示例演示显式授予对所需的 Azure 的基础结构来管理和维护的应用程序服务环境管理端口的访问规则。  请注意，所有管理通信量通过 SSL 排列，因此即使打开端口将无法访问它们的 Azure 的管理基础结构以外的任意实体受客户端证书。


    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "ALLOW AzureMngmt" -Type Inbound -Priority 100 -Action Allow -SourceAddressPrefix 'INTERNET'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '454-455' -Protocol TCP
    

当锁定访问端口 80 和 443 为"隐藏"应用程序服务环境背后上游设备或服务，您需要知道的上游的 IP 地址。  例如，如果您使用的 web 应用程序防火墙 (WAF)，WAF 将有它自己的 IP 地址 （或地址） 用时代理通信流到下游应用程序服务环境。  您将需要使用此 IP 地址的网络安全规则的*SourceAddressPrefix*参数中。

在下面的示例中，是明确允许从上游的特定 IP 地址的入站的通信。  地址*1.2.3.4*上游 WAF 的 IP 地址用作占位符。  更改的值，以匹配您的上游设备或服务使用的地址。

    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT HTTP" -Type Inbound -Priority 200 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT HTTPS" -Type Inbound -Priority 300 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP
    
如果需要 FTP 支持，作为模板使用下面的规则，可以授予访问 FTP 控制端口和数据端口通道。  由于 FTP 是一种有状态协议，您可能不能穿传统的 HTTP/HTTPS 防火墙或代理设备的 FTP 通信。  在这种情况下需要将*SourceAddressPrefix*设置为一个不同的值-例如 IP 地址范围的开发人员或部署计算机运行的 FTP 客户端。 

    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT FTPCtrl" -Type Inbound -Priority 400 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '21' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT FTPDataRange" -Type Inbound -Priority 500 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '10001-10020' -Protocol TCP

(**注︰**在预览期间可能会更改数据通道端口范围。)

如果使用远程调试与 Visual Studio，则下列规则将演示如何授予访问权限。  由于每个版本使用不同的端口以进行远程调试，没有每个受支持版本的 Visual Studio 的不同规则。  与 FTP 访问，远程调试通信通过传统 WAF 或代理设备可能会不正确地排列。  相反， *SourceAddressPrefix*可以设置为运行 Visual Studio 开发人员计算机的 IP 地址范围中。

    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT RemoteDebuggingVS2012" -Type Inbound -Priority 600 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '4016' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT RemoteDebuggingVS2013" -Type Inbound -Priority 700 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '4018' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT RemoteDebuggingVS2015" -Type Inbound -Priority 800 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '4020' -Protocol TCP

## 分配到子网的网络安全组 ##
网络安全组已拒绝访问所有外部通信的默认安全规则。  通过组合，上面所述网络安全规则和默认安全规则阻止入站的通信，其结果是来自源地址与*允许*操作相关联的区域将能够将通信发送到应用程序的应用程序服务环境中运行的唯一通信量。

网络安全组填充了安全规则之后，需要分配给子网包含应用程序的服务环境。  工作分配命令引用的应用程序服务环境所在的虚拟网络名称以及创建应用程序服务环境所在子网的名称。  

下面的示例显示分配给子网和虚拟网络的网络安全组︰


    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityGroupToSubnet -VirtualNetworkName 'testVNet' -SubnetName 'Subnet-test'

网络安全组分配成功后 （分配是长时间运行的操作，可能需要几分钟才能完成），只*允许*规则匹配的入站的通信将成功到达应用程序服务环境中的应用程序。

出于完整性的考虑下面的示例演示如何移除并因此不同关联子网的网络安全组︰


    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Remove-AzureNetworkSecurityGroupFromSubnet -VirtualNetworkName 'testVNet' -SubnetName 'Subnet-test'

## 显式的 IP SSL 的特殊注意事项 ##
如果应用程序配置了一个显式的 IP 地址，而不是默认的 IP 地址的应用程序服务环境中，HTTP 和 HTTPS 通信排入子网通过一组不同的端口也比端口 80 和 443。

在初始预览应用程序服务环境的过程中不能确定 IP SSL 所使用的特定端口。  但是一旦通过门户网站、 命令行工具和 REST Api 公开这些信息，开发人员将能够配置网络安全组来控制通信通过这些端口。

## 入门教程

要开始使用应用程序服务的环境，请参阅[应用程序服务环境简介][IntroToAppServiceEnvironment]

围绕安全地连接到后端资源的应用程序服务环境中的应用程序的详细信息，请参阅[安全地连接到后端应用程序服务环境中的资源][SecurelyConnecttoBackend]

关于 Azure 应用程序服务平台的详细信息，请参阅[Azure 应用程序服务][AzureAppService]。

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- LINKS -->
[virtualnetwork]: https://azure.microsoft.com/documentation/articles/virtual-networks-faq/
[HowToCreateAnAppServiceEnvironment]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-an-app-service-environment/
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/
[IntroToAppServiceEnvironment]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[SecurelyConnecttoBackend]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-securely-connecting-to-backend-resources/ 

<!-- IMAGES -->
 

测试
