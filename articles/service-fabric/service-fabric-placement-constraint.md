---
ms.openlocfilehash: 5a5b8b55a18264a7caab776e955d453cca70c13e
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="服务结构群集业务流程放置约束"
   description="放置约束服务结构中的概念性概述"
   services="service-fabric"
   documentationCenter=".net"
   authors="GaugeField"
   manager="timlt"
   editor=""/>

<tags
   ms.service="Service-Fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="03/17/2015"
   ms.author="abhic"/>

# 放置约束概述

服务结构使开发人员能够约束服务到满足特定条件的节点的复制副本的位置。 这些条件是通过使用适当的服务上下文特定值计算的布尔表达式表示的。


## Capabilities
通过使用放置约束，您可以︰

- 将不同类型的不同类型的节点通过定义节点属性的节点上的服务的限制。

- 将某些约束应用于主副本而不是辅助副本


## 主要概念
NodeProperty-用户或系统定义映射从一个字符串的值会随每个节点，即 节点名称。


<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## 下一步行动

详细信息︰[应用程序方案](../service-fabric-application-scenarios)。
 