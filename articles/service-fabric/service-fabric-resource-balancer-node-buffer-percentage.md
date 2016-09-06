---
ms.openlocfilehash: 172a000a83f521d60f72272617ba80aa6f9bc3da
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="节点缓冲区百分比"
   description="资源平衡器中的节点缓冲区百分比的角色概述"
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

# 节点缓冲区百分比概述

目前，客户可以指定节点的容量限制为约束资源平衡尊重基于约束的优先级。 如果容量约束的优先级值较高 （节点容量不能突破） 并且如果利用率高、 群集节点，它可以发生故障转移不直接，或者该节点容量受到破坏的情况。

扫描问题发生与辅助副本的节点是否与主副本的节点时节点容量附近出现故障。 在这种情况下，如果主要负荷大于辅助负载，辅助副本不能立即升级，而无过量的节点或复制的副本。

有主动封装逻辑运行，更多的群集节点将节点容量限制附近。 节点缓冲区百分比是一种功能，可防止更多的故障转移时间或节点故障转移期间，过量通过为客户指定的节点应该保留免费的百分比的可能性。 不应将新的服务的副本添加到节点缓冲区空间但资源平衡器应该能够使用总节点容量 （会计缓冲区空间） 故障切换并添加缺少的副本。

可以使用此功能，即使万一当主动规格，包装功能处于关闭状态。

此代码示例显示节点缓冲区百分比指标配置每个群集清单中的 FabricSettings 元素通过指标。

``` xml
<FabricSettings>
  <Section Name=" NodeBufferPercentage">
    <Parameter Name="MetricName" Value="0.1"/>
  </Section>
</FabricSettings>

```

值名称"MetricName"表示，10%的指标"MetricName"的每个节点容量应该保留免费使用指标的 0.1。

如果本部分中没有指定值，则将使用默认值为 0。

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## 下一步行动

详细信息︰[资源平衡器体系结构](service-fabric-resource-balancer-architecture.md)
 