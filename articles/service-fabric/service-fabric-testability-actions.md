---
ms.openlocfilehash: e524772e008a3e8f5049d81a584014cc2e1fa136
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="可测试性操作 |Microsoft Azure"
   description="这篇文章谈到 Microsoft Azure 服务结构中找到的可测试性操作。"
   services="service-fabric"
   documentationCenter=".net"
   authors="heeldin"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/31/2015"
   ms.author="heeldin;motanv"/>

# 可测试性操作
模拟一个不可靠的基础结构，以便服务结构为开发人员提供模拟各种实际故障和状态转换的方法。 这些公开为可测试性行动。 在低级别的 Api，会导致特定故障注入、 状态转换或验证操作。 将这些操作结合起来，服务开发人员可以编写全面测试您的服务方案。

服务结构提供开箱即用的常用测试方案组成，这些操作。 强烈建议使用这些内置的情况，仔细选择常见的状态转换和故障情况下进行测试。 但是，操作可以用于创建自定义的测试方案，当您想要添加的或者未涵盖的内置方案尚未或定制的应用程序方案的覆盖范围。

System.Fabric.Testability.dll 程序集中找到 C# 实现的操作。 Microsoft.ServiceFabric.Testability.Powershell.dll 程序集中找到的可测试性 PowerShell 模块。 作为运行时安装 ServiceFabricTestability PowerShell 的一部分安装模块，以便易于使用。

## 与正常的容错操作正常
可测试性操作分为两个主要的存储桶︰

* 正常的错误︰ 这些故障模拟像机器重新启动的故障，处理故障。 在这种情况下的故障，突然停止过程的执行上下文。 这意味着应用程序重新启动之前，可以运行状态没有清理。

* 温和的故障︰ 这些故障模拟正常操作，如复制副本将移和谷触发的负载平衡。 在这种情况下该服务获取关闭的通知，并在退出之前可以清理状态。

更好的质量验证，引入各种宽容和正常故障时运行的服务和业务的工作负荷。 正常的错误运用位置服务进程突然退出在某些工作流程的方案。 一旦该服务复制副本恢复通过服务结构测试的恢复路径。 这将有助于测试数据的一致性和是否发生故障后正确地维护服务状态。 故障亦宽容失败的其他套测试该服务正确反应到副本服务结构被移动。 在 RunAsync 方法中测试取消的处理。 检查取消标记被设置，正确保存其状态并退出 RunAsync 方法所需要的服务。

## 可测试性的操作列表

| 操作 | 说明 | 托管的 API | Powershell Cmdlet | 正常/正常故障 |
|---------|-------------|-------------|-------------------|------------------------------|
|CleanTestState| 从发生错误关机测试驱动程序的群集中删除所有的测试状态。 | CleanTestStateAsync | 删除 ServiceFabricTestState | 不适用 |
| InvokeDataLoss | 至服务分区导致数据丢失。 | InvokeDataLossAsync | 调用 ServiceFabricPartitionDataLoss | 正常 |
| InvokeQuorumLoss | 将仲裁丢失的给定状态的服务分区。 | InvokeQuorumLossAsync | 调用 ServiceFabricQuorumLoss | 正常 |
| 移动主 | 将有状态服务的指定主副本移动到指定的群集节点。 | MovePrimaryAsync | 移动 ServiceFabricPrimaryReplica | 正常 |
| 移动辅助 | 将当前状态的服务的辅助副本移动到不同的群集节点。 | MoveSecondaryAsync | 移动 ServiceFabricSecondaryReplica | 正常 |
| RemoveReplica | 模拟通过从群集中删除复制副本的复制副本失败。 这将关闭该复制副本并将它转换到角色 None，从群集中删除所有状态。 | RemoveReplicaAsync | 删除 ServiceFabricReplica | 正常 |
| RestartDeployedCodePackage | 通过重新启动群集中的节点上部署的代码包中模拟代码包过程失败。 这将中止代码包过程将重新承载的所有用户服务副本启动该进程。 | RestartDeployedCodePackageAsync | 重新启动 ServiceFabricDeployedCodePackage | 正常 |
| RestartNode | 通过重新启动节点，来模拟服务结构群集节点出现故障。 | RestartNodeAsync | 重新启动 ServiceFabricNode | 正常 |
| RestartPartition | 模拟数据中心掉电或群集掉电情况下通过重新启动分区的某些或所有复制副本。 | RestartPartitionAsync | 重新启动 ServiceFabricPartition | 正常 |
| RestartReplica | 模拟的复制副本故障重新启动群集中的持久性的副本、 关闭该复制副本，然后重新打开它。 | RestartReplicaAsync | 重新启动 ServiceFabricReplica | 正常 |
| StartNode | 启动已停止的群集节点。 | StartNodeAsync | 开始-ServiceFabricNode | 不适用 |
| StopNode | 通过停止群集中的节点，来模拟某个节点出现故障。 节点下将一直保留，直到被称为 StartNode。 | StopNodeAsync | Stop ServiceFabricNode | 正常 |
| ValidateApplication | 验证的可用性和运行状况的应用程序，通常后插入系统中引入一些故障中的所有服务结构服务。 | ValidateApplicationAsync | 测试 ServiceFabricApplication | 不适用 |
| ValidateService | 验证的可用性和运行状况服务结构服务，通常后插入系统中引入一些故障。 | ValidateServiceAsync | 测试 ServiceFabricService | 不适用 |

