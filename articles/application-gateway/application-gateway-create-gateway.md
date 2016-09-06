---
ms.openlocfilehash: bfb39b55829e1aebc0a41c7028b19f972f564036
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="创建、 启动或删除应用程序网关 |Microsoft Azure"
   description="此页面介绍了如何创建、 配置、 启动和删除 Azure 应用程序网关"
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
   ms.date="07/29/2015"
   ms.author="joaoma"/>

# 创建、 启动或删除应用程序网关

在此版本中，您可以通过使用 PowerShell 创建应用程序网关或 REST API 调用。 Azure 门户和 CLI 支持将在未来版本中提供。
这篇文章将引导您完成创建和配置、 启动和删除应用程序网关的步骤。

> [AZURE.SELECTOR]
- [Azure 经典的步骤](application-gateway-create-gateway.md)
- [资源管理器 Powershell 的步骤](application-gateway-create-gateway-arm.md)



## 在开始之前

1. 安装最新版本的 Web 平台安装程序使用 Azure PowerShell cmdlet。 您可以从下载并安装最新版本[下载页面](http://azure.microsoft.com/downloads/)的**Windows PowerShell**部分。
2. 请确认您有使用有效的子网的工作虚拟网络。 请确保没有虚拟机或云部署使用的子网。 应用程序网关必须自行在虚拟的网络子网中。
3. 将被配置为使用应用程序网关服务器必须存在或已经创建了在虚拟的网络中，或与公用 IP/VIP 及其终结点分配。

## 必须创建应用程序网关是什么？


时通过采用**新的 AzureApplicationGateway**命令用于创建应用程序网关，此时设置没有配置，新创建的资源需要配置使用 XML 或配置对象。


这些值为︰

- **后端服务器池︰**后端服务器的 IP 地址的列表。 列出的 IP 地址或者应属于虚拟网，或应公用 IP/VIP。
- **后端服务器池设置︰**每个池都有设置，如端口、 协议和基于 cookie 的关系等。 这些设置到池密切相关，并且应用于池中的所有服务器。
- **前结束端口︰**此端口是打开的应用程序网关的公用端口。 流量达到此端口，，然后获取重定向到一台后端服务器。
- **侦听器︰**侦听器具有前端端口、 协议 （Http 或 Https，这些都是区分大小写），和 SSL 证书名称 （如果配置 SSL 卸载）。
- **规则︰**该规则绑定侦听器和后面结束服务器池，定义的后端服务器池应被定向到流量，当它达到特定侦听器。 目前，只有*基本*的规则被支持。 *基本*规则是循环负载分配。



## 创建新的应用程序网关

没有您将需要创建一个应用程序网关时要遵循的步骤的顺序︰

1. 创建应用程序网关资源。
2. 创建配置 XML 文件或配置对象。
3. 提交到新创建的应用程序网关资源配置。

### 创建应用程序网关资源

若要创建网关，请使用`New-AzureApplicationGateway`cmdlet，替换为您自己的值。 请注意计费网关不会在这里启动。 成功启动网关时，在后面的步骤，开始计费。

下面的示例创建一个新应用程序网关使用虚拟网络称为"testvnet1"和名为"网-1"的子网。


    PS C:\> New-AzureApplicationGateway -Name AppGwTest -VnetName testvnet1 -Subnets @("Subnet-1")

    VERBOSE: 4:31:35 PM - Begin Operation: New-AzureApplicationGateway
    VERBOSE: 4:32:37 PM - Completed Operation: New-AzureApplicationGateway
    Name       HTTP Status Code     Operation ID                             Error
    ----       ----------------     ------------                             ----
    Successful OK                   55ef0460-825d-2981-ad20-b9a8af41b399


 *描述*、 *InstanceCount*和*GatewaySize*是可选参数。


**若要验证**是否已创建网关，您可以使用`Get-AzureApplicationGateway`cmdlet。




    PS C:\> Get-AzureApplicationGateway AppGwTest
    Name          : AppGwTest
    Description   :
    VnetName      : testvnet1
    Subnets       : {Subnet-1}
    InstanceCount : 2
    GatewaySize   : Medium
    State         : Stopped
    VirtualIPs    : {}
    DnsName       :

>[AZURE.NOTE]  *InstanceCount*的默认值为 2，最大值为 10。 *GatewaySize*的默认值为中。 您可以选择小型、 中型和大型。


 因为网关尚未开始， *Vip*和*DnsName*均为空白。 网关处于运行状态后，这些将被创建。

## 配置应用程序网关

您可以配置应用程序网关使用 XML 或配置对象。

## 配置使用 XML 的应用程序网关

在以下示例中，您将使用 XML 文件来配置所有应用程序网关设置并将它们提交到应用程序网关资源。  

### 第 1 步  

将以下文本复制到记事本。

    <?xml version="1.0" encoding="utf-8"?>
    <ApplicationGatewayConfiguration xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/windowsazure">
        <FrontendPorts>
            <FrontendPort>
                <Name>(name-of-your-frontend-port)</Name>
                <Port>(port number)</Port>
            </FrontendPort>
        </FrontendPorts>
        <BackendAddressPools>
            <BackendAddressPool>
                <Name>(name-of-your-backend-pool)</Name>
                <IPAddresses>
                    <IPAddress>(your-IP-address-for-backend-pool)</IPAddress>
                    <IPAddress>(your-second-IP-address-for-backend-pool)</IPAddress>
                </IPAddresses>
            </BackendAddressPool>
        </BackendAddressPools>
        <BackendHttpSettingsList>
            <BackendHttpSettings>
                <Name>(backend-setting-name-to-configure-rule)</Name>
                <Port>80</Port>
                <Protocol>[Http|Https]</Protocol>
                <CookieBasedAffinity>Enabled</CookieBasedAffinity>
            </BackendHttpSettings>
        </BackendHttpSettingsList>
        <HttpListeners>
            <HttpListener>
                <Name>(name-of-the-listener)</Name>
                <FrontendPort>(name-of-your-frontend-port)</FrontendPort>
                <Protocol>[Http|Https]</Protocol>
            </HttpListener>
        </HttpListeners>
        <HttpLoadBalancingRules>
            <HttpLoadBalancingRule>
                <Name>(name-of-load-balancing-rule)</Name>
                <Type>basic</Type>
                <BackendHttpSettings>(backend-setting-name-to-configure-rule)</BackendHttpSettings>
                <Listener>(name-of-the-listener)</Listener>
                <BackendAddressPool>(name-of-your-backend-pool)</BackendAddressPool>
            </HttpLoadBalancingRule>
        </HttpLoadBalancingRules>
    </ApplicationGatewayConfiguration>

编辑配置项括号之间的值。 使用.xml 扩展名保存该文件

>[AZURE.IMPORTANT] Http 或 Https 协议项是区分大小写。

下面的示例演示如何使用配置文件来设置应用程序网关以负载平衡在公共端口 80 上的 Http 通信量并将网络通信发送到后端端口 80 2 IP 地址之间。

    <?xml version="1.0" encoding="utf-8"?>
    <ApplicationGatewayConfiguration xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/windowsazure">
        <FrontendPorts>
            <FrontendPort>
                <Name>FrontendPort1</Name>
                <Port>80</Port>
            </FrontendPort>
        </FrontendPorts>
        <BackendAddressPools>
            <BackendAddressPool>
                <Name>BackendPool1</Name>
                <IPAddresses>
                    <IPAddress>10.0.0.1</IPAddress>
                    <IPAddress>10.0.0.2</IPAddress>
                </IPAddresses>
            </BackendAddressPool>
        </BackendAddressPools>
        <BackendHttpSettingsList>
            <BackendHttpSettings>
                <Name>BackendSetting1</Name>
                <Port>80</Port>
                <Protocol>Http</Protocol>
                <CookieBasedAffinity>Enabled</CookieBasedAffinity>
            </BackendHttpSettings>
        </BackendHttpSettingsList>
        <HttpListeners>
            <HttpListener>
                <Name>HTTPListener1</Name>
                <FrontendPort>FrontendPort1</FrontendPort>
                <Protocol>Http</Protocol>
            </HttpListener>
        </HttpListeners>
        <HttpLoadBalancingRules>
            <HttpLoadBalancingRule>
                <Name>HttpLBRule1</Name>
                <Type>basic</Type>
                <BackendHttpSettings>BackendSetting1</BackendHttpSettings>
                <Listener>HTTPListener1</Listener>
                <BackendAddressPool>BackendPool1</BackendAddressPool>
            </HttpLoadBalancingRule>
        </HttpLoadBalancingRules>
    </ApplicationGatewayConfiguration>





### 第 2 步

接下来，您将设置应用程序网关。 您将使用`Set-AzureApplicationGatewayConfig`cmdlet 的配置 XML 文件。


    PS C:\> Set-AzureApplicationGatewayConfig -Name AppGwTest -ConfigFile "D:\config.xml"

    VERBOSE: 7:54:59 PM - Begin Operation: Set-AzureApplicationGatewayConfig
    VERBOSE: 7:55:32 PM - Completed Operation: Set-AzureApplicationGatewayConfig
    Name       HTTP Status Code     Operation ID                             Error
    ----       ----------------     ------------                             ----
    Successful OK                   9b995a09-66fe-2944-8b67-9bb04fcccb9d

## 配置应用程序网关使用的配置对象

下面的示例演示如何配置应用程序网关使用配置对象。 所有的配置项必须单独配置和添加到应用程序网关配置对象。 创建配置对象后，您将使用`Set-AzureApplicationGateway`命令以提交到以前创建的应用程序网关资源配置。

>[AZURE.NOTE] 在将值分配给每个配置对象之前, 您需要声明 PowerShell 使用存储哪种类型的对象。 第一行创建单个项目定义什么 Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model 将使用 （对象名称）。

### 第 1 步

创建单个配置的所有项目。

下面的示例中所示，创建前端 IP。

    PS C:\> $fip = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.FrontendIPConfiguration
    PS C:\> $fip.Name = "fip1"
    PS C:\> $fip.Type = "Private"
    PS C:\> $fip.StaticIPAddress = "10.0.0.5"

创建前端端口，如下面的示例中所示。

    PS C:\> $fep = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.FrontendPort
    PS C:\> $fep.Name = "fep1"
    PS C:\> $fep.Port = 80

创建后端服务器池

 定义的 IP 地址，它在下一个示例中所示添加到后端服务器池。


    PS C:\> $servers = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendServerCollection
    PS C:\> $servers.Add("10.0.0.1")
    PS C:\> $servers.Add("10.0.0.2")

 使用 $server 对象，将值添加到后端池对象 ($pool)。

    PS C:\> $pool = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendAddressPool
    PS C:\> $pool.BackendServers = $servers
    PS C:\> $pool.Name = "pool1"

创建池的后端服务器的设置

    PS C:\> $setting = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendHttpSettings
    PS C:\> $setting.Name = "setting1"
    PS C:\> $setting.CookieBasedAffinity = "enabled"
    PS C:\> $setting.Port = 80
    PS C:\> $setting.Protocol = "http"

创建侦听器

    PS C:\> $listener = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.HttpListener
    PS C:\> $listener.Name = "listener1"
    PS C:\> $listener.FrontendPort = "fep1"
    PS C:\> $listener.FrontendIP = "fip1"
    PS C:\> $listener.Protocol = "http"
    PS C:\> $listener.SslCert = ""

创建规则

    PS C:\> $rule = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.HttpLoadBalancingRule
    PS C:\> $rule.Name = "rule1"
    PS C:\> $rule.Type = "basic"
    PS C:\> $rule.BackendHttpSettings = "setting1"
    PS C:\> $rule.Listener = "listener1"
    PS C:\> $rule.BackendAddressPool = "pool1"

### 第 2 步

将所有单个配置项分配给应用程序网关配置对象 ($appgwconfig):

将前端 IP 添加到配置

    PS C:\> $appgwconfig = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.ApplicationGatewayConfiguration
    PS C:\> $appgwconfig.FrontendIPConfigurations = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.FrontendIPConfiguration]"
    PS C:\> $appgwconfig.FrontendIPConfigurations.Add($fip)

