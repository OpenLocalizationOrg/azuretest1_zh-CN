---
ms.openlocfilehash: a494252b36b3dba595c02bc489725cd528c590d7
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="我们的服务结构应用程序升级"
   description="本文介绍了升级服务结构应用程序。"
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


# 我们的服务结构应用程序升级


一个服务结构应用程序服务的集合。 升级过程中，服务结构新[应用程序清单](service-fabric-application-model.md#describe-an-application)与以前的版本进行比较，并确定在应用程序需要更新的服务。 这取决于将在服务中的数字清单到以前的版本的版本进行比较。 如果尚未更改服务，该服务不会升级。

## 滚动升级概述
在应用程序进行滚动升级，分阶段执行升级。 在每个阶段中，升级应用于群集，称为升级的域中的节点的子集。 因此，在应用程序保持可用整个升级。 升级期间，群集可能包含新旧版本的混合。 因此，这两个版本必须是向前和向后兼容。 如果它们不兼容，应用程序管理员负责主持了多阶段升级维护可用性。 这是执行升级到最终版本之前是与以前的版本兼容的应用程序的中间版本升级。

配置群集时，群集清单中被指定升级域。 不应假定，升级域将按特定的顺序接收更新。 升级域是部署的应用程序的逻辑单元。 升级域允许在升级过程中保持高可用性的服务。

如果升级适用于群集，属于这种情况，当应用程序具有一个升级域中的所有节点，是可能的非滚动升级。 这是推荐使用，因为该服务将不可用，在升级时。 此外，Azure 时群集设置有一个升级域不提供任何保证。

## 在升级过程中的运行状况检查
对于升级，健康策略必须设置 （或可能使用默认值）。 在指定超时内升级的所有升级域和被认为是所有升级的域状态良好时，称作成功升级。  正常升级域意味着升级域传递运行状况策略中指定的所有运行状况检查，-例如，健康策略可能要求对应用程序实例中的所有服务必须都是<em>健康</em>，如健康定义的服务结构。

健康策略和服务结构的升级过程中检查的服务和应用程序的不可知。 也就是说，没有服务的特定测试进行检查。  例如，您的服务具有最小的吞吐量要求。 但是，服务结构没有信息可供测试，并且因此不会检查吞吐量为您的应用程序定义。   请参阅执行检查[健康的文章](service-fabric-health-introduction.md)-在升级过程中的检查包括如果正确，请复制该应用程序包，并且实例被启动等。

应用程序运行状况是一种聚合应用程序的子实体。 简而言之，服务结构计算运行状况的应用程序通过应用程序以及应用程序的服务的运行状况报告的健康状况。 应用程序服务的运行状况进一步计算聚合服务副本如孩子的健康状况。 一旦应用程序运行状况策略都不满足，可以继续升级或健康策略是否违反了该应用程序的升级会失败。

## 升级模式

通用模式 （及建议） 对升级进行监控。  监视一个升级域上执行升级并且所有运行状况检查 （每个指定的策略） 的阶段转到下一个升级域自动，依次类推。  如果运行状况检查失败，也已达到超时，升级或者回滚升级的域中，或如果该更改为 UnmonitoredManual 的模式是在升级时所选的选项。

UnmonitoredManual 将开始下一个升级域上的升级升级域上每个升级后需要手动干预。 没有服务结构运行状况检查，不会进行，并依赖于要执行的下一个升级域在开始升级之前的运行状况或状态检查 intervener。

## 应用程序升级流程图

下一个服务结构的应用程序在升级过程的理解与流程图。 特别是，流描述如何超时，包括*HealthCheckStableDuration*、 *HealthCheckRetryTimeout*和*UpgradeHealthCheckInterval*帮助控制时成功或失败，被视为一个升级的域中升级。

![服务结构应用程序升级过程][image]


## 下一步行动

[升级的教程](service-fabric-application-upgrade-tutorial.md)

[升级参数](service-fabric-application-upgrade-parameters.md)

[数据序列化](service-fabric-application-upgrade-data-serialization.md)

[高级的主题](service-fabric-application-upgrade-advanced.md)

[应用程序升级的疑难解答 ](service-fabric-application-upgrade-troubleshooting.md)



[image]: media/service-fabric-application-upgrade/service-fabric-application-upgrade-flowchart.png
 
