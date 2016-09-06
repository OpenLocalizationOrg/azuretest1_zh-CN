---
ms.openlocfilehash: bdb75a5978685a7d0bc322cef939827687382ff2
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="正在运行混乱测试。"
   description="这篇文章谈到由 Microsoft 附带的预存的服务结构方案。"
   services="service-fabric"
   documentationCenter=".net"
   authors="anmolah"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/26/2015"
   ms.author="anmola"/>

# 可测试性方案
大型分布式的系统如云基础架构是天生不可靠。 服务结构为开发人员能够编写在不可靠的基础架构上运行的服务。 为了编写高质量的服务，开发人员需要能够引起这种不可靠的基础结构来测试他们的服务的稳定性。 服务结构使开发人员能够促使测试服务中存在的故障的容错操作。 但是，目标模拟的故障才会您目前为止。 要使测试更多的服务结构附带预先固定测试方案。 方案模拟连续交叉存取的故障，宽容和正常的在整个群集通过较长时间。 一旦配置了速度和故障的种类，它运行作为客户端工具，通过 C# Api 或 PowerShell，生成错误在群集和您的服务。 作为可测试性功能的一部分我们能够提供以下方案。

1.  混乱的测试
2.  故障转移测试

## 混乱的测试
群集内的整个服务结构，混乱情形生成错误。 该方案压缩错误通常出现在数月或数年乃至几个小时。 交错执行故障高故障率的组合查找角情况下，否则错过它。 这将导致该服务的代码质量显著提高。

### 混乱的测试中模拟的故障
 - 重新启动的节点
 - 重新启动已部署的代码包的
 - 删除复制副本
 - 重启的副本
 - （可选） 的主副本移动
 - 移动的辅助副本 （可选）

为指定的时间段，混乱测试运行故障和群集验证的多个迭代。 达到稳定状态的群集和验证成功所花费的时间也是可配置的。 我们命中一个群集验证失败时，该方案将会失败。 例如，请考虑设置为运行的 1 小时最大 3 的并发故障与测试。 测试将促使 3 的错误，然后验证群集运行状况。 测试将循环上一步，直到群集将变得不正常或 1 小时为止。 如果任何迭代中群集变得不正常，即不在配置的时间内稳定，测试将失败的异常。 此异常指示内容有问题，需要进一步调查。 在当前的形式测试混乱测试故障生成引擎引发仅安全故障。 这意味着，在没有外部故障的仲裁或数据的丢失将永远不会发生。

### 重要的配置选项
 - **TimeToRun**︰ 总成功完成之前将运行测试的时间。 测试可以更早完成而不会验证失败。
 - **MaxClusterStabilizationTimeout**︰ 群集变得健康测试失败前等待的时间的最大金额。 执行检查的目标副本群集运行状况是确定，服务健康是否确定，是否设置实现的服务分区和 InBuild 的复制副本的大小。
 - **MaxConcurrentFaults**︰ 在每个迭代中引发并发错误的最大数目。 数字越大更大因此产生更复杂的故障切换和切换组合中的测试。 测试可保证，在没有外部故障的存在不是仲裁或数据丢失，无论多高的这种配置是。
 - **EnableMoveReplicaFaults**︰ 启用或禁用故障导致的主要或辅助副本移动。 默认情况下，这些错误将被禁用。
 - **WaitTimeBetweenIterations**︰ 即后故障一轮迭代和相应验证之间等待的时间量。

### 如何运行混乱测试
C# 示例

```csharp
// Add a reference to System.Fabric.Testability.dll and System.Fabric.dll.

using System;
using System.Fabric;
using System.Fabric.Testability;
using System.Fabric.Testability.Scenario;
using System.Threading;
using System.Threading.Tasks;

class Test
{
    public static int Main(string[] args)
    {
        string clusterConnection = "localhost:19000";

        Console.WriteLine("Starting Chaos Test Scenario...");
        try
        {
            RunChaosTestScenarioAsync(clusterConnection).Wait();
        }
        catch (AggregateException ae)
        {
            Console.WriteLine("Chaos Test Scenario did not complete: ");
            foreach (Exception ex in ae.InnerExceptions)
            {
                if (ex is FabricException)
                {
                    Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
                }
            }
            return -1;
        }

        Console.WriteLine("Chaos Test Scenario completed.");
        return 0;
    }

    static async Task RunChaosTestScenarioAsync(string clusterConnection)
    {
        TimeSpan maxClusterStabilizationTimeout = TimeSpan.FromSeconds(180);
        uint maxConcurrentFaults = 3;
        bool enableMoveReplicaFaults = true;

        // Create FabricClient with connection & security information here.
        FabricClient fabricClient = new FabricClient(clusterConnection);

        // The Chaos Test Scenario should run at least 60 minutes or up until it fails.
        TimeSpan timeToRun = TimeSpan.FromMinutes(60);
        ChaosTestScenarioParameters scenarioParameters = new ChaosTestScenarioParameters(
          maxClusterStabilizationTimeout,
          maxConcurrentFaults,
          enableMoveReplicaFaults,
          timeToRun);

        // Other related parameters:
        // Pause between two iterations for a random duration bound by this value.
        // scenarioParameters.WaitTimeBetweenIterations = TimeSpan.FromSeconds(30);
        // Pause between concurrent actions for a random duration bound by this value.
        // scenarioParameters.WaitTimeBetweenFaults = TimeSpan.FromSeconds(10);

        // Create the scenario class and execute it asynchronously.
        ChaosTestScenario chaosScenario = new ChaosTestScenario(fabricClient, scenarioParameters);

        try
        {
            await chaosScenario.ExecuteAsync(CancellationToken.None);
        }
        catch (AggregateException ae)
        {
            throw ae.InnerException;
        }
    }
}
```