向配置添加前端端口

    PS C:\> $appgwconfig.FrontendPorts = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.FrontendPort]"
    PS C:\> $appgwconfig.FrontendPorts.Add($fep)

向配置添加后端服务器池

    PS C:\> $appgwconfig.BackendAddressPools = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendAddressPool]"
    PS C:\> $appgwconfig.BackendAddressPools.Add($pool)  

向配置添加后端池设置

    PS C:\> $appgwconfig.BackendHttpSettingsList = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendHttpSettings]"
    PS C:\> $appgwconfig.BackendHttpSettingsList.Add($setting)

将侦听器添加到配置

    PS C:\> $appgwconfig.HttpListeners = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.HttpListener]"
    PS C:\> $appgwconfig.HttpListeners.Add($listener)

将规则添加到配置

    PS C:\> $appgwconfig.HttpLoadBalancingRules = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.HttpLoadBalancingRule]"
    PS C:\> $appgwconfig.HttpLoadBalancingRules.Add($rule)

### 第 3 步

将配置对象提交给应用程序网关资源使用`Set-AzureApplicationGatewayConfig`。

    Set-AzureApplicationGatewayConfig -Name AppGwTest -Config $appgwconfig

## 启动该网关

一旦配置了网关，则使用`Start-AzureApplicationGateway`用于启动该网关。 网关已成功启动后，将开始使用应用程序网关的计费。


