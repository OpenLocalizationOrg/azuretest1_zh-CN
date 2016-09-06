---
ms.openlocfilehash: 08afee2c6e3ebef89d80f060892e3347706e1546
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="故障排除服务结构应用程序升级"
   description="本文介绍了一些常见的问题，围绕升级服务结构应用程序以及如何解决这些问题。"
   services="service-fabric"
   documentationCenter=".net"
   authors="mani-ramaswamy"
   manager="samgeo"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/11/2015"
   ms.author="subramar"/>

# 应用程序升级的疑难解答

本文介绍了一些常见的问题，围绕升级服务结构应用程序以及如何解决这些问题。

## 失败的应用程序升级的疑难解答

升级失败，**获取 ServiceFabricApplicationUpgrade**命令的输出将包含一些用于调试故障的其他信息。 此信息可用于︰

1. 识别故障类型
2. 发现失败的原因
3. 找出故障组件，供进一步调查

此信息将服务结构检测无论**FailureAction**是回滚失败时，就立即可用，或者挂起升级。

### 识别故障类型

在**获取 ServiceFabricApplicationUpgrade**的输出， **FailureTimestampUtc**标识中的时间戳 （UTC) 升级故障检测到服务结构和触发**FailureAction**处。 **FailureReason**标识三个潜在高级别故障原因之一︰

1. UpgradeDomainTimeout-指示特定升级域时间太长而无法完成， **UpgradeDomainTimeout**已过期。
2. OverallUpgradeTimeout-表示整体升级时间太长而无法完成， **UpgradeTimeout**已过期。
3. 运行状况检查的指示后升级升级域，应用程序一直运行不正常，根据指定的运行状况策略和过期的**HealthCheckRetryTimeout** 。

这些条目将仅显示在输出时升级将失败，启动回滚。 根据故障的类型，将显示详细信息。

### 调查升级超时

升级的超时故障通常由服务可用性问题。 下面的输出是典型的升级，服务副本或实例无法启动新的代码版本中。 **UpgradeDomainProgressAtFailure**字段中捕获任何挂起的升级工作，在出现故障时的快照。

~~~
PS D:\temp> Get-ServiceFabricApplicationUpgrade fabric:/DemoApp

ApplicationName                : fabric:/DemoApp
ApplicationTypeName            : DemoAppType
TargetApplicationTypeVersion   : v2
ApplicationParameters          : {}
StartTimestampUtc              : 4/14/2015 9:26:38 PM
FailureTimestampUtc            : 4/14/2015 9:27:05 PM
FailureReason                  : UpgradeDomainTimeout
UpgradeDomainProgressAtFailure : MYUD1

                                 NodeName            : Node4
                                 UpgradePhase        : PostUpgradeSafetyCheck
                                 PendingSafetyChecks :
                                     WaitForPrimaryPlacement - PartitionId: 744c8d9f-1d26-417e-a60e-cd48f5c098f0

                                 NodeName            : Node1
                                 UpgradePhase        : PostUpgradeSafetyCheck
                                 PendingSafetyChecks :
                                     WaitForPrimaryPlacement - PartitionId: 4b43f4d8-b26b-424e-9307-7a7a62e79750
UpgradeState                   : RollingBackCompleted
UpgradeDuration                : 00:00:46
CurrentUpgradeDomainDuration   : 00:00:00
NextUpgradeDomain              :
UpgradeDomainsStatus           : { "MYUD1" = "Completed";
                                 "MYUD2" = "Completed";
                                 "MYUD3" = "Completed" }
UpgradeKind                    : Rolling
RollingUpgradeMode             : UnmonitoredAuto
ForceRestart                   : False
UpgradeReplicaSetCheckTimeout  : 00:00:00
~~~

在此示例中，我们可以看到，在升级失败升级域*MYUD1*和两个分区 （*744c8d9f-1d26-417e-a60e-cd48f5c098f0*和*4b43f4d8-b26b-424e-9307-7a7a62e79750*） 被难住，无法在*节点 1*和*节点 4*的目标节点上放置主副本 (*WaitForPrimaryPlacement*)。 可以使用**Get ServiceFabricNode**命令以验证这两个节点在升级域*MYUD1*。 *UpgradePhase*说︰ *PostUpgradeSafetyCheck*，这意味着升级的域中的所有节点必须都完成升级后出现了这些安全检查。 所有这些信息组合使用新版本的应用程序代码指向可能存在的问题。 最常见的问题是在打开或升级到主代码路径中的服务错误。

*PreUpgradeSafetyCheck* *UpgradePhase*表示出现了问题，在实际执行升级之前准备升级域。 在这种情况下，最常见的问题是服务关闭或降级从主代码路径中的错误。