Powershell

```powershell
$connection = "localhost:19000"
$timeToRun = 60
$maxStabilizationTimeSecs = 180
$concurrentFaults = 3
$waitTimeBetweenIterationsSec = 60

Connect-ServiceFabricCluster $connection

Invoke-ServiceFabricChaosTestScenario -TimeToRunMinute $timeToRun -MaxClusterStabilizationTimeoutSec $maxStabilizationTimeSecs -MaxConcurrentFaults $concurrentFaults -EnableMoveReplicaFaults -WaitTimeBetweenIterationsSec $waitTimeBetweenIterationsSec
```


## 故障转移测试

故障转移测试方案是针对一个特定服务分区的混乱测试方案的版本。 它同时使其他服务不会影响测试对特定服务分区故障转移的影响。 一旦与目标分区信息和其他配置的参数作为客户端运行工具使用 C# Api 或 Powershell 生成错误用于服务分区。 该方案通过一系列的模拟的故障和服务验证时方提供工作负荷运行业务逻辑循环。 服务验证失败表明需要进一步调查的问题。

### 故障转移测试中模拟的故障
- 重新启动分区位于部署代码包
- 主要/辅助副本或无状态的实例中删除
- 重新启动主辅助副本 （如果保持服务）
- 移动主副本
- 移动一个辅助副本
- 重新启动分区。

故障转移测试工作引起选的故障，然后运行验证服务，以确保其稳定性。 故障转移测试只能导致一个错误而不可能是一次混乱测试中的多个故障。 如果在服务分区不在配置的超时时间内稳定下来每个故障后测试，则测试失败导致仅安全故障。 这意味着，在没有外部故障的仲裁或数据的丢失将不会发生。

### 重要的配置选项
 - **PartitionSelector**︰ 选择器对象，它指定目标所需要的分区。
 - **TimeToRun**︰ 总在完成之前将运行测试的时间
 - **MaxServiceStabilizationTimeout**︰ 群集变得健康测试失败前等待的时间的最大金额。 执行检查是服务运行状况是否确定、 目标副本集实现的所有分区和 InBuild 的复制副本的大小。
 - **WaitTimeBetweenFaults**︰ 每个故障和验证周期之间等待的时间量

### 如何运行故障转移测试
C# 示例

```csharp
// Add a reference to System.Fabric.Testability.dll and System.Fabric.dll.

using System;
using System.Fabric;
using System.Fabric.Testability;
using System.Fabric.Testability.Scenario;
using System.Threading;
using System.Threading.Tasks;

class Test
{
    public static int Main(string[] args)
    {
        string clusterConnection = "localhost:19000";
        Uri serviceName = new Uri("fabric:/samples/PersistentToDoListApp/PersistentToDoListService");

        Console.WriteLine("Starting Chaos Test Scenario...");
        try
        {
            RunFailoverTestScenarioAsync(clusterConnection, serviceName).Wait();
        }
        catch (AggregateException ae)
        {
            Console.WriteLine("Chaos Test Scenario did not complete: ");
            foreach (Exception ex in ae.InnerExceptions)
            {
                if (ex is FabricException)
                {
                    Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
                }
            }
            return -1;
        }

        Console.WriteLine("Chaos Test Scenario completed.");
        return 0;
    }

    static async Task RunFailoverTestScenarioAsync(string clusterConnection, Uri serviceName)
    {
        TimeSpan maxServiceStabilizationTimeout = TimeSpan.FromSeconds(180);
        PartitionSelector randomPartitionSelector = PartitionSelector.RandomOf(serviceName);

        // Create FabricClient with connection & security information here.
        FabricClient fabricClient = new FabricClient(clusterConnection);

        // The Chaos Test Scenario should run at least 60 minutes or up until it fails.
        TimeSpan timeToRun = TimeSpan.FromMinutes(60);
        FailoverTestScenarioParameters scenarioParameters = new FailoverTestScenarioParameters(
          randomPartitionSelector,
          timeToRun,
          maxServiceStabilizationTimeout);

        // Other related parameters:
        // Pause between two iterations for a random duration bound by this value.
        // scenarioParameters.WaitTimeBetweenIterations = TimeSpan.FromSeconds(30);
        // Pause between concurrent actions for a random duration bound by this value.
        // scenarioParameters.WaitTimeBetweenFaults = TimeSpan.FromSeconds(10);

        // Create the scenario class and execute it asynchronously.
        FailoverTestScenario chaosScenario = new FailoverTestScenario(fabricClient, scenarioParameters);

        try
        {
            await chaosScenario.ExecuteAsync(CancellationToken.None);
        }
        catch (AggregateException ae)
        {
            throw ae.InnerException;
        }
    }
}
```


Powershell

```powershell
$connection = "localhost:19000"
$timeToRun = 60
$maxStabilizationTimeSecs = 180
$waitTimeBetweenFaultsSec = 10
$serviceName = "fabric:/SampleApp/SampleService"

Connect-ServiceFabricCluster $connection

Invoke-ServiceFabricFailoverTestScenario -TimeToRunMinute $timeToRun -MaxServiceStabilizationTimeoutSec $maxStabilizationTimeSecs -WaitTimeBetweenFaultsSec $waitTimeBetweenFaultsSec -ServiceName $serviceName -PartitionKindSingleton
```

 
