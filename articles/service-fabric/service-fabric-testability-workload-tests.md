---
ms.openlocfilehash: aff6be0a604acad0d6a24118d1678a7d969a1110
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="自定义测试方案"
   description="如何加强您的服务针对 Graceful/Ungraceful 故障"
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
   ms.date="08/05/2015"
   ms.author="anmola"/>

# 期间的服务工作负荷模拟故障

可测试性与传送的方案使开发人员可以不担心个别错误处理。 但是也有情况下，客户端负载显式隔行扫描的位置和故障可能会需要。 隔行扫描客户端负载和故障，可确保服务在故障发生时，实际执行某些操作。 给定的控制级别可测试性提供了这些可能是工作负载执行的精确时刻。 在应用程序中的不同状态的故障此感应可以查找 bug 并提高质量。

## 示例自定义方案
此测试显示了方案在业务工作负载[宽容和正常故障](service-fabric-testability-actions.md#graceful-vs-ungraceful-fault-actions)进行隔行扫描。 而在中间的服务操作或计算以获得最佳效果，应引起故障。

允许通过公开四个工作负载，A、 B、 C 和 D serice 一个示例每个对应于一组的工作流，并且可能会计算、 存储或混合。 为了简单起见我们将在我们的示例工作负荷抽象。 在本示例中执行的不同错误是。
  + RestartNode︰ 正常故障模拟计算机重新启动
  + RestartDeployedCodePackage︰ 正常故障模拟服务主机进程崩溃
  + RemoveReplica︰ 正常故障模拟复制副本删除
  + MovePrimary︰ 正常故障模拟副本移动触发的服务结构的负载平衡器

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
        // Replace these strings with the actual version for your cluster and appliction.
        string clusterConnection = "localhost:19000";
        Uri applicationName = new Uri("fabric:/samples/PersistentToDoListApp");
        Uri serviceName = new Uri("fabric:/samples/PersistentToDoListApp/PersistentToDoListService");

        Console.WriteLine("Starting Workload Test...");
        try
        {
            RunTestAsync(clusterConnection, applicationName, serviceName).Wait();
        }
        catch (AggregateException ae)
        {
            Console.WriteLine("Workload Test failed: ");
            foreach (Exception ex in ae.InnerExceptions)
            {
                if (ex is FabricException)
                {
                    Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
                }
            }
            return -1;
        }

        Console.WriteLine("Workload Test completed successfully.");
        return 0;
    }

    public enum ServiceWorkloads
    {
        A,
        B,
        C,
        D
    }

    public enum ServiceFabricFaults
    {
        RestartNode,
        RestartCodePackage,
        RemoveReplica,
        MovePrimary,
    }

    public static async Task RunTestAsync(string clusterConnection, Uri applicationName, Uri serviceName)
    {
        // Create FabricClient with connection & security information here.
        FabricClient fabricClient = new FabricClient(clusterConnection);
        // Maximum time to wait for a service to stabilize
        TimeSpan maxServiceStabilizationTime = TimeSpan.FromSeconds(120);

        // How many loops of faults you want to execute
        uint testLoopCount = 20;
        Random random = new Random();

        for (var i = 0; i < testLoopCount; ++i)
        {
            var workload = SelectRandomValue<ServiceWorkloads>(random);
            // Start workload and while it is running go and induce some fault
            var workloadTask = RunWorkloadAsync(workload);

            // While task is executing induce faults into the service. It can be ungraceful faults like
            // RestartNode and RestartDeployedCodePackage or graceful faults like RemoveReplica or MovePrimary
            var fault = SelectRandomValue<ServiceFabricFaults>(random);

            // Create a replica selector which will select a Primary replica from the given service to test
            var replicaSelector = ReplicaSelector.PrimaryOf(PartitionSelector.RandomOf(serviceName));
            // Run the selected random fault
            await RunFaultAsync(applicationName, fault, replicaSelector, fabricClient);
            // Validate the health and stability of the service.
            await fabricClient.ServiceManager.ValidateServiceAsync(serviceName, maxServiceStabilizationTime);

            // Wait for the workload to complete successfully
            await workloadTask;
        }
    }

    private static async Task RunFaultAsync(Uri applicationName, ServiceFabricFaults fault, ReplicaSelector selector, FabricClient client)
    {
        switch (fault)
        {
            case ServiceFabricFaults.RestartNode:
                await client.ClusterManager.RestartNodeAsync(selector, CompletionMode.Verify);
                break;
            case ServiceFabricFaults.RestartCodePackage:
                await client.ApplicationManager.RestartDeployedCodePackageAsync(applicationName, selector, CompletionMode.Verify);
                break;
            case ServiceFabricFaults.RemoveReplica:
                await client.ServiceManager.RemoveReplicaAsync(selector, CompletionMode.Verify, false);
                break;
            case ServiceFabricFaults.MovePrimary:
                await client.ServiceManager.MovePrimaryAsync(selector.PartitionSelector);
                break;
        }
    }

    private static Task RunWorkloadAsync(ServiceWorkloads workload)
    {
        throw new NotImplementedException();
        // This is where you trigger and complete your service workload
        // Please note the faults induced while your service workload is running will
        // fault the Primary service hence you will need to reconnect to complete or check
        // the status of the workload
    }

    private static T SelectRandomValue<T>(Random random)
    {
        Array values = Enum.GetValues(typeof(T));
        T workload = (T)values.GetValue(random.Next(values.Length));
        return T;
    }
}
```
 
