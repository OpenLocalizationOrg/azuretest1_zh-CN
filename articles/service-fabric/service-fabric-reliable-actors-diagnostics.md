---
ms.openlocfilehash: 1e115aa2c6e252e2f5046b2c0b233ebbfe856650
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="可靠的演员诊断和性能监视"
   description="本文介绍了诊断和性能监视功能在可靠者运行时，包括事件和性能计数器由它发出。"
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
   ms.author="abhisram"/>

# 诊断和可靠的参与者的性能监视
可靠的演员运行时发出[EventSource](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.aspx)事件和[性能计数器](https://msdn.microsoft.com/library/system.diagnostics.performancecounter.aspx)提供运行时如何运行的见解和帮助进行故障排除和性能监控。

## EventSource 事件
可靠的演员运行时的 EventSource 名为"Microsoft ServiceFabric 演员"。 使用者应用程序要[在 Visual Studio 中调试](service-fabric-debugging-your-application.md)时，此事件源的事件将出现在[诊断事件](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md#view-service-fabric-system-events-in-visual-studio)窗口。

服务结构还提供定向到[应用程序理解](http://azure.microsoft.com/services/application-insights/)这些事件的选项。 有关这方面的详细信息，请参阅[应用程序理解安装服务结构](service-fabric-diagnostics-application-insights-setup.md)上的文章。

工具和技术，帮助收集和/或查看 EventSource 事件中的其他示例是[PerfView](http://www.microsoft.com/download/details.aspx?id=28567) [Azure 诊断](../cloud-services-dotnet-diagnostics.md)、[语义的记录](https://msdn.microsoft.com/library/dn774980.aspx)， [Microsoft TraceEvent 库](http://www.nuget.org/packages/Microsoft.Diagnostics.Tracing.TraceEvent)。

### 关键字
属于可靠的演员 EventSource 的所有事件都都与一个或多个关键字相关联。 这使筛选收集的事件。 下面的关键字位的定义︰

|位|说明|
|---|---|
|0x1|设置汇总结构演员运行时操作的重要事件|
|0x2|事件描述主角方法调用的一组。 有关详细信息，请参阅[关于演员的介绍性主题](service-fabric-reliable-actors-introduction.md#actors)。|
|0x4|一套为主角的状态相关的事件。 有关详细信息，请参见主题上[有状态的参与者](service-fabric-reliable-actors-introduction.md#stateful-actors)|
|0x8|一套与主角中打开基于并发相关的事件。 有关详细信息，请参阅对[并发性](service-fabric-reliable-actors-introduction.md#concurrency)主题。|

## 性能计数器
可靠的演员运行时定义以下性能计数器类别。

|类别|说明|
|---|---|
|服务结构主角|特定于服务结构参与者的计数器。 如所用的时间来保存参与者状态。|
|服务结构主角方法|由服务结构参与者实现的方法的特定的计数器。 如频率使用者将调用方法。|

每个上述类别都有一个或多个计数器。

默认情况下，在 Windows 操作系统中可用的[Windows 性能监视器](https://technet.microsoft.com/library/cc749249.aspx)应用程序可以用于收集和查看性能计数器数据。 [Azure 诊断](../cloud-services-dotnet-diagnostics.md)是另一种方法收集性能计数器数据并将其上载到 Azure 表。

### 性能计数器实例名称
有大量的服务使用者或参与者服务分区的群集将有大量的主角性能计数器实例。 性能计数器实例名称有助于标识与性能计数器实例的特定[分区](service-fabric-reliable-actors-platform.md#service-fabric-partition-concepts-for-actors)和滑稽演员方法 （如果适用）。

#### `Service Fabric Actor` 类别
该类别为`Service Fabric Actor`，计数器实例名称是以下面的格式︰

`ServiceFabricPartitionID_ActorsRuntimeInternalID`

*ServiceFabricPartitionID* -是性能计数器实例相关联的服务结构分区 id 的字符串表示形式。 分区 ID 是一个 GUID，并使用生成的字符串表示形式[`Guid.ToString`](https://msdn.microsoft.com/library/97af8hh4.aspx)格式说明符"D"的方法。

*ActorRuntimeInternalID* -是由其内部使用的结构者运行时生成一个 64 位整数的字符串表示形式。 这将包括在性能计数器实例名称，以确保其唯一性并避免与其他性能计数器实例名称冲突。 用户不应尝试解释性能计数器实例名称的这一部分。

以下是属于服务结构使用者类别的计数器的计数器实例名称的示例︰

`2740af29-78aa-44bc-a20b-7e60fb783264_635650083799324046`

在上例中，`2740af29-78aa-44bc-a20b-7e60fb783264`是服务结构分区 ID 的字符串表示形式和`635650083799324046`是为运行时的内部生成的 64 位 ID 使用。

#### `Service Fabric Actor Method` 类别
该类别为`Service Fabric Actor Method`，计数器实例名称是以下面的格式︰

`MethodName_ActorsRuntimeMethodId_ServiceFabricPartitionID_ActorsRuntimeInternalID`

*方法名称*-是主角方法性能计数器实例相关联的名称。 方法名称的格式是基于平衡的名称与在 Windows 上的性能计数器实例名称的最大长度约束的可读性结构演员运行时中的某些逻辑来确定的。

*ActorsRuntimeMethodId* -是一个 32 位整数，它由其内部使用的结构者运行时生成的字符串表示形式。 这将包括在性能计数器实例名称，以确保其唯一性并避免与其他性能计数器实例名称冲突。 用户不应尝试解释性能计数器实例名称的这一部分。

*ServiceFabricPartitionID* -是性能计数器实例相关联的服务结构分区 id 的字符串表示形式。 分区 ID 是一个 GUID，并使用生成的字符串表示形式[`Guid.ToString`](https://msdn.microsoft.com/library/97af8hh4.aspx)格式说明符"D"的方法。

*ActorRuntimeInternalID* -是由其内部使用的结构者运行时生成一个 64 位整数的字符串表示形式。 这将包括在性能计数器实例名称，以确保其唯一性并避免与其他性能计数器实例名称冲突。 用户不应尝试解释性能计数器实例名称的这一部分。

以下是属于服务结构主角 Method 类别的计数器的计数器实例名称的示例︰

`ivoicemailboxactor.leavemessageasync_2_89383d32-e57e-4a9b-a6ad-57c6792aa521_635650083804480486`

在上例中，`ivoicemailboxactor.leavemessageasync`是方法名、`2`是生成的运行时的内部使用的 32 位 ID`89383d32-e57e-4a9b-a6ad-57c6792aa521`是服务结构分区 ID 的字符串表示形式和`635650083804480486`是 64 位 ID 生成的运行时内部使用。

## 事件和性能计数器的列表

### 主角方法事件和性能计数器
可靠的演员运行时发出与[演员方法](service-fabric-reliable-actors-introduction.md#actors)相关的下列事件。

|事件名称|事件 Id|Level|关键字|说明|
|---|---|---|---|---|
|ActorMethodStart|7|详细|0x2|主角运行时将调用者的方法|
|ActorMethodStop|8|详细|0x2|主角方法已完成执行，即返回运行时的主角方法的异步调用和演员方法返回该任务已完成。|
|ActorMethodThrewException|9|警告|0x3|主角方法运行时的主角方法的异步调用过程中或在执行任务过程中返回一个演员方法，在执行期间引发了异常。 此事件表明某种需要调查的参与者代码中的失败。|

可靠的演员运行时将发布下列性能计数器与演员方法的执行。

|类别名称|计数器名称|说明|
|---|---|---|
|服务结构主角方法|调用/秒|每秒调用使用者服务方法的次数|
|服务结构主角方法|每次调用的平均毫秒|以毫秒为单位执行的主角服务方法所用时间|
|服务结构主角方法|每秒引发的异常|数次主角服务方法异常 / 秒|

### 并发事件和性能计数器
可靠的演员运行时发出以下与[并发](service-fabric-reliable-actors-introduction.md#concurrency)相关的事件。

|事件名称|事件 Id|Level|关键字|说明|
|---|---|---|---|---|
|ActorMethodCallsWaitingForLock|12|详细|0x8|演员在每个新打开的开头写入此事件。 它包含挂起的等待获取每个主角锁定强制关闭基于并发使用者调用的次数。|

可靠的演员运行时将发布下列性能计数器与并发相关。

|类别名称|计数器名称|说明|
|---|---|---|
|服务结构主角|# 正在等待使用者锁定使用者调用的|等待获取每个主角锁定强制关闭基于并发的调用数挂起的主角。|

### 主角状态管理事件和性能计数器
可靠的演员运行时发出与[参与者状态管理](service-fabric-reliable-actors-introduction.md#actor-state-management)相关的下列事件。

|事件名称|事件 Id|Level|关键字|说明|
|---|---|---|---|---|
|ActorSaveStateStart|10|详细|0x4|主角运行库即将保存参与者状态。|
|ActorSaveStateStop|11|详细|0x4|主角运行时已完成保存操作者状态。|

可靠的演员运行时将发布下列性能计数器与参与者状态管理。

|类别名称|计数器名称|说明|
|---|---|---|
|服务结构主角|平均每毫秒保存状态操作|以毫秒为单位保存参与者状态所需时间|

### 与无状态使用者实例相关的事件
可靠的演员运行时发出与[无状态使用者实例](service-fabric-reliable-actors-platform.md#service-fabric-partition-concepts-for-stateless-actors)相关的下列事件。

|事件名称|事件 Id|Level|关键字|说明|
|---|---|---|---|---|
|ServiceInstanceOpen|3|信息|0x1|无状态的参与者打开实例。 这意味着，可以在此分区的主角创建此实例内 (和其他实例可能太)。|
|ServiceInstanceClose|4|信息|0x1|无状态的参与者实例关闭。 这意味着此分区的主角将无法再创建此实例内。 任何新的请求将不传递到已创建此实例中的主角。 在正在进行的任何请求执行完成后，参与者将被销毁。|

### 与有状态的主角副本相关的事件
可靠的演员运行时发出与[有状态的主角副本](service-fabric-reliable-actors-platform.md#service-fabric-partition-concepts-for-stateful-actors)相关的以下事件。

|事件名称|事件 Id|Level|关键字|说明|
|---|---|---|---|---|
|ReplicaChangeRoleToPrimary|1|信息|0x1|有状态的参与者更改副本为主要的角色。 这意味着，将在此副本中创建此分区的主角。|
|ReplicaChangeRoleFromPrimary|2|信息|0x1|状态的参与者更改副本到非主要的角色。 这意味着在此复制副本无法再创建此分区的主角。 任何新的请求将不传递到已创建此复制副本中的主角。 在正在进行的任何请求执行完成后，参与者将被销毁。|

### 主角激活和停用事件
可靠的演员运行时发出以下事件相关[主角激活和停用](service-fabric-reliable-actors-lifecycle.md)。

|事件名称|事件 Id|Level|关键字|说明|
|---|---|---|---|---|
|ActorActivated|5|信息|0x1|参与者已被激活。|
|ActorDeactivated|6|信息|0x1|参与者已被停用。|
