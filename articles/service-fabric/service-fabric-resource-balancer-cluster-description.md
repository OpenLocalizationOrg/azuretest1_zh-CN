---
ms.openlocfilehash: 8767c34e66c8d38f19e092897099806e2d3e432f
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="资源平衡群集描述"
   description="指定资源平衡器的群集说明"
   services="service-fabric"
   documentationCenter=".net"
   authors="abhic"
   manager="timlt"
   editor=""/>

<tags
   ms.service="Service-Fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="04/27/2015"
   ms.author="abhic"/>

# 群集描述

服务结构资源平衡器提供了几种机制来描述一个群集。 运行时资源平衡器使用这些条信息以确保它将服务放在确保高可用性，同时确保群集资源的最大利用率在群集中运行的服务的方式。 描述了群集的资源平衡器功能是故障域、 升级域、 节点属性和节点的容量。 另外，资源平衡器包含一些可以调整其性能的配置选项。

## 主要概念

### 容错域︰

容错域启用群集管理员定义可能会遇到由于共享物理之间的相关性，如电源和网络来源同时出现故障的物理节点。 容错域通常用于表示这些共享的依赖关系，与更多的节点可能无法在一起故障域树中的较高点相关的层次结构。 下图显示了通过分层故障域数据中心、 机架上，且第一个刀片的顺序组织的多个节点。

![容错域][Image1]

 在运行时，服务结构资源管理器认为在群集故障域，并试图分散给定服务的副本，以使它们在不同故障域。 此过程有助于确保，在任何一个故障域，出现故障时的可用性服务和其状态不会受到威胁。 下图显示一种服务，分散到几个故障域，即使有足够的空间来将其注意力集中在域中只能有一个或两个复制的副本。

![容错域][Image2]

容错域配置群集清单内。 每个节点定义为在一个特定的故障域内。 运行时资源管理器将从所有节点，以开发系统中的所有故障域的完整概述报告结合起来。

### 升级域

升级域是信息的另一条消耗的资源管理器。 升级域，如故障域描述组均已关闭的升级在大约同一时间的节点。 升级域不是分层结构，并可视为标记。

而故障域由群集中的节点的物理布局，通过基于策略的群集管理员确定升级域。 与群集中升级相关的策略。 更升级的域进行升级更精细，通过限制对运行群集和服务的影响，并通过防止任何故障影响大量的服务。 根据其他政策，例如服务结构后升级域服务结构移到下一个域之前应等待的时间更为升级域还可以增加完成群集中的升级所需的时间量。

出于这些原因，资源管理器收集升级域的信息，并传播副本升级域中的群集故障域一样。 升级域可能或不可能与故障域一一对应，但通常不会对应一对一。 下图显示了在以前定义的故障域的多个升级域的层。 资源管理器仍深压浅副本跨域以便无故障或升级域成为集中使用的副本，以确保高可用性服务，而不考虑升级或群集中的故障。

![升级域][Image3]

升级域和故障域均配置作为群集清单，清单中的节点定义的组成部分如下所示︰

``` xml
<Infrastructure>
    <WindowsServer>
      <NodeList>
        <Node NodeTypeRef="NodeType01" IsSeedNode="true" IPAddressOrFQDN="localhost" NodeName="Node1" FaultDomain="fd:/RACK1/BLADE1" UpgradeDomain="ud:/UD1"/>
        <Node NodeTypeRef="NodeType01" IsSeedNode="false" IPAddressOrFQDN="localhost" NodeName="Node2" FaultDomain="fd:/RACK2/BLADE1" UpgradeDomain="ud:/UD2"/>
        <Node NodeTypeRef="NodeType01" IsSeedNode="true" IPAddressOrFQDN="localhost" NodeName="Node3" FaultDomain="fd:/RACK3/BLADE2" UpgradeDomain="ud:/UD3"/>
        <Node NodeTypeRef="NodeType01" IsSeedNode="false" IPAddressOrFQDN="localhost" NodeName="Node4" FaultDomain="fd:/RACK2/BLADE2" UpgradeDomain="ud:/UD1"/>
        <Node NodeTypeRef="NodeType01" IsSeedNode="true" IPAddressOrFQDN="localhost" NodeName="Node5" FaultDomain="fd:/RACK1/BLADE2" UpgradeDomain="ud:/UD2"/>
        </NodeList>
    </WindowsServer>
</Infrastructure>
```
- 在 Azure 部署中，故障域和升级域分配通过 Azure;因此，节点和 Azure 的基础架构选项中的角色的定义不包括故障域或升级域信息。

