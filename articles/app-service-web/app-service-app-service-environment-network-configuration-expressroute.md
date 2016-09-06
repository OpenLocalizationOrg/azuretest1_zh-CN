---
ms.openlocfilehash: a217d159cc5fbea1abc4664ffb554769295bd8ca
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="使用快速通道的详细网络配置信息" 
    description="在虚拟的网络中运行的应用程序服务环境的详细网络配置信息连接到 ExpressRoute 电路。" 
    services="app-service\web" 
    documentationCenter="" 
    authors="stefsch" 
    manager="nirma" 
    editor=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="06/30/2015" 
    ms.author="stefsch"/>   

# ExpressRoute 的应用程序服务环境的详细网络配置信息 

## 概述 ##
客户可将[Azure ExpressRoute][ExpressRoute]电路连接到其虚拟网络基础架构，从而将内部网络扩展到 Azure。  此[虚拟网络][virtualnetwork]基础结构的子网中，可以创建一个应用程序的服务环境。  应用程序服务环境上运行的应用程序然后可以建立安全连接访问后端资源，只能通过 ExpressRoute 连接。  

## 所需的网络连接 ##
有网络连接应用程序服务不可能在虚拟网络连接到 ExpressRoute 最初符合的环境要求。

应用程序服务环境需要以下所有才能正常工作︰


-  Azure 存储和 Sql 数据库资源位于同一区域作为应用程序服务环境中的出站网络连接。  此网络路径不能因为这样做将有可能更改出站网络通信的有效 NAT 地址经过内部公司代理。  更改针对 Azure 存储和 Sql 数据库终结点的应用程序服务环境出站网络通信的 NAT 地址会导致连接故障。
-  虚拟网络的 DNS 配置必须能够解析以下中的终结点的 Azure 控制域︰ * *。 file.core.windows.net*， * *。 blob.core.windows.net*，**。 database.windows.net*。
-  每当应用程序服务环境创建，以及在重新配置和扩展对应用程序服务环境，虚拟网络的 DNS 配置必须保持稳定。   
-  此[文章][requiredports]中所述，必须允许所需端口的入站的网络访问的应用程序服务环境。

可以通过确保有效的 DNS 配置为虚拟网络满足 DNS 需求。  

可以通过配置应用程序服务环境的子网，以允许所需的访问，此[文章][requiredports]中所述[网络安全组][NetworkSecurityGroups]满足的入站的网络访问要求。

## 启用出站网络连接的应用程序服务环境##
默认情况下，新创建的 ExpressRoute 电路公布允许出站 Internet 连接的默认路由。  使用此配置应用程序服务环境将能够连接到其他 Azure 的终结点。

然而常见的客户配置是定义自己强制出站 Internet 通讯改为流动通过客户的代理/防火墙结构内部部署的默认路由。  由于出站通信量或者阻止的内部，或 NAT 将不再可用于各种 Azure 的终结点的地址无法识别套，此通信流总是中断应用程序服务的环境。

解决方法是定义一个 （或多个） 的用户定义路由 (UDRs) 位于子网包含应用程序的服务环境。  UDR 定义将起作用而不是默认工艺路线的特定子网的路由。

用户定义的路由的背景信息是本[概述][UDROverview]中可用。  

[UDRHowTo]该[如何为指南]提供了创建和配置用户定义工艺路线的详细信息。

## 若要为应用程序服务环境的的 UDR 配置示例 ##

**必备组件**

1. 从[Azure 下载页面][AzureDownloads] （日期 6 月 2015年或更高版本） 上安装最新的 Azure Powershell。  在"命令行工具"下，没有在"Windows Powershell"将安装最新的 Powershell cmdlet 下"安装"链接。

2. 建议应用程序服务环境，唯一的子网创建供独占使用。  这样可确保应用到的子网 UDRs 为应用程序服务环境将仅打开出站通信。
3. **重要提示**︰ 不**后**遵循下列配置步骤直到部署应用程序的服务环境。  这样可确保在部署的应用程序服务环境之前出站网络连接时可用。

**步骤 1︰ 创建一个命名的路由表**

下面的代码段创建一个西美国 Azure 地区称为"DirectInternetRouteTable"的路由表︰

    New-AzureRouteTable -Name 'DirectInternetRouteTable' -Location uswest

**步骤 2︰ 在路由表中创建一个或多个路由**