> [AZURE.NOTE] `Start-AzureApplicationGateway` Cmdlet 可能需要多达 15-20 分钟才能完成。



    PS C:\> Start-AzureApplicationGateway AppGwTest

    VERBOSE: 7:59:16 PM - Begin Operation: Start-AzureApplicationGateway
    VERBOSE: 8:05:52 PM - Completed Operation: Start-AzureApplicationGateway
    Name       HTTP Status Code     Operation ID                             Error
    ----       ----------------     ------------                             ----
    Successful OK                   fc592db8-4c58-2c8e-9a1d-1c97880f0b9b

## 验证网关状态

使用`Get-AzureApplicationGateway`用于检查网关的状态。 如果上一步中成功*启动 AzureApplicationGateway* ，状态*运行*，应和 Vip 和 DnsName 应具有有效的条目。

下面的示例演示应用程序网关最多、 运行，并已准备好采取通信发往`http://<generated-dns-name>.cloudapp.net`。

    PS C:\> Get-AzureApplicationGateway AppGwTest

    VERBOSE: 8:09:28 PM - Begin Operation: Get-AzureApplicationGateway
    VERBOSE: 8:09:30 PM - Completed Operation: Get-AzureApplicationGateway
    Name          : AppGwTest
    Description   :
    VnetName      : testvnet1
    Subnets       : {Subnet-1}
    InstanceCount : 2
    GatewaySize   : Medium
    State         : Running
    Vip           : 138.91.170.26
    DnsName       : appgw-1b8402e8-3e0d-428d-b661-289c16c82101.cloudapp.net


