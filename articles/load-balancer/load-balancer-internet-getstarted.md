---
ms.openlocfilehash: ae89f00aec88645fd47818e140ef8104261e691c
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---

<properties
   pageTitle="开始面向 Internet 的负载平衡器配置 |Microsoft Azure"
   description="设置第一个面向 Internet 的负载平衡器为您的虚拟机或云服务。 "
   services="load-balancer"
   documentationCenter="na"
   authors="joaoma"
   manager="adinah"
   editor="tysonn" />
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/12/2015"
   ms.author="joaoma" />

# 开始配置面向 Internet 的负载平衡器

> [AZURE.SELECTOR]
- [Azure 经典的步骤](load-balancer-internet-getstarted.md)
- [资源管理器 Powershell 的步骤](load-balancer-arm-powershell.md)

所有租户类型 （IaaS 或 PaaS） 都负载均衡服务在 Microsoft Azure 的工作和所有受支持的操作系统 （Windows 或任何受支持的基于 Linux 的操作系统）。


## 设置虚拟机的面向 Internet 的负载平衡器

为了跨云服务的虚拟机从互联网负载平衡网络通讯量，您必须创建负载平衡的设置。 此过程假定您已创建虚拟机，它们是所有在相同的云服务。

**若要配置的虚拟机的负载平衡设置**

1. 在 Azure 的门户中，**虚拟机**，请单击，然后单击负载平衡组中的虚拟机的名称。
2.  **终结点**，请单击，然后单击**添加**。

4.  在**添加到虚拟机的终结点**页上，单击向右箭头。

4.  在**指定终结点的详细信息**页上︰
    - 在**名称**中，键入该终结点的名称或从常用协议的预定义终结点的列表中选择名称。
    -  在**协议**中，根据需要选择所需类型的终结点，TCP 或 UDP 协议。
    -  在**公共端口和专用端口**中，键入要将虚拟机使用时，根据需要的端口号。 可以使用专用端口和防火墙规则虚拟机重定向通信是适合您的应用程序的方式。 专用端口可以是公有的端口相同。 例如，对于 web (HTTP) 通信的终结点，可以到公用和专用端口分配端口 80。

5.  选择**创建一种负载平衡组**，然后单击向右箭头。

6.  **配置负载平衡设置**页中，在为负载平衡设置，键入一个名称，然后将探测行为的 Azure 负载平衡器的值。
负载平衡器使用探测器来确定负载平衡组中的虚拟机是否可以接收传入通信。

7.  单击复选标记以创建负载平衡的端点。 您将看到的**终结点**页的虚拟机的**负载平衡集名称**列中的**是**。

8.  在门户中，单击**虚拟机**的负载平衡设置的其他虚拟机名称单击**端点**，和，然后单击**添加**。

9.  在**添加到虚拟机的终结点**页上，单击**一个现有的负载平衡集添加终结点**，选择集的名称的负载平衡，然后单击向右箭头。

10. 在**指定终结点的详细信息**页上，键入该终结点的名称，然后单击复选标记。
负载平衡组中的其他虚拟机，请重复步骤 8-10。

您还可以使用下面的 Windows PowerShell cmdlet 配置终结点︰

- 添加 AzureEndpoint

