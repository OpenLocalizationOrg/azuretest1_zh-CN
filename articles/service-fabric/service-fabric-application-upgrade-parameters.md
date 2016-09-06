---
ms.openlocfilehash: 1a141011addeb00327b1618d7c04c02b0d296fdc
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---

<properties
   pageTitle="我们的服务结构应用程序升级︰ 升级参数"
   description="本文介绍相关的升级服务结构应用程序的各种参数。"
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
   ms.date="07/17/2015"
   ms.author="subramar"/>



# 升级的应用程序参数

本文介绍了应用服务结构应用程序在升级过程的各种参数。  参数包含的名称和版本的应用程序，并将旋钮的超时和运行状况检查，在升级过程中应用和升级失败，则必须应用这些策略指定该控件。


<br>

| 参数 | 说明 |
| --- | --- |
| 应用程序名称 | 正在升级的应用程序的名称。 示例︰ 结构: / VisualObjects，结构: / ClusterMonitor  |
| TargetApplicationTypeVersion | 应用程序的版本类型升级的目标。 |
| FailureAction | FailureAction 属性描述要执行的服务结构中，如果升级失败的操作。 应用程序可能被回滚到上一个版本的更新 （回滚） 之前的版本或停止当前的升级域在应用程序升级和升级的模式更改为手动。  允许的值为回滚和手册。 |
| HealthCheckWaitDurationSec | 之后要等待时间 （以秒为单位） 在升级完成之前服务结构升级域上对应用程序的运行状况进行评估。 此持续时间也将视为应用程序应该运行之前可以将其视为正常的时间。 如果通过了健康检查，升级过程将继续到下一个升级域。  如果运行状况检查失败，服务结构等待间隔 (UpgradeHealthCheckInterval) 达到 HealthCheckRetryTimeout 后再次重试运行状况检查。 默认值和建议的值为 0 秒。 |
| HealthCheckRetryTimeoutSec | 该服务结构继续执行之前声明升级为失败的运行状况评估的持续时间 （以秒为单位）。 默认值为 600 秒。 此持续时间开始后到达 HealthCheckWaitDuration。 在此 HealthCheckRetryTimeout，服务结构可能会执行多个应用程序运行状况的运行性能状况检查。 默认值为 10 分钟，并建议相应地自定义您的应用程序。 |
| HealthCheckStableDurationSec | 持续时间 （以秒为单位） 来验证应用程序是否稳定将移动到下一个升级域或升级完成前等待。 此等待持续时间用于防止健康正确的未检测到的更改后执行运行状况检查。 默认值为 0 秒，并建议相应地自定义您的应用程序。 |
| UpgradeDomainTimeoutSec | 最大时间 （以秒为单位） 升级升级单个域。 如果此超时是达到，则升级将停止并采取由 UpgradeFailureAction 指定的操作。 默认值是永远不会 （无限） 和建议相应地自定义您的应用程序。 |
| UpgradeTimeout | 适用于整个升级超时 （以秒为单位）。 如果到达此超时时间，触发升级停止和 UpgradeFailureAction。 默认值是永远不会 （无限） 和建议相应地自定义您的应用程序。 |
| UpgradeHealthCheckInterval | 在_群集_的_清单_（这不作为升级的 cmdlet 的一部分指定） 的 ClusterManager 部分中指定，此设置将指定的运行状况，处于选中状态的频率 （默认值为 60 秒）。  |






<br>
## 服务应用程序升级过程中的运行状况评估

<br>
健康评估标准是可选的。 如果在启动升级时未指定的健康评估标准，服务结构使用在正在升级的应用程序实例的 ApplicationManifest.xml 中指定的应用程序运行状况策略。


<br>

| 参数 | 说明 |
| --- | --- |
| ConsiderWarningAsError | 默认值为 False。 在评估应用程序在升级过程中的健康状况时将应用程序警告运行状况事件视为错误。 默认情况下，服务结构的计算结果不警告运行状况事件是失败 （错误），所以即使有警告事件继续升级。   |
| MaxPercentUnhealthyDeployedApplications | 默认值和建议的值为 0。 指定已部署的应用程序 （请参见[健康节](service-fabric-health-introduction.md)） 不正常应用程序之前应考虑非正常运行和升级失败最大的数目。 这是打包的应用程序的节点上的健康状况，因此这是非常有用，在升级期间，检测即时的问题应用程序包部署在该节点上的位置是不健康 （崩溃和等）。 在典型情况下，应用程序的副本会均衡地加载到另一个节点，因此使得应用程序显示正常，从而使升级继续。 通过指定严格的 MaxPercentUnhealthyDeployedApplications 健康，服务结构可以检测快速应用程序包有问题，导致快速升级失败。 |
| MaxPercentUnhealthyServices | 默认值和建议的值为 0。 指定该应用程序未考虑不正常且失败升级之前可以不正常的应用程序实例中服务的最大数目。 |
| MaxPercentUnhealthyPartitionsPerService | 默认值和建议的值为 0。 指定服务被认为不正常之前在服务分区最大数量可以是不正常的。 |
| MaxPercentUnhealthyReplicasPerPartition | 默认值和建议的值为 0。 指定分区被认为不正常运行不正常的分区中复制副本的最大数量。 |
| UpgradeReplicaSetCheckTimeout | 无状态的服务--在一个升级域，服务结构中尝试来确保该服务的其他实例都可用。 如果多个目标实例计数，服务结构将等待多个实例都可用，最大最大超时值 （由 UpgradeReplicaSetCheckTimeout 属性指定）。 如果超时过期，服务结构继续升级，无论有多少个服务实例。 如果目标实例计数，服务结构不会等待，并立即继续进行升级。 有状态服务范围一个升级域，试图确保副本集具有一个仲裁服务结构。 服务结构等待仲裁都可用，最大最大超时值 （由 UpgradeReplicaSetCheckTimeout 属性指定）。 如果超时过期，服务结构继续升级，无论仲裁。 此设置被设置为从不 （无限） 时滚动向前 （应用时升级继续如预期的那样） 和 900 秒时回滚 （升级遇到了错误，并且处于正在回滚状态时，应用）。 |
| ForceRestart | 如果您更新配置或数据的包，而不是更新服务代码，服务结构不重新启动该服务除非 ForceRestart 属性设置为 true 作为 API 调用的一部分。 更新完成后，服务结构将通知服务新的配置包的数据是可用。 该服务负责应用所做的更改。 如有必要，该服务可以重新启动。 |



<br>
<br>
每个应用程序实例的服务类型，可以指定 MaxPercentUnhealthyServices、 MaxPercentUnhealthyPartitionsPerService 和 MaxPercentUnhealthyReplicasPerPartition 标准。 这是为了确保包含不同的服务类型的应用程序，可以为每种服务类型有不同的评估策略。 举一个例子，无状态的网关服务类型可以有不同的特定应用程序实例的状态引擎服务类型不同 MaxPercentUnhealthyPartitionsPerService。

## 下一步行动


[升级的教程](service-fabric-application-upgrade-tutorial.md)

[高级的主题](service-fabric-application-upgrade-advanced.md)

[应用程序升级的疑难解答 ](service-fabric-application-upgrade-troubleshooting.md)

[数据序列化](service-fabric-application-upgrade-data-serialization.md)
 
