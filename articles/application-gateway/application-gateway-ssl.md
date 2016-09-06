---
ms.openlocfilehash: bfbd5434f2e4b168bfa07c737101082a204bb8a5
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="将应用程序网关配置的 SSL 卸载 |Microsoft Azure"
   description="本文提供了如何配置 SSL 卸载在 Azure 应用程序网关。"
   documentationCenter="na"
   services="application-gateway"
   authors="joaoma"
   manager="jdial"
   editor="tysonn"/>
<tags 
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article" 
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services" 
   ms.date="06/30/2015"
   ms.author="joaoma"/>

# 配置 SSL 卸载应用程序网关

可以配置应用程序网关以终止 SSL 会话在网关，可以避免代价高昂的 SSL 解密，web 场中的服务器。 前端服务器安装和管理应用程序，还简化了 SSL 卸载。

## 在开始之前

1. 安装最新版本的 Web 平台安装程序使用 Azure PowerShell cmdlet。 您可以从下载并安装最新版本[下载页面](http://azure.microsoft.com/downloads/)的**Windows PowerShell**部分。
2. 请确认您有使用有效的子网的工作虚拟网络。
3. 请您在虚拟的网络中，或者与公用 IP/VIP 分配有后端服务器。

要配置 SSL 卸载应用程序网关，请执行下列步骤中列出的顺序。

1. [创建新的应用程序网关](#create-a-new-application-gateway)
2. [上载的 SSL 证书](#upload-ssl-certificates) 
3. [配置网关](#configure-the-gateway)
4. [设置网关配置](#set-the-gateway-configuration)
5. [启动该网关](#start-the-gateway)
6. [验证网关状态](#verify-the-gateway-status)


## 创建新的应用程序网关

**若要创建网关**，使用`New-AzureApplicationGateway`cmdlet，替换为您自己的值。 请注意计费网关不会在这里启动。 成功启动网关时，在后面的步骤，开始计费。

此示例演示该 cmdlet 的第一行上跟输出。 

    PS C:\> New-AzureApplicationGateway -Name AppGwTest -VnetName testvnet1 -Subnets @("Subnet-1")

    VERBOSE: 4:31:35 PM - Begin Operation: New-AzureApplicationGateway 
    VERBOSE: 4:32:37 PM - Completed Operation: New-AzureApplicationGateway
    Name       HTTP Status Code     Operation ID                             Error 
    ----       ----------------     ------------                             ----
    Successful OK                   55ef0460-825d-2981-ad20-b9a8af41b399

**若要验证**是否已创建网关，您可以使用`Get-AzureApplicationGateway`cmdlet。


中的示例，*说明*， *InstanceCount*和*GatewaySize*都是可选参数。 *InstanceCount*的默认值为 2，最大值为 10。 *GatewaySize*的默认值为中。 小型和大型的其他可用的值。 因为网关尚未开始， *Vip*和*DnsName*均为空白。 网关处于运行状态后，这些将被创建。

此示例演示该 cmdlet 的第一行上跟输出。 

    PS C:\> Get-AzureApplicationGateway AppGwTest

    VERBOSE: 4:39:39 PM - Begin Operation:
    Get-AzureApplicationGateway VERBOSE: 4:39:40 PM - Completed 
    Operation: Get-AzureApplicationGateway
    Name: AppGwTest 
    Description: 
    VnetName: testvnet1 
    Subnets: {Subnet-1} 
    InstanceCount: 2 
    GatewaySize: Medium 
    State: Stopped 
    VirtualIPs: 
    DnsName:


## 上载的 SSL 证书 

使用`Add-AzureApplicationGatewaySslCertificate`将上载到应用程序网关的服务器证书*pfx*格式。 证书的名称是用户选择名称和应用程序网关内必须唯一。 此证书是通过名称引用，此应用程序网关的所有证书管理操作。

此示例演示该 cmdlet 的第一行上跟输出。 在此示例中的值替换为您自己。

    PS C:\> Add-AzureApplicationGatewaySslCertificate  -Name AppGwTest -CertificateName GWCert -Password <password> -CertificateFile <full path to pfx file> 
    
    VERBOSE: 5:05:23 PM - Begin Operation: Get-AzureApplicationGatewaySslCertificate 
    VERBOSE: 5:06:29 PM - Completed Operation: Get-AzureApplicationGatewaySslCertificate
    Name       HTTP Status Code     Operation ID                             Error 
    ----       ----------------     ------------                             ----
    Successful OK                   21fdc5a0-3bf7-2c12-ad98-192e0dd078ef

接下来，验证证书上载。 使用`Get-AzureApplicationGatewayCertificate`。

此示例演示该 cmdlet 的第一行上跟输出。 

    PS C:\> Get-AzureApplicationGatewaySslCertificate AppGwTest 

    VERBOSE: 5:07:54 PM - Begin Operation: Get-AzureApplicationGatewaySslCertificate 
    VERBOSE: 5:07:55 PM - Completed Operation: Get-AzureApplicationGatewaySslCertificate
    Name           : SslCert 
    SubjectName    : CN=gwcert.app.test.contoso.com 
    Thumbprint     : AF5ADD77E160A01A6......EE48D1A 
    ThumbprintAlgo : sha1RSA
    State..........: Provisioned


## 配置网关

应用程序网关配置包含多个值。 值可以绑定在一起，以构建配置。 

这些值为︰
 
- **后端服务器池︰**后端服务器的 IP 地址的列表。 列出的 IP 地址或者应属于 VNet 的子网，或应公用 IP/VIP。 
- **后端服务器池设置︰**每个池都有像端口、 协议和基于 cookie 关联设置。 这些设置到池密切相关，并且应用于池中的所有服务器。
- **前端端口︰**此端口是打开的应用程序网关的公用端口。 流量达到此端口，，然后获取重定向到一台后端服务器。
- **侦听器︰**侦听器具有前端端口、 协议 （Http 或 Https，这些都是区分大小写），和 SSL 证书名称 （如果配置 SSL 卸载）。 
- **规则︰**该规则绑定侦听器和后端服务器池并定义时将触发特定侦听器通信应定向到哪个后端服务器池。 目前，只有*基本*的规则被支持。 *基本*规则是循环负载分配。

**其他配置的说明︰**

对于 SSL 证书配置，请在**HttpListener**协议应当改为*Https* （区分大小写）。 **SslCert**元素需要被添加到**HttpListener**的值设置为相同的名称中使用 SSL 证书上面部分上传。 应更新前端端口为 443。

**若要启用 cookie 基于相似性**︰ 可以配置应用程序网关以确保该请求从客户端会话始终指向同一个 VM 在 web 场中。 这是通过注入的会话 cookie，这样的网关，以适当地直接通信。 若要启用 cookie 基于相似性，请**BackendHttpSettings**元素中设置*启用* **CookieBasedAffinity** 。 



通过创建的配置对象，或通过使用配置 XML 文件，您可以构建您的配置。 若要构造您的配置使用配置 XML 文件，请使用下面的示例。

**配置 XML 示例**


        <?xml version="1.0" encoding="utf-8"?>
    <ApplicationGatewayConfiguration xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/windowsazure">
        <FrontendIPConfigurations />
        <FrontendPorts>
            <FrontendPort>
                <Name>FrontendPort1</Name>
                <Port>443</Port>
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
                <Protocol>Https</Protocol>
                <SslCert>GWCert</SslCert>
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


## 设置网关配置

接下来，您将设置应用程序网关。 您可以使用`Set-AzureApplicationGatewayConfig`cmdlet 的配置对象或配置 XML 文件。


    PS C:\> Set-AzureApplicationGatewayConfig -Name AppGwTest -ConfigFile D:\config.xml

    VERBOSE: 7:54:59 PM - Begin Operation: Set-AzureApplicationGatewayConfig 
    VERBOSE: 7:55:32 PM - Completed Operation: Set-AzureApplicationGatewayConfig
    Name       HTTP Status Code     Operation ID                             Error 
    ----       ----------------     ------------                             ----
    Successful OK                   9b995a09-66fe-2944-8b67-9bb04fcccb9d

## 启动该网关

一旦配置了网关，则使用`Start-AzureApplicationGateway`用于启动该网关。 网关已成功启动后，将开始使用应用程序网关的计费。 


**注意︰**`Start-AzureApplicationGateway` Cmdlet 可能需要多达 15-20 分钟才能完成。 

   
    PS C:\> Start-AzureApplicationGateway AppGwTest 

    VERBOSE: 7:59:16 PM - Begin Operation: Start-AzureApplicationGateway 
    VERBOSE: 8:05:52 PM - Completed Operation: Start-AzureApplicationGateway
    Name       HTTP Status Code     Operation ID                             Error 
    ----       ----------------     ------------                             ----
    Successful OK                   fc592db8-4c58-2c8e-9a1d-1c97880f0b9b


## 验证网关状态

使用`Get-AzureApplicationGateway`用于检查网关的状态。 如果上一步中成功*启动 AzureApplicationGateway* ，状态*运行*，应和 Vip 和 DnsName 应具有有效的条目。 

此示例演示应用程序网关，运行，并已准备好采取通信 

    PS C:\> Get-AzureApplicationGateway AppGwTest 

    Name          : AppGwTest2
    Description   : 
    VnetName      : testvnet1
    Subnets       : {Subnet-1}
    InstanceCount : 2
    GatewaySize   : Medium
    State         : Running
    VirtualIPs    : {23.96.22.241}
    DnsName       : appgw-4c960426-d1e6-4aae-8670-81fd7a519a43.cloudapp.net


## 下一步行动


如果您想详细了解一般情况下负载平衡选项，请参阅︰

- [Azure 负载平衡器](https://azure.microsoft.com/documentation/services/load-balancer/)
- [Azure 的流量管理器](https://azure.microsoft.com/documentation/services/traffic-manager/)

测试
