---
ms.openlocfilehash: 0983ce3d4a9971fa3ddb757d603f658fa9a9162a
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="可靠的演员重入"
   description="可重入性介绍的服务结构可靠参与者"
   services="service-fabric"
   documentationCenter=".net"
   authors="jessebenson"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/05/2015"
   ms.author="amanbha"/>


# 可靠的操作者重入
默认情况下，结构参与者允许逻辑调用上下文基于可重入性。 这样可重入的如果它们是相同的调用上下文链中的参与者。 例如，参与者 A 将消息发送给参与者 B 将消息发送到使用者 c。作为消息处理，如果演员 C 调用一个演员的一部分，该消息是重入的因此将允许。 不同的调用上下文的一部分的任何其他邮件将被阻止在演员的直到它完成处理。

参与者想要禁止基于上下文的可重入性可以通过修饰主角禁用它的逻辑调用类的`ReentrantAttribute(ReentrancyMode.Disallowed)`。

```csharp
public enum ReentrancyMode
{
    LogicalCallContext = 1,
    Disallowed = 2
}
```

下面的代码演示使用者类，将重新进入模式设置为`ReentrancyMode.Disallowed`。 在这种情况下如果参与者发送重入消息到另一个使用者类型的异常`FabricException`将引发。

```csharp
[Reentrant(ReentrancyMode.Disallowed)]
class VoicemailBoxActor : Actor<VoicemailBox>, IVoicemailBoxActor
{
    ...
}
```
