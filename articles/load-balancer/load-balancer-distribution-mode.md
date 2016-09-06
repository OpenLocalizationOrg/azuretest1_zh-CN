---
ms.openlocfilehash: eec3ac76e883756b9e7be60160b5587e4420e48d
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="配置负载平衡器分布模式 |Microsoft Azure"
   description="如何配置 Azure 负载平衡器分布模式来支持源 IP 相似性"
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
   ms.date="08/02/2015"
   ms.author="joaoma" />


# 负载平衡器 （来源 IP 关联） 的分布模式

我们引入了新的分发模式，称为源 IP 关系 （也称为会话关联或客户端 IP 相关性）。 Azure 负载平衡器可以被配置为使用 2 元组 （来源 IP、 目标 IP） 或 3 元组 （源 IP、 目标 IP 协议） 将映射到可用的服务器的通信。 通过使用来源 IP 的相似性，从相同的客户端计算机启动连接转到同一个 DIP 终结点。

![希基于负载平衡器](./media/load-balancer-distribution-mode/load-balancer-session-affinity.png)

源 IP 关系解决了 Azure 负载平衡器和 RD 网关之间的不兼容。 现在，您可以构建一个云服务中的 RD 网关服务器场。
另一种使用情形是媒体传真实数据上载通过 UDP 发生的地方，但通过 TCP 控制平面实现的其中︰

- 客户端首先启动 TCP 会话到负载平衡公用地址、 获取定向到特定的调节，该频道仍保持活动状态以监视的连接运行状况
- 新的 UDP 会话从同一个客户端计算机启动到同一个负载平衡公共终结点，这里的期望是根据以前的 TCP 连接以便上载媒体可以执行最高吞吐量的同时也通过 TCP 控制通道，此连接还定向到同一个 DIP 终结点。
 
注意，如果负载平衡组更改 （删除或添加虚拟机），分发客户端请求时都重新计算。 您不能依赖于从现有的客户端会话结束在同一台服务器的新连接。 此外，使用来源 IP 相关性分布模式可能导致通信等分发。 运行代理服务器后面的客户端可能会被视为一个唯一的客户机应用程序。

使用分配算法是一个 5 元组 (源 IP，源端口目标 IP、 目标端口协议类型) 哈希将映射到可用的服务器的通信。 它只在传输会话中提供建立密切关系。 在同一会话中 TCP 或 UDP 数据包将被定向到负载平衡终结点后面的同一个数据中心 IP (DIP) 实例。 当客户端关闭并重新打开该连接或来自同一来源 IP 启动一个新会话时，源端口更改并导致转到不同的 DIP 终结点的通信。

![希基于负载平衡器](./media/load-balancer-distribution-mode/load-balancer-distribution.png)


## 配置源 IP 关系设置为负载平衡器
 
为虚拟机，您可以使用 powershell 更改超时设置︰
 
Azure 的终结点添加到虚拟机并设置负载平衡器分布模式

    Get-AzureVM -ServiceName "mySvc" -Name "MyVM1" | Add-AzureEndpoint -Name "HttpIn" -Protocol "tcp" -PublicPort 80 -LocalPort 8080 –LoadBalancerDistribution “sourceIP”| Update-AzureVM

>[AZURE.NOTE] 可以将 LoadBalancerDistribution 设置为 sourceIP 2 元 （来源 IP、 目标 IP） 负载平衡，sourceIPProtocol 3 元组 （源 IP、 DestinaDestination IP 协议） 负载平衡或无如果所需的 5 元组负载平衡的默认行为


获取终结点负载平衡器分布模式配置

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
    LoadBalancerDistribution : sourceIP
 
如果不存在 LoadBalancerDistribution 元素 Azure 负载平衡器将使用默认值 5 元组算法。

 
### 设置负载平衡的端点集分布模式

如果终结点是负载平衡的端点集的一部分，必须在负载平衡的端点集设置分布模式︰

    Set-AzureLoadBalancedEndpoint -ServiceName "MyService" -LBSetName "LBSet1" -Protocol tcp -LocalPort 80 -ProbeProtocolTCP -ProbePort 8080 –LoadBalancerDistribution "sourceIP"

### 云服务配置分发模式更改

您可以利用 Azure SDK 的.NET 2.5 （要在 11 月发布） 来更新您的云服务终结点设置为云服务在.csdef 中进行。 为了更新云服务部署负载平衡器分布模式，部署升级是必需的。
以下是.csdef 更改为终结点设置的示例︰

    <WorkerRole name="worker-role-name" vmsize="worker-role-size" enableNativeCodeExecution="[true|false]">
    <Endpoints>
    <InputEndpoint name="input-endpoint-name" protocol="[http|https|tcp|udp]" localPort="local-port-number" port="port-number" certificate="certificate-name" loadBalancerProbe="load-balancer-probe-name" loadBalancerDistribution="sourceIP" />
    </Endpoints>
    </WorkerRole>
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


## API 的示例

您可以配置使用服务管理 API 请确保要添加 x-ms-版本标头设置为版本 2014年-09-01 或更高的负载平衡器分布。
 
更新部署中指定的负载平衡设置的配置

请求的示例

    POST https://management.core.windows.net/<subscription-id>/services/hostedservices/<cloudservice-name>/deployments/<deployment-name>?comp=UpdateLbSet 

    x-ms-version: 2014-09-01 

    Content-Type: application/xml 

    <LoadBalancedEndpointList xmlns="http://schemas.microsoft.com/windowsazure" xmlns:i="http://www.w3.org/2001/XMLSchema-instance"> 
    <InputEndpoint> 
    <LoadBalancedEndpointSetName> endpoint-set-name </LoadBalancedEndpointSetName> 
    <LocalPort> local-port-number </LocalPort> 
    <Port> external-port-number </Port> 
    <LoadBalancerProbe> 
    <Port> port-assigned-to-probe </Port> 
    <Protocol> probe-protocol </Protocol> 
    <IntervalInSeconds> interval-of-probe </IntervalInSeconds> 
    <TimeoutInSeconds> timeout-for-probe </TimeoutInSeconds> 
    </LoadBalancerProbe> 
    <Protocol> endpoint-protocol </Protocol> 
    <EnableDirectServerReturn> enable-direct-server-return </EnableDirectServerReturn> 
    <IdleTimeoutInMinutes>idle-time-out</IdleTimeoutInMinutes> 
    <LoadBalancerDistribution>sourceIP</LoadBalancerDistribution> 
    </InputEndpoint> 
    </LoadBalancedEndpointList>

LoadBalancerDistribution 的值可以是 sourceIP 二元关系、 sourceIPProtocol 为三元关系或无 （没有关系。 即 5 元组）

    Response

    HTTP/1.1 202 Accepted 
    Cache-Control: no-cache 
    Content-Length: 0 
    Server: 1.0.6198.146 (rd_rdfe_stable.141015-1306) Microsoft-HTTPAPI/2.0 
    x-ms-servedbyregion: ussouth2 
    x-ms-request-id: 9c7bda3e67c621a6b57096323069f7af 
    Date: Thu, 16 Oct 2014 22:49:21 GMT

## 下一步行动

[内部负载平衡器概述](load-balancer-internal-overview.md)

[开始配置 Internet 面临负载平衡器](load-balancer-internet-getstarted.md)

[配置空闲 TCP 的超时设置为负载平衡器](load-balancer-tcp-idle-timeout.md)测试