## 删除应用程序网关

要删除应用程序网关︰

1. 使用`Stop-AzureApplicationGateway`cmdlet 停止网关。
2. 使用`Remove-AzureApplicationGateway`用于删除网关。
3. 验证是否已删除通过使用网关`Get-AzureApplicationGateway`cmdlet。

下面的示例演示`Stop-AzureApplicationGateway`的第一行上的 cmdlet 跟输出。

    PS C:\> Stop-AzureApplicationGateway AppGwTest

    VERBOSE: 9:49:34 PM - Begin Operation: Stop-AzureApplicationGateway
    VERBOSE: 10:10:06 PM - Completed Operation: Stop-AzureApplicationGateway
    Name       HTTP Status Code     Operation ID                             Error
    ----       ----------------     ------------                             ----
    Successful OK                   ce6c6c95-77b4-2118-9d65-e29defadffb8

一旦应用程序网关处于已停止状态时，使用`Remove-AzureApplicationGateway`用于在删除此服务。


    PS C:\> Remove-AzureApplicationGateway AppGwTest

    VERBOSE: 10:49:34 PM - Begin Operation: Remove-AzureApplicationGateway
    VERBOSE: 10:50:36 PM - Completed Operation: Remove-AzureApplicationGateway
    Name       HTTP Status Code     Operation ID                             Error
    ----       ----------------     ------------                             ----
    Successful OK                   055f3a96-8681-2094-a304-8d9a11ad8301

若要验证在删除该服务，您可以使用`Get-AzureApplicationGateway`cmdlet。 此步骤不是必需的。


    PS C:\> Get-AzureApplicationGateway AppGwTest

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