### 节点属性
节点属性是用户定义的键/值对，给定节点提供额外的元数据。 节点属性的示例包括是否该节点拥有的硬盘或视频卡，其硬盘、 内核和其他物理属性中的心轴数。

节点属性还可以用于指定更抽象的属性，以帮助在放置策略决策。 例如，群集中的多个节点可以分配"color"作为一种分割到不同的节群集。 此代码示例显示节点属性定义的节点的群集清单通过作为然后可以应用于群集中的多个节点的节点类型定义的一部分。

节点名称、 节点类型、 FaultDomain 和 UpgradeDomain 放置属性具有默认值。  服务结构将为您自动提供默认值，以便您可以在创建服务时使用它们。 用户不应具有这些相同的名称来指定自己位置的属性。

``` xml
<NodeTypes>
  <NodeType Name="NodeType1">
    <PlacementProperties>
      <Property Name="HasDisk" Value="true"/>
      <Property Name="SpindleCount" Value="4"/>
      <Property Name="HasGPU" Value="true"/>
      <Property Name="NodeColor" Value="blue"/>
      <Property Name="NodeName" Value="Node1"/>
      <Property Name="NodeType" Value="NodeType1"/>
      <Property Name="FaultDomain" Value="fd:/RACK1/BLADE1"/>
      <Property Name="UpgradeDomain" Value="ud:/UD1"/>
    </PlacementProperties>
  </NodeType>
    <NodeType Name="NodeType2">
    <PlacementProperties>
      <Property Name="HasDisk" Value="false"/>
      <Property Name="SpindleCount" Value="-1"/>
      <Property Name="HasGPU" Value="false"/>
      <Property Name="NodeColor" Value="green"/>
    </PlacementProperties>
  </NodeType>
</NodeTypes>
```

运行时资源平衡器使用节点属性信息来确保需要特定功能服务置于相应的节点。

### 节点的容量
节点的容量是定义名称和特定的资源数量的键/值对的特定节点具有可供消耗。 它具有名为"MemoryInMb"指标的容量和它有 2048 MB 的可用内存在默认情况下，该代码示例演示一个节点。 通过群集清单，很像节点属性定义容量。

``` xml
<NodeType Name="NodeType03">
  <Capacities>
    <Capacity Name="MemoryInMB" Value="2048"/>
    <Capacity Name="DiskSpaceInGB" Value="1024"/>
  </Capacities>
</NodeType>
```
![节点的容量][Image4]

由于在一个节点运行的服务可更新报告负载通过其容量需求，资源平衡器还会检查定期节点是否在或任何其度量标准的容量。 如果是这样，资源平衡器可以移动服务到更少的加载节点来减少资源争用和提高整体的性能和利用率。

请注意，尽管还可能的节点类型属性一节中列出给定的指标，最好它列为产能，如果它可以在运行时使用该节点的属性。 要列出其另外作为属性使查询的节点放置约束，如果资源在运行时由其他服务可能是必需的服务取决于"最低硬件要求"。 建议还包括，它作为产这样资源平衡器可以执行其他操作。

当都会创建新服务，的使用现有节点的容量和现有的消耗有关的信息服务，以确定是否有足够的服务结构群集资源平衡器中放置整个新服务的能力。 如果产能不足然后创建服务请求被拒绝的错误消息，指出没有剩余的不足群集容量。


### 资源平衡器配置

在群集清单，以下几个不同的配置值，用于定义资源平衡器的总体行为︰

- 平衡的阈值控制如何平衡群集可能会变得对某个特定指标之前资源平衡的反应。 平衡阀值是允许存在，才能平衡群集资源平衡器的用法使用和最小限度地使用节点之间的最大比率。

下图显示两个示例，其中给定的指标，要平衡阈值是 10。

![平衡阈值][Image5]

请注意，这次上一个节点的"使用率"不会不考虑到由容量的节点，该节点的大小，但绝对只使用的当前报告指定度量节点上。

该代码示例演示平衡指标阈值配置每个群集清单中的 FabricSettings 元素通过指标。

``` xml
<FabricSettings>
  <Section Name="MetricBalancingThresholds">
    <Parameter Name="MetricName" Value="10"/>
  </Section>
</FabricSettings>
```