您将需要将一个或多个路由添加到路由表中，以启用出站 Internet 访问权限。  下面的示例将添加足够路由，以涵盖所有可能的 Azure 地址在美国西部地区使用。

    Get-AzureRouteTable -Name 'DirectInternetRouteTable' | Set-AzureRoute -RouteName 'Direct Internet Range 1' -AddressPrefix 23.0.0.0/8 -NextHopType Internet
    Get-AzureRouteTable -Name 'DirectInternetRouteTable' | Set-AzureRoute -RouteName 'Direct Internet Range 2' -AddressPrefix 40.0.0.0/8 -NextHopType Internet
    Get-AzureRouteTable -Name 'DirectInternetRouteTable' | Set-AzureRoute -RouteName 'Direct Internet Range 3' -AddressPrefix 65.0.0.0/8 -NextHopType Internet
    Get-AzureRouteTable -Name 'DirectInternetRouteTable' | Set-AzureRoute -RouteName 'Direct Internet Range 4' -AddressPrefix 104.0.0.0/8 -NextHopType Internet
    Get-AzureRouteTable -Name 'DirectInternetRouteTable' | Set-AzureRoute -RouteName 'Direct Internet Range 5' -AddressPrefix 137.0.0.0/8 -NextHopType Internet
    Get-AzureRouteTable -Name 'DirectInternetRouteTable' | Set-AzureRoute -RouteName 'Direct Internet Range 6' -AddressPrefix 138.0.0.0/8 -NextHopType Internet
    Get-AzureRouteTable -Name 'DirectInternetRouteTable' | Set-AzureRoute -RouteName 'Direct Internet Range 7' -AddressPrefix 157.0.0.0/8 -NextHopType Internet
    Get-AzureRouteTable -Name 'DirectInternetRouteTable' | Set-AzureRoute -RouteName 'Direct Internet Range 8' -AddressPrefix 168.0.0.0/8 -NextHopType Internet
    Get-AzureRouteTable -Name 'DirectInternetRouteTable' | Set-AzureRoute -RouteName 'Direct Internet Range 9' -AddressPrefix 191.0.0.0/8 -NextHopType Internet


有关使用 Azure 的 CIDR 范围的综合和更新列表，您可以下载的 Xml 文件，其中包含所有从[Microsoft 下载中心获取][DownloadCenterAddressRanges]的范围 

**注意︰**将可在*AddressPrefix*参数中使用缩写的 CIDR 短一手 0.0.0.0/0 的某个时刻。  此短手相当于"所有 Internet 地址"。  现在开发人员将需要改为使用更广泛的 CIDR 范围足以覆盖的区域在已部署的应用程序服务环境中使用的所有可能 Azure 的地址范围。

**步骤 3︰ 将包含应用程序服务环境的子网路由表相关联**

最后一个配置步骤是相关联的子网路由表将在其中部署应用程序的服务环境。  以下命令将关联到最终将包含应用程序服务环境"ASESubnet""DirectInternetRouteTable"。

    Set-AzureSubnetRouteTable -VirtualNetworkName 'YourVirtualNetworkNameHere' -SubnetName 'ASESubnet' -RouteTableName 'DirectInternetRouteTable'


**步骤 4︰ 最后步骤**

一旦在路由表绑定到子网，则建议先测试并确认预期的效果。  例如，在子网中部署虚拟机，并确认︰


- 下的 ExpressRoute 电路不流动到 Azure 的终结点的出站通信。
- Azure 的终结点的 DNS 查找被正确解析。 

一旦确认以上步骤，可以继续创建应用程序服务环境 ！

## 入门教程

要开始使用应用程序服务的环境，请参阅[应用程序服务环境简介][IntroToAppServiceEnvironment]

关于 Azure 应用程序服务平台的详细信息，请参阅[Azure 应用程序服务][AzureAppService]。

<!-- LINKS -->
[virtualnetwork]: http://azure.microsoft.com/services/virtual-network/
[ExpressRoute]: http://azure.microsoft.com/services/expressroute/
[requiredports]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-control-inbound-traffic/
[NetworkSecurityGroups]: http://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[UDROverview]: http://azure.microsoft.com/documentation/articles/virtual-networks-udr-overview/
[UDRHowTo]: http://azure.microsoft.com/documentation/articles/virtual-networks-udr-how-to/
[HowToCreateAnAppServiceEnvironment]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-an-app-service-environment/
[AzureDownloads]: http://azure.microsoft.com/en-us/downloads/ 
[DownloadCenterAddressRanges]: http://www.microsoft.com/download/details.aspx?id=41653  
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/
[IntroToAppServiceEnvironment]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
 

<!-- IMAGES -->

测试
