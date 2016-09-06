---
ms.openlocfilehash: 30c0a04c99c5e50471b3d65d8142375c4b8fc644
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="创建和配置应用程序网关与内部负载平衡器 (ILB) 使用 Azure 资源管理器 |Microsoft Azure"
   description="此页面介绍了如何创建、 配置、 启动和删除 Azure 的资源管理器内部负载平衡器 (ILB) 与 Azure 应用程序网关"
   documentationCenter="na"
   services="application-gateway"
   authors="joaoma"
   manager="jdial"
   editor="tysonn"/>
<tags 
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="hero-article" 
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services" 
   ms.date="08/07/2015"
   ms.author="joaoma"/>


# 创建应用程序网关与使用 Azure 资源管理器内部负载平衡器 (ILB)

> [AZURE.SELECTOR]
- [Azure 经典的步骤](application-gateway-ilb.md)
- [资源管理器 Powershell 的步骤](application-gateway-ilb-arm.md)

与面向 VIP internet 或内部端点不暴露给 internet，也称为内部负载平衡器 (ILB) 终结点，可以配置应用程序网关。 使用 ILB 配置网关可用于内部业务线应用程序不向 internet 公开。 它也是有用的服务/中各层的多层应用程序驻留在安全边界不暴露给 internet，但仍需要循环负载分配、 会话建立密切关系或 SSL 端接。 这篇文章将引导您完成配置应用程序网关与 ILB 的步骤。

## 在开始之前