当前的**UpgradeState**是*RollingBackCompleted*，因此必须为原来的升级执行**FailureAction**，自动回滚升级失败后的回滚。 如果与手动**FailureAction**执行了原来的升级，升级会改为将处于挂起状态以允许实时调试应用程序。

### 调查健康检查失败

健康检查故障可以触发多个可能发生在所有节点在升级升级域完成之后, 通过所有安全检查的其他问题。 下面的输出是典型的升级失败的运行性能状况检查失败。 **UnhealthyEvaluations**字段中捕获的所有快照故障运行状况检查根据用户指定的[运行状况策略](service-fabric-health-introduction.md)升级失败的时候。

~~~
PS D:\temp> Get-ServiceFabricApplicationUpgrade fabric:/DemoApp

ApplicationName                         : fabric:/DemoApp
ApplicationTypeName                     : DemoAppType
TargetApplicationTypeVersion            : v4
ApplicationParameters                   : {}
StartTimestampUtc                       : 4/24/2015 2:42:31 AM
UpgradeState                            : RollingForwardPending
UpgradeDuration                         : 00:00:27
CurrentUpgradeDomainDuration            : 00:00:27
NextUpgradeDomain                       : MYUD2
UpgradeDomainsStatus                    : { "MYUD1" = "Completed";
                                          "MYUD2" = "Pending";
                                          "MYUD3" = "Pending" }
UnhealthyEvaluations                    :
                                          Unhealthy services: 50% (2/4), ServiceType='PersistedServiceType', MaxPercentUnhealthyServices=0%.

                                          Unhealthy service: ServiceName='fabric:/DemoApp/Svc3', AggregatedHealthState='Error'.

                                              Unhealthy partitions: 100% (1/1), MaxPercentUnhealthyPartitionsPerService=0%.

                                              Unhealthy partition: PartitionId='3a9911f6-a2e5-452d-89a8-09271e7e49a8', AggregatedHealthState='Error'.

                                                  Error event: SourceId='Replica', Property='InjectedFault'.

                                          Unhealthy service: ServiceName='fabric:/DemoApp/Svc2', AggregatedHealthState='Error'.

                                              Unhealthy partitions: 100% (1/1), MaxPercentUnhealthyPartitionsPerService=0%.

                                              Unhealthy partition: PartitionId='744c8d9f-1d26-417e-a60e-cd48f5c098f0', AggregatedHealthState='Error'.

                                                  Error event: SourceId='Replica', Property='InjectedFault'.

UpgradeKind                             : Rolling
RollingUpgradeMode                      : Monitored
FailureAction                           : Manual
ForceRestart                            : False
UpgradeReplicaSetCheckTimeout           : 49710.06:28:15
HealthCheckWaitDuration                 : 00:00:00
HealthCheckStableDuration               : 00:00:10
HealthCheckRetryTimeout                 : 00:00:10
UpgradeDomainTimeout                    : 10675199.02:48:05.4775807
UpgradeTimeout                          : 10675199.02:48:05.4775807
ConsiderWarningAsError                  :
MaxPercentUnhealthyPartitionsPerService :
MaxPercentUnhealthyReplicasPerPartition :
MaxPercentUnhealthyServices             :
MaxPercentUnhealthyDeployedApplications :
ServiceTypeHealthPolicyMap              :
~~~

研究健康检查故障首先需要了解服务结构运行状况模型，但即使没有这种深入了解，我们可以看到两个服务是不正常︰*结构: / DemoApp/Svc3*和*结构: / DemoApp/Svc2*以及错误运行状况报告 (如本例中的"InjectedFault")。 在此示例中，2 个 4 服务是健康的下面的 0%不正常 (*MaxPercentUnhealthyServices*) 的默认目标。

升级已挂起故障时，通过指定手动**FailureAction**时开始升级，因此如果需要采取任何进一步措施前，我们可以调查在实时系统中的故障状态。

### 恢复暂停的升级

与回滚**FailureAction**，是无法进行恢复需要因为升级将自动回滚时出现故障。 与手动**FailureAction**，有多个恢复选项︰

1. 手动触发回滚
2. 手动继续在后面的升级
3. 继续监视的升级

**开始-ServiceFabricApplicationRollback**命令可在任何时候启动回滚应用程序。 一旦该命令返回成功，回滚请求已在系统中注册并将立即开始。

**恢复 ServiceFabricApplicationUpgrade**命令可用于手动，逐步升级的剩余部分一次一个升级域。 在此模式下，仅通过系统-没有更多的健康状况，将执行检查执行检查的安全。 *UpgradeState*显示了*RollingForwardPending*，这意味着当前的升级域已完成升级，但下一个升级域尚未开始 （挂起） 时，可以仅使用此命令。

**更新 ServiceFabricApplicationUpgrade**命令可以用于继续监视的升级与这两个安全和运行状况检查正在执行。

~~~
PS D:\temp> Update-ServiceFabricApplicationUpgrade fabric:/DemoApp -UpgradeMode Monitored

