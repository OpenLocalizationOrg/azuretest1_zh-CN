---
ms.openlocfilehash: 9cf51fbec0122eadd83a3febe737fa14da813e78
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="可靠的参与者生命周期"
   description="介绍了生命周期和服务结构可靠参与者的垃圾回收"
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


# 演员的生命周期和垃圾回收
参与者被激活时对它进行第一次调用，则停用 （由演员运行时垃圾回收） 如果不用于一段时间。 若要配置此时间段内请参阅部分下面的主角垃圾回收。

在主角激活会发生什么情况？

- 当主角的传入的调用并尚未处于活动状态时，将创建新主角。
- 加载的状态 （如果它是有状态的参与者）
- `OnActivateAsync` 调用方法 （它可以重写在主角实现）。
- 它被添加到活动参与者表格。

使用者停用上会发生什么情况？

- 当演员一段时间没有使用时，它从表中删除的活动参与者。
- `OnDeactivateAsync` 这将清除所有主角的计时器调用方法 （它可以重写在主角实现）。

> [AZURE.TIP] 结构演员运行时发出一些[与演员激活和停用相关的事件](service-fabric-reliable-actors-diagnostics.md#actor-activation-and-deactivation-events)。 它们可用于诊断和性能监控。

## 主角垃圾回收
演员运行时定期扫描一段时间，没有使用过的演员，并停用它们。 它们处于非活动状态后它们可以成为垃圾回收的 CLR。

怎样才能成为"被用于"垃圾回收？

- 接收呼叫。
- `IRemindable.ReceiveReminderAsync` 正被调用 （参与者使用提醒的情况下才适用） 的方法。

值得注意的是否参与者使用的计时器，并且其计时器回调被调用，它将做**不**计为"正在使用"。

之前进入垃圾回收的详细信息，就必须定义以下术语︰

- *扫描间隔*-这是该处演员运行时扫描其活动参与者表可以被垃圾回收器的参与者的时间间隔。 它的默认值为 1 分钟。
- *空闲超时*-这是使用者需处于未使用状态的时间量 （空闲） 之前可以被垃圾回收。 它的默认值为 60 分钟。

通常不需要更改这些默认设置。 但是，如果有必要，这些时间间隔可以更改在该程序集中的所有使用者类型的程序集级别或参与者类型级别使用`ActorGarbageCollection`属性。 下面的示例演示在垃圾收集间隔更改为 HelloActor。

```csharp
[ActorGarbageCollection(IdleTimeoutInSeconds = 10, ScanIntervalInSeconds = 2)]
class HelloActor : Actor, IHello
{
    public Task<string> SayHello(string greeting)
    {
        return Task.FromResult("You said: '" + greeting + "', I say: Hello Actors!");
    }
}
```

若要更改默认值`ActorGarbageCollection`在程序集级别特性，添加下列代码段为`AssemblyInfo.cs`。

```csharp
[assembly: ActorGarbageCollection(IdleTimeoutInSeconds = 10, ScanIntervalInSeconds = 2)]
```

有关其活动的参与者表中为每个参与者，演员运行时跟踪的一段时间它已空闲 （即未使用）。 主角运行时检查每个演员每`ScanIntervalInSeconds`以它是否是垃圾收集并收集它，如果已空闲的`IdleTimeoutInSeconds`。

任何时候使用演员，空闲时间重置为 0。 此后，主角只能是垃圾回收如果再次处于空闲状态为`IdleTimeoutInSeconds`。 请记住，参与者则认为是否两主角接口方法执行操作者提醒回调已使用。 参与者将**不**被视为如果执行其计时器回调，则已使用。

下面的关系图包含一个示例来说明这些概念。

![][1]

本示例假定存在是活动的参与者表中只能有一个活动参与者并且显示该演员的生存期主角方法调用，提醒和计时器的影响。 有关该示例的以下几点是值得一提的是︰

- ScanInterval 和 IdleTimeout 设置为 5 和 10 日分别在示例 （单位无关紧要，不在这里，因为我们的目的只是为了说明这样一个概念）。
- 扫描者进行垃圾回收发生在 T = 0，5，10，15，20，25 所定义的 5 ScanInterval。
- 定期计时器触发 T = 4，8，12，16，20，24，其回调执行。 它不会影响使用者的空闲时间。
- 在 T = 7，一个演员的方法调用重置为空闲时间 0 和延迟主角进行垃圾回收。
- 主角提醒回调在 T = 14 执行并进一步延迟主角进行垃圾回收。
- 在 T = 25 垃圾集合扫描，主角的空闲时间最后超过 10 IdleTimeout 和演员进行垃圾回收。

请注意演员将永远不会执行垃圾回收时它正在执行的方法，不管多少时间花费在执行该方法之一。 如上文所述，使用者接口方法和提醒回调的执行可以防止垃圾回收正在主角的空闲时间重置为 0。 计时器回调的执行无法复位为 0 的空闲时间。 但是，垃圾回收的主角被延迟，直到计时器回调已完成执行。

<!--Image references-->
[1]: ./media/service-fabric-reliable-actors-lifecycle/garbage-collection.png