1. 安装最新版本的 Web 平台安装程序使用 Azure PowerShell cmdlet。 您可以从下载并安装最新版本[下载页面](http://azure.microsoft.com/downloads/)的**Windows PowerShell**部分。
2. 对于应用程序网关，您将创建一个虚拟的网络和子网。 请确保没有虚拟机或云部署使用的子网。 应用程序网关必须自行在虚拟的网络子网中。
3. 将被配置为使用应用程序网关服务器必须存在或已经创建了在虚拟的网络中，或与公用 IP/VIP 及其终结点分配。

## 必须创建应用程序网关是什么？
 

- **后端服务器池︰**后端服务器的 IP 地址的列表。 列出的 IP 地址或者应属于虚拟网，或应公用 IP/VIP。 
- **后端服务器池设置︰**每个池都有像端口、 协议和基于 cookie 关联设置。 这些设置到池密切相关，并且应用于池中的所有服务器。
- **前结束端口︰**此端口是打开的应用程序网关的公用端口。 流量达到此端口，，然后获取重定向到一台后端服务器。
- **侦听器︰**侦听器具有前端端口、 协议 （Http 或 Https，这些都是区分大小写），和 SSL 证书名称 （如果配置 SSL 卸载）。 
- **规则︰**该规则绑定侦听器和后面结束服务器池，定义的后端服务器池应被定向到流量，当它达到特定侦听器。 目前，只有*基本*的规则被支持。 *基本*规则是循环负载分配。


 
## 创建新的应用程序网关

使用 Azure 经典和 Azure 资源管理器之间的差异将是您将创建的应用程序网关和配置所需的项的顺序。
使用资源管理器中，所有项目这将使应用程序网关将单独进行配置，然后放置在一起以创建应用程序网关资源。


这里，如下所示创建应用程序网关所需的步骤︰

1. 创建资源组的资源管理器
2. 创建虚拟网络中，子网应用程序网关
3. 创建应用程序网关配置对象
4. 创建网关应用程序资源


## 创建资源组的资源管理器

请确保您切换使用 ARM cmdlet PowerShell 模式。 使用[Windows Powershell 与资源管理器](powershell-azure-resource-manager.md)提供了更多的信息。

### 第 1 步

    Switch-AzureMode -Name AzureResourceManager

### 第 2 步

登录到您的 Azure 帐户。


    Add-AzureAccount

您将被提示到身份验证使用您的凭据。


### 第 3 步

选择您要使用的 Azure 订阅。 

    Select-AzureSubscription -SubscriptionName "MySubscription"

若要查看可用的订阅列表，使用 Get AzureSubscription cmdlet。


### 第 4 步

创建新的资源组 （跳过此步骤如果使用现有资源组）

    New-AzureResourceGroup -Name appgw-rg -location "West US"

Azure 的资源管理器需要的所有资源组都指定一个位置。 这是该资源组中的资源用作默认位置。 请确保所有的命令，以创建应用程序网关将使用相同的资源组。

在上面的示例中，我们将创建资源组名为"appgw rg"和"西美国"的位置。 

## 创建虚拟网络中，子网应用程序网关

下面的示例演示如何创建使用资源的虚拟网络管理器︰ 

### 第 1 步  
    
    $subnet = New-AzureVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24

分配给子网变量被用来创建虚拟网络地址范围 10.0.0.0/24

### 第 2 步
    
    $vnet = New-AzurevirtualNetwork -Name appgwvnet -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnet

创建虚拟网络名资源组"appw-rg"中的"appgwvnet"为前缀 10.0.0.0/16 使用的子网 10.0.0.0/24 的美国西部地区 
    

## 创建应用程序网关配置对象

### 第 1 步

    $gipconfig = New-AzureApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet

创建一个名为"gatewayIP01"的应用程序网关 IP 配置。 应用程序网关启动时，它将拿起从配置子网的 IP 地址，将网络流量路由到后端 IP 池中的 IP 地址。 请记住每个实例需要一个 IP 地址。
 
### 第 2 步

    $pool = New-AzureApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 10.0.0.10,10.0.0.11,10.0.0.12

此步骤将配置的 IP 地址"10.0.0.10,10.0.0.11，10.0.0.12"名为"pool01"的后端 IP 地址池。 那些将接收来自前端 IP 终结点的网络通信的 IP 地址。将替换上面添加您自己应用程序的 IP 地址终结点的 IP 地址。

### 第 3 步

    $poolSetting = New-AzureApplicationGatewayBackendHttpSettings -Name poolsetting01 -Port 80 -Protocol Http -CookieBasedAffinity Disabled

在后端池中配置应用程序网关设置"poolsetting01"的负载平衡网络通讯量。

### 第 4 步

    $fp = New-AzureApplicationGatewayFrontendPort -Name frontendport01  -Port 80

将名为"frontendport01"ILB 前端 IP 端口配置。

### 第 5 步

    $fipconfig = New-AzureApplicationGatewayFrontendIPConfig -Name fipconfig01 -Subnet $subnet

创建名为"fipconfig01"的前端 IP 配置，将与当前的虚拟网络子网中的私有 IP 相关联。

### 第 6 步

    $listener = New-AzureApplicationGatewayHttpListener -Name listener01  -Protocol Http -FrontendIPConfiguration $fipconfig -FrontendPort $fp

将创建名为"listener01"的侦听器并将前端 IP 配置为前端端口。

### 第 7 步 

    $rule = New-AzureApplicationGatewayRequestRoutingRule -Name rule01 -RuleType Basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool

创建负载平衡路由规则称为"rule01"，配置负载平衡器行为。

### 第 8 步

    $sku = New-AzureApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2

配置应用程序网关实例的大小

>[AZURE.NOTE]  *InstanceCount*的默认值为 2，最大值为 10。 *GatewaySize*的默认值为中。 您可以选择 Standard_Small、 Standard_Medium 和 Standard_Large 之间。

## 创建应用程序网关使用 New AzureApplicationGateway

    $appgw = New-AzureApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku

从上面的步骤中创建应用程序网关将所有的配置项。 在示例应用程序网关称为"appgwtest"。 




## 启动该网关

一旦配置了网关，则使用`Start-AzureApplicationGateway`用于启动该网关。 网关已成功启动后，将开始使用应用程序网关的计费。 


**注意︰**`Start-AzureApplicationGateway` Cmdlet 可能需要多达 15-20 分钟才能完成。 

下面的示例中，应用程序网关被称为"appgwtest"和资源组是"appgw rg":


### 第 1 步

获取应用程序网关对象和关联到"$getgw"的变量︰
 
    $getgw =  Get-AzureApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg

### 第 2 步
     
使用`Start-AzureApplicationGateway`启动应用程序网关︰

    PS C:\> Start-AzureApplicationGateway -ApplicationGateway $getgw  

    PS C:\> Start-AzureApplicationGateway AppGwTest 

    VERBOSE: 7:59:16 PM - Begin Operation: Start-AzureApplicationGateway 
    VERBOSE: 8:05:52 PM - Completed Operation: Start-AzureApplicationGateway
    Name       HTTP Status Code     Operation ID                             Error 
    ----       ----------------     ------------                             ----
    Successful OK                   fc592db8-4c58-2c8e-9a1d-1c97880f0b9b

## 验证应用程序网关状态

使用`Get-AzureApplicationGateway`用于检查网关的状态。 如果上一步中成功*启动 AzureApplicationGateway* ，状态*运行*，应和 Vip 和 DnsName 应具有有效的条目。 

此示例演示应用程序网关最多、 运行，并已准备好采取通信发往`http://<generated-dns-name>.cloudapp.net`。 

    PS C:\> Get-AzureApplicationGateway -Name appgwtest -ResourceGroupName app-rg

    VERBOSE: 8:09:28 PM - Begin Operation: Get-AzureApplicationGateway 
    VERBOSE: 8:09:30 PM - Completed Operation: Get-AzureApplicationGateway
    Name          : AppGwTest 
    Description   : 
    VnetName      : appgwvnet 
    Subnets       : {Subnet01} 
    InstanceCount : 2 
    GatewaySize   : Medium 
    State         : Running 
    Vip           : 138.91.170.26 
    DnsName       : appgw-1b8402e8-3e0d-428d-b661-289c16c82101.cloudapp.net


## 删除应用程序网关

若要删除一个应用程序网关，您需要执行以下操作的顺序︰

1. 使用`Stop-AzureApplicationGateway`cmdlet 停止网关。 
2. 使用`Remove-AzureApplicationGateway`用于删除网关。
3. 验证是否已删除通过使用网关`Get-AzureApplicationGateway`cmdlet。

此示例演示`Stop-AzureApplicationGateway`的第一行上的 cmdlet 跟输出。 

### 第 1 步

获取应用程序网关对象和关联到"$getgw"的变量︰
 
    $getgw =  Get-AzureApplicationGateway -Name appgwtest -ResourceGroupName app-rg

### 第 2 步
     
使用`Stop-AzureApplicationGateway`停止应用程序网关︰

    PS C:\> Stop-AzureApplicationGateway -ApplicationGateway $getgw  

    VERBOSE: 9:49:34 PM - Begin Operation: Stop-AzureApplicationGateway 
    VERBOSE: 10:10:06 PM - Completed Operation: Stop-AzureApplicationGateway
    Name       HTTP Status Code     Operation ID                             Error 
    ----       ----------------     ------------                             ----
    Successful OK                   ce6c6c95-77b4-2118-9d65-e29defadffb8

一旦应用程序网关处于已停止状态时，使用`Remove-AzureApplicationGateway`用于在删除此服务。


    PS C:\> Remove-AzureApplicationGateway -Name $appgwName -ResourceGroupName $rgname -Force

    VERBOSE: 10:49:34 PM - Begin Operation: Remove-AzureApplicationGateway 
    VERBOSE: 10:50:36 PM - Completed Operation: Remove-AzureApplicationGateway
    Name       HTTP Status Code     Operation ID                             Error 
    ----       ----------------     ------------                             ----
    Successful OK                   055f3a96-8681-2094-a304-8d9a11ad8301

>[AZURE.NOTE] "-强制"开关可用于取消删除确认消息
>

若要验证在删除该服务，您可以使用`Get-AzureApplicationGateway`cmdlet。 此步骤不是必需的。


    PS C:\>Get-AzureApplicationGateway -Name appgwtest-ResourceGroupName app-rg

    VERBOSE: 10:52:46 PM - Begin Operation: Get-AzureApplicationGateway 

    Get-AzureApplicationGateway : ResourceNotFound: The gateway does not exist. 
    .....

## 下一步行动

如果您想要配置 SSL 卸载，请参阅[配置应用程序网关用于 SSL 卸载](application-gateway-ssl.md)。

如果您想要配置的应用程序网关与 ILB 一起使用，请参阅[创建应用程序网关与内部负载平衡器 (ILB)](application-gateway-ilb.md)。

如果您想详细了解一般情况下负载平衡选项，请参阅︰

- [Azure 负载平衡器](https://azure.microsoft.com/documentation/services/load-balancer/)
- [Azure 的流量管理器](https://azure.microsoft.com/documentation/services/traffic-manager/)


测试