[添加 AzureEndpoint](https://msdn.microsoft.com/library/windowsazure/dn495300)

- 获得 AzureEndpoint

[获得 AzureEndpoint](https://msdn.microsoft.com/library/windowsazure/dn495158)

- 删除 AzureEndpoint

[删除 AzureEndpoint](https://msdn.microsoft.com/library/windowsazure/dn495161)


## 设置为云服务面向 Internet 的负载平衡器


云服务会自动获配一个负载平衡器，可以通过服务模型进行自定义。

您可以利用 Azure SDK 的.NET 2.5 来更新您的云服务。 在[服务定义](https://msdn.microsoft.com/library/azure/gg557553.aspx).csdef 文件进行云服务的终结点设置。

下面的示例演示如何配置云部署的 servicedefinition.csdef 文件︰

检查 snipet 的云部署生成的.csdef 文件，您可以看到外部终结点配置为在端口 10000、 10001 和 10002 使用 HTTP 端口。


    <ServiceDefinition name=“Tenant“>
    <WorkerRole name=“FERole” vmsize=“Small“>
    <Endpoints>
        <InputEndpoint name=“FE_External_Http” protocol=“http” port=“10000“ />
        <InputEndpoint name=“FE_External_Tcp“  protocol=“tcp“  port=“10001“ />
        <InputEndpoint name=“FE_External_Udp“  protocol=“udp“  port=“10002“ />

        <InputEndpointname=“HTTP_Probe” protocol=“http” port=“80” loadBalancerProbe=“MyProbe“ />

        <InstanceInputEndpoint name=“InstanceEP” protocol=“tcp” localPort=“80“>
           <AllocatePublicPortFrom>
              <FixedPortRange min=“10110” max=“10120“  />
           </AllocatePublicPortFrom>
        </InstanceInputEndpoint>
        <InternalEndpoint name=“FE_InternalEP_Tcp” protocol=“tcp“ />
    </Endpoints>
    </WorkerRole>
    </ServiceDefinition>




### 负载平衡器的云服务运行状况检查


以下是健康探测器的一个示例︰

        <LoadBalancerProbes>
        <LoadBalancerProbe name=“MyProbe” protocol=“http” path=“Probe.aspx” intervalInSeconds=“5” timeoutInSeconds=“100“ />
        </LoadBalancerProbes>

负载平衡器将该终结点的信息和探测器可用于查询服务的运行状况的 VM}:80/Probe.aspx 的 http://{DIP 的形式创建一个 URL 的信息。

服务检测到相同的 IP 地址从定期探测。 这是健康探测请求来自主机的节点运行虚拟机的位置。
该服务必须响应以 HTTP 200 状态代码负载平衡器假定该服务是运行状况良好。 任何其他 HTTP 状态代码 (例如 503) 直接采用轮换虚拟机。

该探测器定义还控制探测器的频率。 在我们上面的情况下，负载平衡器探测终结点每隔 5 秒钟之内。 如果没有肯定的答案收到 10 秒 （两个探测器区间），探测器假定下，和虚拟机采取轮换。 同样，如果服务是轮换和接收到的肯定应答，服务会还原为旋转向右走。 如果该服务正常和不正常之间起伏波动，负载平衡器可以决定推迟回旋转服务重新引入，直到已为大量的探测器状态良好。

检查服务定义架构[健全探测器](https://msdn.microsoft.com/library/azure/jj151530.aspx)的详细信息。

## 设置使用 PowerShell 的负载平衡器

在创建虚拟机之后, 可以使用 PowerShell cmdlet 以添加到同一个云服务中的虚拟机的负载平衡器。

在下面的示例中，您将添加称为"webfarm"到云服务终结点"mycloudservice"（或 mycloudservice.cloudapp.net） 的负载平衡器和一个名为 myVM 的虚拟机。 负载平衡器接收通信的端口 80 和负载平衡在端口 8080 上的虚拟机之间的网络通信使用 HTTP。

    Get-AzureVM -ServiceName "mycloudservice" -Name "MyVM" | Add-AzureEndpoint -Name "HttpIn" -Protocol "tcp" -PublicPort 80 -LocalPort 8080 -LBSetName "WebFarm" -ProbePort 80 -ProbeProtocol "http" -ProbePath '/' | Update-AzureVM


## 下一步行动

[开始配置内部负载平衡器](load-balancer-internal-getstarted.md)

[配置负载平衡器分布模式](load-balancer-distribution-mode.md)

[配置负载平衡器的空闲 TCP 超时设置](load-balancer-tcp-idle-timeout.md)

测试
