---
ms.openlocfilehash: cc853cacf3dfa17e7dd6d2088fc6e72988f40fcb
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="主动预防性的度量标准包装"
   description="在资源平衡器使用主动预防性的规格，包装的概述"
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

# 主动预防性的度量标准包装

公共服务结构资源平衡器配置是实现每个节点上的每个指标按相等的利用率。 资源平衡器也是在寻找一个地方服务的新服务请求到达时的费用。 如果没有足够的可用空间来放置新服务副本 （所有复制副本的所有服务的分区），资源平衡器将尝试为其创建空间，通过移动现有的工作负载。 虽然这是非常正常的这取决于完整群集和负载碎片程度，花费的时间。

例如，如果 decently 利用群集，并且客户想要添加默认大负载 （如一个或多个指标的最大节点容量） 的新服务可能出现资源平衡器需要将多个复制副本移动以放置新的服务。 此外，如果服务是有状态且大，执行必要的移动可能需要一些时间因为需要复制的数据。 这些问题都可以延长新服务创建时间。 虽然通常服务应容许的偶尔慢的创建时间，一些工作负载不太能容忍，要创建越早越好，这意味着，在需要确保群集"碎片整理"资源平衡器一种稳定状态以提供更大机会，没有足够的可用空间，为新的工作负载。

主动预防性规格，包装 （也 作为资源平衡的平衡阶段，目标是最小化包装到更少的节点的工作负载的服务创建时间而不是将它们分发过程平衡的一部分运行碎片整理） 机制。 当指标配置进行碎片整理资源平衡的目标是实现最大平均值的标准偏差，而不是最小平均标准偏差时使用平衡。

最大偏差，与资源平衡器将尝试尽可能在一些节点上放置尽可能多的服务，同时保持尽可能的多个节点为空。 此外，将新服务的基本约束之一是复制副本不能为同一个升级域或故障域中。 目标是能够快速添加新的服务，如的目标资源平衡器应升级域和故障域 （服务加载每个升级的域故障域的总和） 的负载分布的最小标准偏差。 结果将是相同的每个升级的域故障域的可用空间量。 碎片整理还将考虑所有其他约束关系、 放置约束和节点的容量指标等系统中。

## 资源平衡器的群集配置
在群集清单中，下列定义规格包装的整体行为的多个不同的配置值功能下资源平衡器︰

### DefragmentationMetrics – 资源平衡器应考虑执行主动的装箱碎片的指标。

应在此列表中指定所有已配置的衡量标准 （就像在活动中和平衡阈值列表）。 如果指定度量值"true"，将被视为一个碎片整理指标中，但带有值"false"（或者如果没有指定此列表中） 可能不会进行碎片整理。

``` xml
<FabricSettings>
  <Section Name="DefragmentationMetrics">
    <Parameter Name="MetricName1" Value="true"/>
    <Parameter Name="MetricName2" Value="false"/>
  </Section>
</FabricSettings>
```

### BalacingThreshholds

平衡的阈值控制方式零碎群集可能会变得在某个特定指标方面资源平衡器运行平衡阶段 （这将公制封装逻辑） 之前。 如果如碎片整理指标，被视为指标，平衡阈值是用法使用和最小限度地使用每个资源平衡器允许它 does 群集中碎片整理之前存在的升级或故障域节点之间的最小比例。 如果任何升级或故障域具有此提供小于阈值，将做碎片整理阶段。

下图显示两个示例，其中的平衡阈值为给定的指标是 10。

![平衡阈值][Image1]

请注意，这次上一个节点的"使用率"不会不考虑到由容量的节点，该节点的大小，但绝对只使用的当前报告指定度量节点上。

如果没有指定度量，则默认值为 1。 在这种情况下，直到群集具有至少一个空节点和每个故障域升级的每个域中，则将执行碎片整理。

值为 0 表示不考虑此度量平衡阶段--或者如果不考虑进行碎片整理过程。

该代码示例演示平衡指标阈值配置每个群集清单中的 FabricSettings 元素通过指标。

``` xml
<FabricSettings>
  <Section Name="MetricBalancingThresholds">
    <Parameter Name="MetricName" Value="10"/>
  </Section>
</FabricSettings>
```

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## 下一步行动

详细信息︰[资源平衡器体系结构](service-fabric-resource-balancer-architecture.md)

[Image1]: media/service-fabric-resource-balancer-proactive-metric-packing/PMP.png
 