## 使用 Powershell 运行可测试性操作

本教程展示如何使用 PowerShell 运行可测试性操作。 您将学习如何运行的可测试性操作针对本地 （亦即。 一台机器) 群集或 Azure 的群集。 当您安装了 Microsoft 服务结构 MSI; 自动安装的可测试性 PowerShell 模块-Microsoft.Fabric.Testability.Powershell.dll而且，当您打开 PowerShell 提示符时自动加载的模块。

教程段︰

- [对一台机器群集运行操作](#run-an-action-against-a-one-box-cluster)
- [对 Azure 群集运行操作](#run-an-action-against-an-azure-cluster)

### 对一台机器群集运行操作

要针对本地群集运行的可测试性操作，首先您需要连接到群集，请在管理员模式下打开 PowerShell 提示。 让我们看一下**重新启动 ServiceFabricNode**操作。

```powershell
Restart-ServiceFabricNode -NodeName Node1 -CompletionMode DoNotVerify
```

此处名为"节点 1"的节点上正在运行的操作**重新启动 ServiceFabricNode**并完成模式指定，它不应验证是否重启操作实际上会成功;为"验证"将导致它以验证是否真正成功的重新启动操作，请指定完成模式。 而不是直接由其名称指定节点，您可以指定通过分区键和副本的种类，如下所示︰

```powershell
Restart-ServiceFabricNode -ReplicaKindPrimary  -PartitionKindNamed -PartitionKey Partition3 -CompletionMode Verify
```


```powershell
$connection = "localhost:19000"
$nodeName = "Node1"

Connect-ServiceFabricCluster $connection
Restart-ServiceFabricNode -NodeName $nodeName -CompletionMode DoNotVerify
```

**重新启动 ServiceFabricNode**用于重新启动群集中的服务结构节点。 这将关闭该 Fabric.exe 进程将重新启动该节点上的系统服务和用户服务副本的所有。 使用此 API 来测试您的服务可以帮助揭示沿故障切换恢复路径的 bug。 它有助于模拟在群集中的节点故障。

下面的屏幕快照所示为正在运行**重新启动 ServiceFabricNode**的可测试性命令。

![](media/service-fabric-testability-actions/Restart-ServiceFabricNode.png)

第一个*Get ServiceFabricNode* (从 ServiceFabric PowerShell 模块 cmdlet) 的输出显示本地群集具有五个节点︰ Node.1 到 Node.5;然后执行的可测试性操作 (cmdlet)**重新启动 ServiceFabricNode**的节点上，命名为 Node.4 之后, 我们看到该节点的运行时间已被重置。

### 对 Azure 群集运行操作

对 Azure 群集运行 （使用 PowerShell) 的可测试性操作是类似于针对本地群集; 运行操作仅差别是︰ 可以运行操作，而不是连接到本地群集之前必须首先连接到 Azure 群集。

## 使用 C 运行的可测试性操作# 

若要运行的可测试性操作，使用 C#，您首先需要连接到使用 FabricClient 群集。 然后获取运行操作所需的参数。 可以使用不同的参数运行相同的操作。
查看 RestartServiceFabricNode 操作，运行它的一种方法是使用节点信息 （节点名称和节点实例 ID） 群集中。

```csharp
RestartNodeAsync(nodeName, nodeInstanceId, completeMode, operationTimeout, CancellationToken.None)
```

几个参数的说明︰

**CompleteMode** -完成模式指定，它不应验证是否重启操作实际上会成功;为"验证"将导致它以验证是否真正成功的重新启动操作，请指定完成模式。  
**OperationTimeout** -设置操作完成，然后 TimeoutException 异常的时间的量。
**CancellationToken** -启用挂起的调用被取消。

而不是直接由其名称指定节点，您可以指定通过分区键和副本的类型。

有关详细信息请参阅[分区选择器和复制副本选择器](#partition_replica_selector)。


```csharp
// Add a reference to System.Fabric.Testability.dll and System.Fabric.dll.
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Fabric.Testability;
using System.Fabric;
using System.Threading;
using System.Numerics;

class Test
{
    public static int Main(string[] args)
    {
        string clusterConnection = "localhost:19000";
        Uri serviceName = new Uri("fabric:/samples/PersistentToDoListApp/PersistentToDoListService");
        string nodeName = "N0040";
        BigInteger nodeInstanceId = 130743013389060139;

        Console.WriteLine("Starting RestartNode test");
        try
        {
            //RestartNode using the replicaSelector
            RestartNodeAsync(clusterConnection, serviceName).Wait();

            //Another way to Restart Node using Nodename and NodeInstanceID.
            RestartNodeAsync(clusterConnection, nodeName, nodeInstanceId).Wait();
        }
        catch (AggregateException exAgg)
        {
            Console.WriteLine("RestartNode did not complete: ");
            foreach (Exception ex in exAgg.InnerExceptions)
            {
                if (ex is FabricException)
                {
                    Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
                }
            }
            return -1;
        }

        Console.WriteLine("RestartNode completed.");
        return 0;
    }

    static async Task RestartNodeAsync(string clusterConnection, Uri serviceName)
    {
        PartitionSelector randomPartitionSelector = PartitionSelector.RandomOf(serviceName);
        ReplicaSelector primaryofReplicaSelector = ReplicaSelector.PrimaryOf(randomPartitionSelector);

        // Create FabricClient with connection & security information here.
        FabricClient fabricclient = new FabricClient(clusterConnection);
        await fabricclient.ClusterManager.RestartNodeAsync(primaryofReplicaSelector, CompletionMode.Verify);
    }

    static async Task RestartNodeAsync(string clusterConnection, string nodeName, BigInteger nodeInstanceId)
    {
        // Create FabricClient with connection & security information here.
        FabricClient fabricclient = new FabricClient(clusterConnection);
        await fabricclient.ClusterManager.RestartNodeAsync(nodeName, nodeInstanceId, CompletionMode.Verify);
    }
}
```

## 分区选择器和复制副本选择器

### 分区选择器
PartitionSelector 是暴露在可测试性的帮助，用于选择要对其执行任何可测试性操作的特定分区。 它可以用于选择特定分区，如果事先已知的分区 ID。 或者，您可以提供分区键，操作将在内部解决的分区 ID。 您还可以选择随机分区。

若要使用，创建 PartitionSelector 对象并选择的分区使用选择 * 方法之一，然后在 PartitionSelector 的对象传递给需要它的 API。 如果未不选择任何选项默认为随机分区。

```csharp
Uri serviceName = new Uri("fabric:/samples/InMemoryToDoListApp/InMemoryToDoListService");
Guid partitionIdGuid = new Guid("8fb7ebcc-56ee-4862-9cc0-7c6421e68829");
string partitionName = "Partition1";
Int64 partitionKeyUniformInt64 = 1;

// Select Random partition
PartitionSelector randomPartitionSelector = PartitionSelector.RandomOf(serviceName);

// Select partition based on Id
PartitionSelector partitionSelectorById = PartitionSelector.PartitionIdOf(serviceName, partitionIdGuid);

// Select partition based on name
PartitionSelector namedPartitionSelector = PartitionSelector.PartitionKeyOf(serviceName, partitionName);

// Select partition based on partition key
PartitionSelector uniformIntPartitionSelector = PartitionSelector.PartitionKeyOf(serviceName, partitionKeyUniformInt64);
```

### 复制副本选择器
ReplicaSelector 是暴露在可测试性的帮助，用来帮助选择一个复制副本，对其执行任何可测试性的操作。 它可以用于选择一个特定的复制副本，如果事先已知副本 id。 此外，您可以选择主副本或随机辅助的选择。 ReplicaSelector PartitionSelector，从派生，因此您需要选择该复制副本，您想要执行的操作的可测试性的分区。

要使用，请创建一个 ReplicaSelector 对象并设置您想要选择副本和分区的方式。 您就可以将它传递到需要它的 API。 如果未不选择任何选项默认为随机副本和随机分区。

Guid partitionIdGuid = 新 Guid("8fb7ebcc-56ee-4862-9cc0-7c6421e68829");PartitionSelector partitionSelector = PartitionSelector.PartitionIdOf （服务、 partitionIdGuid）;长 replicaId = 130559876481875498;


```csharp
// Select Random replica
ReplicaSelector randomReplicaSelector = ReplicaSelector.RandomOf(partitionSelector);

// Select primary replica
ReplicaSelector primaryReplicaSelector = ReplicaSelector.PrimaryOf(partitionSelector);

// Select replica by Id
ReplicaSelector replicaByIdSelector = ReplicaSelector.ReplicaIdOf(partitionSelector, replicaId);

// Select random secondary replica
ReplicaSelector secondaryReplicaSelector = ReplicaSelector.RandomSecondaryOf(partitionSelector);
```

## 下一步行动

- [可测试性方案](service-fabric-testability-scenarios.md)
- 如何测试您的服务
   - [期间的服务工作负荷模拟故障](service-fabric-testability-workload-tests.md)
   - [对服务通信故障的服务](service-fabric-testability-scenarios-service-communication.md)
 