- 活动阈值作为一个门上资源平衡器通过限制资源平衡器对存在大量绝对负载时的反应的情况下运行的频率。 这种方式，如果群集不是很忙的特定指标，资源平衡器不运行即使该少量的指标是非常平衡的群集中。 该措施是为了防止浪费资源的重新平衡做出实质性小增益为群集。 下图显示了平衡阈值的跃点数为 4 和活动阈值是 1536年。

![活动阈值][Image6]还注意活动和平衡阈值必须超过同一度量导致资源平衡运行。 触发两个独立的指标不会导致资源平衡运行。

该代码示例演示，一样平衡阈值，活动阈值配置每个群集清单中的 FabricSettings 元素通过指标。

``` xml
<FabricSettings>
  <Section Name="MetricActivityThresholds">
    <Parameter Name="MetricName" Value="1536"/>
  </Section>
</FabricSettings>
```

- PLBRefreshInterval – 控制资源平衡器频率扫描的信息，它必须检查约束冲突。 约束冲突包括放置约束，不能满足，服务实例或副本，通过一些指标，和重载故障或升级域，容量节点及其所需数少于或群集使用群集中的节点上的平衡和活动阈值和负载的当前视图的不平衡。 以秒为单位，定义的刷新间隔，默认值为 1。 频率约束检查和放置或者受 （MinConstraintCheckInterval 和 MinPlacementInterval） 的两个新时间间隔，如果定义了新的参数，PLBRefreshInterval 将不会使用并不能定义。

- MinConstraintCheckInterval-控制资源平衡器频率扫描的信息，它必须检查约束冲突。 约束冲突包括放置约束，不能满足，服务实例或副本，通过一些指标，和重载故障或升级域，容量节点及其所需数少于或群集使用群集中的节点上的平衡和活动阈值和负载的当前视图的不平衡。 约束检查间隔是以秒为单位定义的。 如果未定义约束检查的时间间隔，则其默认值将等于 PLBRefreshInterval （同时不能指定这两个值） 的值。

- MinPlacementInterval – 控制资源平衡器检查如果有新的实例或需要放置的副本的频率，并尝试将它们放置。 以秒为单位定义的放置时间间隔。 如果未定义放置间隔，默认值为的 PLBRefreshInterval （同时不能指定这两个值） 的值相等。

- MinLoadBalancingInterval – 建立最小资源平衡轮之间的时间量。 如资源平衡器基于信息从最后一个 PLBRefreshInterval 时间间隔内执行扫描操作，它不会再次运行为至少同样多的时间。 以秒为单位，定义，默认值为 5。

请注意，这些值是雄心勃勃的但该负载平衡整体时，才出现在群集中的平衡和活动阈值满足给定指标的。 该代码示例演示是否准确且处于活动状态的负载平衡不需要特定的群集，这些值可以进行以下配置中的 FabricSettings 元素通过不太积极。 在此示例配置，两个约束的最小时间距离检查时 10 秒，放置 5 秒钟负载平衡会每隔 5 分钟。 请注意在这种情况下未定义 PLBRefreshInterval。

``` xml
<Section Name="PlacementAndLoadBalancing">
  <Parameter Name="MinPlacementInterval" Value="5" />
  <Parameter Name="MinConstraintChecknterval" Value="10" />
  <Parameter Name="MinLoadBalancingInterval" Value="600" />
</Section>
```

在下面的示例配置第二个我们有 PLBRefreshInterval 和 MinLoadBalancingInterval 定义。 由于 PLBRefreshInterval 设置为 2 秒，MinPlacementInterval 和 MinConstraintCheckInterval 将有它们的值设置为 2 秒也。

``` xml
<Section Name="PlacementAndLoadBalancing">
  <Parameter Name="PLBRefreshInterval" Value="2" />
  <Parameter Name="MinLoadBalancingInterval" Value="600" />
</Section>
```

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## 下一步行动

详细信息︰[资源平衡器体系结构](service-fabric-resource-balancer-architecture.md)


[Image1]: media/service-fabric-resource-balancer-cluster-description/FD1.png
[Image2]: media/service-fabric-resource-balancer-cluster-description/FD2.png
[Image3]: media/service-fabric-resource-balancer-cluster-description/UD.png
[Image4]: media/service-fabric-resource-balancer-cluster-description/NC.png
[Image5]: media/service-fabric-resource-balancer-cluster-description/Config.png
[Image6]: media/service-fabric-resource-balancer-cluster-description/Thresholds.png
 