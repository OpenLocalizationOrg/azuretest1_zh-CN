---
ms.openlocfilehash: e2222b84f3ef06ac5c3cbd16e8064ea2913f9e10
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="管理︰ 负载平衡器分布模式 （源 IP 关系）"
   description="Azure 负载平衡器分布模式下的管理功能" 
   services="virtual-network" 
   documentationCenter="" 
   authors="telmosampaio" 
   manager="carolz" 
   editor=""
   />

<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/21/2015"
   ms.author="telmos"
   />
   
# 管理虚拟网络︰ 负载平衡器分布模式 （源 IP 关系）
**源 IP 相似性**（也称为**会话关联**或**客户端的 IP 相似性**） Azure 负载平衡器分布模式，将从一个客户端连接到单个 Azure 托管服务器，而不是分发不同的 Azure 托管服务器 （默认负载平衡器行为） 动态地向每个客户端连接。

使用源 IP 相似性，Azure 负载平衡器可以配置使用二元组合 （来源 IP、 目标 IP） 或三元组合 （来源 IP、 目标 IP 协议） 来映射通信可用 Azure 托管服务器的池中。 当使用源 IP 相似性，发起来自同一个客户端计算机的连接由一个 DIP 终结点 （一台 Azure 托管服务器） 处理。

## 服务来源

源 IP 关系解决了以前[在 Azure 负载平衡器和 RD 网关技术支持 (DOC) 之间的不兼容性](http://go.microsoft.com/fwlink/p/?LinkId=517389)。

## 实现

源 IP 相似性可以配置为︰ 

* [虚拟机的终结点](../virtual-machines/virtual-machines-set-up-endpoints.md)
* [负载平衡的端点集](../load-balancer/load-balancer-overview.md)
* [Web 角色](http://msdn.microsoft.com/library/windowsazure/ee758711.aspx)
* [工作角色](http://msdn.microsoft.com/library/windowsazure/ee758711.aspx)

## 方案
1. 使用单一的云服务的远程桌面网关群集
2. 媒体传 (即 UDP 数据，TCP 控制)
  * 客户端初始化到 Azure 承载负载平衡公共 IP 地址的 TCP 会话
  * 客户端请求定向到 DIP 通过负载平衡器;此通道保持活动连接的运行状况监视
  * 客户端初始化到 Azure 承载负载平衡公共 IP 地址的 UDP 会话
  * Azure 负载平衡器将请求指定给同一个 DIP 终结点为 TCP 连接
  * 客户端将使用 UDP 吞吐量媒体上载同时控制通道通过 TCP 的可靠性
  
## 注意事项
* 如果负载平衡设置发生了更改 （例如添加或删除虚拟机），客户端通道分发时都重新计算;可能由另一台服务器处理来自现有客户端的新连接，比最初使用了
* 使用源 IP 关系可能会导致通信等分布在 Azure 托管服务器
* 路由通过代理服务器通信的客户端可能被视为单个客户端 Azure 负载平衡器

## PowerShell 示例
请下载[最新的 Azure PowerShell 释放](https://github.com/Azure/azure-sdk-tools/releases)达到最佳效果。

### Azure 的终结点添加到虚拟机并设置负载平衡器分布模式

    Get-AzureVM -ServiceName "mySvc" -Name "MyVM1" | Add-AzureEndpoint -Name "HttpIn" -Protocol "tcp" -PublicPort 80 -LocalPort 8080 –LoadBalancerDistribution “sourceIP”| Update-AzureVM  

    Get-AzureVM -ServiceName "mySvc" -Name "MyVM1" | Add-AzureEndpoint -Name "HttpIn" -Protocol "tcp" -PublicPort 80 -LocalPort 8080 â€“LoadBalancerDistribution â€œsourceIPâ€�| Update-AzureVM  

LoadBalancerDistribution 可以将设置为 sourceIP 2 元 （来源 IP、 目标 IP） 负载平衡、 sourceIPProtocol 3 元组 （源 IP、 目标 IP 协议） 负载平衡，或无，如果要使用默认行为 （五元负载平衡）。  

### 获取终结点负载平衡器分布模式配置
    PS C:\> Get-AzureVM –ServiceName "mySvc" -Name "MyVM1" | Get-AzureEndpoint
    
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

如果不存在 LoadBalancerDistribution 元素 Azure 负载平衡器使用默认值 5 元算法

### 设置负载平衡的端点集分布模式

    Set-AzureLoadBalancedEndpoint -ServiceName "MyService" -LBSetName "LBSet1" -Protocol tcp -LocalPort 80 -ProbeProtocolTCP -ProbePort 8080 –LoadBalancerDistribution "sourceIP"

    Set-AzureLoadBalancedEndpoint -ServiceName "MyService" -LBSetName "LBSet1" -Protocol tcp -LocalPort 80 -ProbeProtocolTCP -ProbePort 8080 â€“LoadBalancerDistribution "sourceIP"
    
如果终结点是负载平衡的端点集的一部分，必须在负载平衡的端点集设置分布模式。

## 云服务示例

您可以利用 Azure SDK.net 来更新您的云服务

在.csdef 进行的云服务的终结点设置。 为了更新云服务部署负载平衡器分布模式，部署升级是必需的。

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
    
## API 示例

开发人员可以配置负载平衡器分发使用服务管理 API。  请确保要添加 x-ms-版本标头设置为版本 2014年-09-01 或更高。

### 更新部署中指定的负载平衡设置的配置

#### 请求

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

#### 响应

    HTTP/1.1 202 Accepted 
    Cache-Control: no-cache 
    Content-Length: 0 
    Server: 1.0.6198.146 (rd_rdfe_stable.141015-1306) Microsoft-HTTPAPI/2.0 
    x-ms-servedbyregion: ussouth2 
    x-ms-request-id: 9c7bda3e67c621a6b57096323069f7af 
    Date: Thu, 16 Oct 2014 22:49:21 GMT
 