UpgradeMode                             : Monitored
ForceRestart                            :
UpgradeReplicaSetCheckTimeout           :
FailureAction                           :
HealthCheckWaitDuration                 :
HealthCheckStableDuration               :
HealthCheckRetryTimeout                 :
UpgradeTimeout                          :
UpgradeDomainTimeout                    :
ConsiderWarningAsError                  :
MaxPercentUnhealthyPartitionsPerService :
MaxPercentUnhealthyReplicasPerPartition :
MaxPercentUnhealthyServices             :
MaxPercentUnhealthyDeployedApplications :
ServiceTypeHealthPolicyMap              :

PS D:\temp>
~~~

升级将继续从上次已挂起升级域，并使用相同的升级参数和运行状况策略作为前。 如果需要请升级参数和运行状况策略在上面的输出中显示的任何可以恢复在升级时更改在相同的命令。 在此示例中，升级已恢复监视模式保持不变，从而使用相同的参数和运行状况策略之前的所有其他参数。

## 进一步的故障诊断

### 服务结构是没有按照指定的运行状况策略

1 的可能原因︰

服务结构将所有百分比都转换为实际的实体 （例如复制副本、 分区和服务） 的运行状况评估的数字，并始终为最接近的整实体将向上舍入。 例如，如果_MaxPercentUnhealthyReplicasPerPartition_最大的是 21%，并有 5 复制副本，然后服务结构将允许多达 2 副本 (即`Math.Ceiling (5\*0.21)`) 计算分区健康时无法正常运行。 应相应地设置健康策略，以便考虑到这一点。

可能的原因 2:

健康策略指定百分比的总的服务并不是特定的服务实例。 例如，在升级之前，假定应用程序有四个服务实例 A、 B、 C 和 D，其中服务 D 是不正常，但对应用程序没有显著影响。 我们想要在升级和一组参数*MaxPercentUnhealthyServices*是假定 A、 B 和 C 的 25%必须是健康的过程中忽略已知的不健康服务 D。 但是，升级期间，D 可能会变得健康而 C 变得不正常。 但仍然建议在升级完成成功在这种情况下由于只有 25%的服务是不正常的这可能导致由于 C 被意外不正常而不是数码的意外错误在此情况下，应为不同的服务类型，从建模 D B 和 c。运行状况策略可以指定在每个服务类型的基础上，因为这将允许将不同的不正常百分比阈值应用到基于他们的应用程序中的角色的不同服务。

### 我没有指定应用程序升级的运行状况策略，但是升级仍然失败的我永远不会指定某些超时

健康策略并不提供给升级请求，他们已经从当前的应用程序版本*ApplicationManifest.xml* （例如，如果使用到 v2，v1 中指定的应用程序 X 的应用程序运行状况策略从 v1 升级应用程序 X）。 是否应升级为使用不同的运行状况策略，该策略将需要指定为应用程序升级 API 调用的一部分。 请注意，作为 API 调用的一部分指定的策略只应用于在升级期间。 一旦升级完成后，将用在*ApplicationManifest.xml*中指定的策略。

### 指定了不正确的超时。

如果超时设置不一致，例如，具有小于*UpgradeDomainTimeout* *UpgradeTimeout*所出现的情况，用户可能曾疑惑。 答案是，则会返回错误。 其他情况下可能会发生这种情况是如果*UpgradeDomainTimeout* *HealthCheckWaitDuration*和*HealthCheckRetryTimeout*的总和小于或*UpgradeDomainTimeout*小于*HealthCheckWaitDuration*和*HealthCheckStableDuration*的总和。

### 我升级的时间太长

完成升级所需的时间将取决于各种的健康检查，并指定超时，哪些又是取决于您的应用程序升级 （包括复制包时，部署，和稳定） 花费的时间。 进行过大规模超时可能意味着更升级失败，并因此开始具有较长的超时的保守建议。

快速刷新器上超时升级时间的交互方式︰

升级对于升级域不能比*HealthCheckWaitDuration*更快完成 + *HealthCheckStableDuration*。

升级失败不能出现比*HealthCheckWaitDuration* + *HealthCheckRetryTimeout*。

升级域的升级时间受到*UpgradeDomainTimeout*。  如果来回切换应用程序的运行状况，保持*HealthCheckRetryTimeout*和*HealthCheckStableDuration*均不为零，然后升级最终将超时在*UpgradeDomainTimeout*上。 *UpgradeDomainTimeout*将启动一旦当前升级域升级开始倒计时。

## 下一步行动

[升级的教程](service-fabric-application-upgrade-tutorial.md)

[升级参数](service-fabric-application-upgrade-parameters.md)

[高级的主题](service-fabric-application-upgrade-advanced.md)

[数据序列化](service-fabric-application-upgrade-data-serialization.md)
 
