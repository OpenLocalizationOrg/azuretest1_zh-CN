---
ms.openlocfilehash: 5a5831684c20ca27758b5e4613662e6159a16513
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   authors="danielceckert" 
   documentationCenter="dev-center-name" 
   editor=""
   manager="jefco" 
   pageTitle="管理︰ 负载平衡器空闲超时" 
   description="Azure 负载平衡器空闲超时的管理功能" 
   services="virtual-network" 
   />

<tags
   ms.author="danecke"
   ms.date="05/27/2015"
   ms.devlang="na"
   ms.service="virtual-network"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   /> 
   
# 管理虚拟网络︰ 负载平衡器 TCP 空闲超时

**TCP 空闲超时**允许开发人员指定在涉及 Azure 负载平衡器的客户端 / 服务器会话期间保证不活动阈值。  4 分钟的 TCP 空闲超时值 （默认值为 Azure 负载平衡器） 意味着，如果有一段不活动状态持续时间长于 4 分钟期间客户端 / 服务器会话涉及 Azure 负载平衡器，连接将被关闭。

当关闭客户端-服务器连接时，客户端应用程序将显示一条错误消息类似"基础连接已关闭︰ 应保持活动状态的连接已关闭服务器"。

[TCP 保持连接](http://tools.ietf.org/html/rfc1122#page-101)是常见的做法，在长时间否则为不活动期间[（MSDN 示例）](http://msdn.microsoft.com/library/system.net.servicepoint.settcpkeepalive.aspx)保持连接。 当使用 TCP 保持加电时，客户端 （通常采用频率的期间短于服务器的空闲超时阈值） 定期发送简单的数据包。  即使没有其他活动发生--因此永远不会满足的空闲超时值和长的一段时间，可保留的连接时，服务器认为这些传输作为连接活动的证据。

虽然 TCP 保持效果好，通常是不为移动应用程序的选项因为它会占用有限的电力资源，在移动设备上。 移动应用程序使用 TCP 保持将更快地耗尽设备电池，因为它不断地绘制网络使用的电源。

若要支持移动设备的方案，Azure 负载平衡器支持可配置 TCP 空闲超时。 开发人员可以为任何 4 分钟和 30 分钟 （可配置 TCP 空闲超时不适用于出站连接） 的入站连接之间的持续时间设置 TCP 空闲超时。 这使客户端能够保持更长的时间会话与长时间的不活动的服务器。  移动设备上的应用程序仍然可以选择利用 TCP 保持活动的技术，以保持连接的预期时间的不活动时间超过 30 分钟，但这更长的 TCP 空闲超时允许应用程序发出 TCP 保持请求远较少比之前，大大降低了移动设备电力资源的压力。

## 实现

可以为配置 TCP 空闲超时︰ 

* [实例级公用 Ip](virtual-networks-instance-level-public-ip.md)
* [负载平衡的端点集](../load-balancer/load-balancer-overview.md)
* [虚拟机的终结点](../virtual-machines/virtual-machines-set-up-endpoints.md)
* [Web 角色](http://msdn.microsoft.com/library/windowsazure/ee758711.aspx)
* [工作角色](http://msdn.microsoft.com/library/windowsazure/ee758711.aspx)

## 下一步行动
* TBD

## PowerShell 示例
请下载[最新的 Azure PowerShell 释放](https://github.com/Azure/azure-sdk-tools/releases)达到最佳效果。

### 配置 TCP 超时 15 分钟到您实例级公用 ip

    Set-AzurePublicIP –PublicIPName webip –VM MyVM -IdleTimeoutInMinutes 15

IdleTimeoutInMinutes 是可选的。 如果没有设置，默认超时值是 4 分钟。 现在，它的值可以设置 4 到 30 分钟之间。

### 在虚拟机上创建的 Azure 的终结点时设置空闲超时

    Get-AzureVM -ServiceName "mySvc" -Name "MyVM1" | Add-AzureEndpoint -Name "HttpIn" -Protocol "tcp" -PublicPort 80 -LocalPort 8080 -IdleTimeoutInMinutes 15| Update-AzureVM

### 检索您的空闲超时配置

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

## 云服务示例

您可以利用 Azure SDK.net 来更新您的云服务

在.csdef 进行的云服务的终结点设置。 因此，为了更新 TCP 超时为云服务部署，部署升级是必需的。 为公用 IP 仅在指定 TCP 超时是一个例外。 公共 IP 设置正在.cscfg，并且可以对其进行更新通过部署进行更新和升级。

终结点设置为.csdef 的更改包括︰

    <WorkerRole name="worker-role-name" vmsize="worker-role-size" enableNativeCodeExecution="[true|false]">
      <Endpoints>
        <InputEndpoint name="input-endpoint-name" protocol="[http|https|tcp|udp]" localPort="local-port-number" port="port-number" certificate="certificate-name" loadBalancerProbe="load-balancer-probe-name" idleTimeoutInMinutes="tcp-timeout" />
      </Endpoints>
    </WorkerRole>The .cscfg changes for the timeout setting on Public IPs are:
    
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
    
## API 示例

开发人员可以配置负载平衡器分发使用服务管理 API。  请确保要添加 x-ms-版本标头设置为版本 2014年-06-01 或更高。

### 更新指定的负载平衡输入上的终结点的部署中的所有虚拟机的配置

#### 请求

    POST https://management.core.windows.net/<subscription-id>/services/hostedservices/<cloudservice-name>/deployments/<deployment-name>

LoadBalancerDistribution 的值可以是 sourceIP 二元关系、 sourceIPProtocol 为三元关系或无 （没有关系。 即 5 元组）

#### 响应

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
 
