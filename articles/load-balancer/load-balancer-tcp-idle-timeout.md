---
ms.openlocfilehash: 3170c991cb3fa6d317fe114a01c50d2fbb851a22
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="配置负载平衡器 TCP 空闲超时 |Microsoft Azure"
   description="配置负载平衡器 TCP 空闲超时"
   services="load-balancer"
   documentationCenter="na"
   authors="joaoma"
   manager="adinah"
   editor="tysonn" />
<tags 
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/12/2015"
   ms.author="joaoma" />

# 如何更改 TCP 空闲超时设置为负载平衡器

在其默认配置中，Azure 负载平衡器有 4 分钟的一个空闲超时设置。

这意味着如果您有 tcp 或 http 会话超时值超过在一段处于非活动状态，就不能保证能够维护客户端与服务之间的连接。

当关闭连接时，客户端应用程序会像一条错误消息"基础连接已关闭︰ 应保持活动状态的连接已关闭服务器"。

常见的做法，以保持更长一段连接活动是使用 TCP 保持活动 (可以找到.NET 示例[这里](https://msdn.microsoft.com/library/system.net.servicepoint.settcpkeepalive.aspx))。

在没有活动连接时检测到发送的数据包。 通过保持日常的网络活动，则空闲超时值永远不会命中和长时间才会保持连接。

其目的是要使用比默认超时设置，以避免连接已除去或增加的空闲超时值为 TCP 连接会话保持连接的短间隔配置 TCP 保持活动状态。

虽然 TCP 保持适用于电池不是约束的情况下，通常是不为移动应用程序的有效选项。 使用 TCP 保持从移动应用程序可能会耗尽设备电池更快。

若要支持这样的情况下，我们已添加了可配置空闲超时。 您现在可以将其设置介于 4 到 30 分钟的持续时间。 此设置适用于仅入站连接。

![tcptimeout](./media/load-balancer-tcp-idle-timeout/image1.png)


## 如何更改在虚拟机中的空闲超时设置和云服务

- 通过 PowerShell 或服务管理 API 的虚拟机上配置的终结点的 TCP 超时
- 配置 TCP 超时 PowerShell 或服务管理 API 通过您 Load-Balanced 终结点集。
- 为您的实例级公用 IP 配置 TCP 超时
- 配置 TCP 超时服务模型通过您 Web/辅助角色。
 

>[AZURE.NOTE] 请记住某些命令将只存在于最新的 Azure PowerShell 包。 如果 powershell 命令不存在，下载最新的 PowerShell 包。

 
### 配置 TCP 超时 15 分钟到您实例级公用 ip。

    Set-AzurePublicIP –PublicIPName webip –VM MyVM -IdleTimeoutInMinutes 15

IdleTimeoutInMinutes 是可选的。 如果没有设置，默认超时值是 4 分钟。 

>[AZURE.NOTE] 超时可接受范围是介于 4 到 30 分钟之间。
 
### 在虚拟机上创建的 Azure 的终结点时设置空闲超时

若要更改超时设置为终结点

    Get-AzureVM -ServiceName "mySvc" -Name "MyVM1" | Add-AzureEndpoint -Name "HttpIn" -Protocol "tcp" -PublicPort 80 -LocalPort 8080 -IdleTimeoutInMinutes 15| Update-AzureVM
 
检索您的空闲超时配置

    PS C:\> Get-AzureVM –ServiceName “MyService” –Name “MyVM” | Get-AzureEndpoint
    VERBOSE: 6:43:50 PM - Completed Operation: Get Deployment
    LBSetName : MyLoadBalancedSet
    LocalPort : 80
    Name : HTTP
    Port : 80
    Protocol : tcp
    Vip : 65.52.xxx.xxx
    ProbePath :
    ProbePort : 80
    ProbeProtocol : tcp
    ProbeIntervalInSeconds : 15
    ProbeTimeoutInSeconds : 31
    EnableDirectServerReturn : False
    Acl : {}
    InternalLoadBalancerName :
    IdleTimeoutInMinutes : 15
 
### 在负载平衡的端点集上设置 TCP 超时

如果终结点是负载平衡的端点集的一部分，必须在负载平衡的端点集设置 TCP 超时︰

    Set-AzureLoadBalancedEndpoint -ServiceName "MyService" -LBSetName "LBSet1" -Protocol tcp -LocalPort 80 -ProbeProtocolTCP -ProbePort 8080 -IdleTimeoutInMinutes 15
 
### 云服务的更改超时设置

您可以利用 Azure SDK 的.NET 2.4 来更新您的云服务。

在.csdef 进行的云服务的终结点设置。 为了更新 TCP 超时为云服务部署，部署升级是必需的。 为公用 IP 仅在指定 TCP 超时是一个例外。 公共 IP 设置正在.cscfg，并且可以对其进行更新通过部署进行更新和升级。

终结点设置为.csdef 的更改包括︰

    <WorkerRole name="worker-role-name" vmsize="worker-role-size" enableNativeCodeExecution="[true|false]">
      <Endpoints>
    <InputEndpoint name="input-endpoint-name" protocol="[http|https|tcp|udp]" localPort="local-port-number" port="port-number" certificate="certificate-name" loadBalancerProbe="load-balancer-probe-name" idleTimeoutInMinutes="tcp-timeout" />
      </Endpoints>
    </WorkerRole>

超时设置上公用 Ip.cscfg 的更改包括︰

    <NetworkConfiguration>
     <VirtualNetworkSite name="VNet"/>
     <AddressAssignments>
    <InstanceAddress roleName="VMRolePersisted">
      <PublicIPs>
        <PublicIP name="public-ip-name" idleTimeoutInMinutes="timeout-in-minutes"/>
      </PublicIPs>
    </InstanceAddress>
     </AddressAssignments>
    </NetworkConfiguration>

## Rest API 示例

您可以配置使用服务管理确保添加的 x-ms-版本标头设置为 2014年-06-01 版本或更高版本的 API 使 TCP 空闲超时。
 
更新指定的负载平衡输入上的终结点的部署中的所有虚拟机的配置
    
    Request

    POST https://management.core.windows.net/<subscription-id>/services/hostedservices/<cloudservice-name>/deployments/<deployment-name>
<BR>

    Response

    <LoadBalancedEndpointList xmlns="http://schemas.microsoft.com/windowsazure" xmlns:i="http://www.w3.org/2001/XMLSchema-instance">
    <InputEndpoint>
    <LoadBalancedEndpointSetName>endpoint-set-name</LoadBalancedEndpointSetName>
    <LocalPort>local-port-number</LocalPort>
    <Port>external-port-number</Port>
    <LoadBalancerProbe>
    <Path>path-of-probe</Path>
    <Port>port-assigned-to-probe</Port>
    <Protocol>probe-protocol</Protocol>
    <IntervalInSeconds>interval-of-probe</IntervalInSeconds>
    <TimeoutInSeconds>timeout-for-probe</TimeoutInSeconds>
    </LoadBalancerProbe>
    <LoadBalancerName>name-of-internal-loadbalancer</LoadBalancerName>
    <Protocol>endpoint-protocol</Protocol>
    <IdleTimeoutInMinutes>15</IdleTimeoutInMinutes>
    <EnableDirectServerReturn>enable-direct-server-return</EnableDirectServerReturn>
    <EndpointACL>
    <Rules>
    <Rule>
    <Order>priority-of-the-rule</Order>
    <Action>permit-rule</Action>
    <RemoteSubnet>subnet-of-the-rule</RemoteSubnet>
    <Description>description-of-the-rule</Description>
    </Rule>
    </Rules>
    </EndpointACL>
    </InputEndpoint>
    </LoadBalancedEndpointList>

## 下一步行动

[内部负载平衡器概述](load-balancer-internal-overview.md)

[开始配置 Internet 面临负载平衡器](load-balancer-internet-getstarted.md)

[配置负载平衡器分布模式](load-balancer-distribution-mode.md)

 
测试
