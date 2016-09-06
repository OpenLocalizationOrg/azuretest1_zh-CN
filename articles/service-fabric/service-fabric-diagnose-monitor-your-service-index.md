---
ms.openlocfilehash: f46999d9931d1e84f8a2bbc4f82beca16eabf6ca
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="诊断并解决服务结构服务"
   description="概念信息和教程，可帮助您诊断、 监视和诊断服务结构服务。"
   services="service-fabric"
   documentationCenter=".net"
   authors="rwike77"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/04/2015"
   ms.author="ryanwi"/>

# 诊断和监视服务结构服务
监视、 检测、 诊断和故障排除允许服务继续对用户体验的中断降至最低。 若要了解详细信息，请阅读︰

- [如何监视和诊断服务本地](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)
- [设置服务结构应用程序的应用程序的见解](service-fabric-diagnostics-application-insights-setup.md)
- [故障排除的应用程序升级失败](service-fabric-application-upgrade-troubleshooting.md)
- [故障排除故障监视应用程序升级](../service-fabric-application-monitored-upgrade-troubleshooting.md)
- [诊断和可靠的参与者的性能监视](service-fabric-reliable-actors-diagnostics.md)
- [诊断和可靠的服务的性能监视](service-fabric-reliable-services-diagnostics.md)

## 群集的疑难解答
以下信息将帮助您解决您的本地开发群集︰

- [解决您的本地开发群集安装](service-fabric-troubleshoot-local-cluster-setup.md)

## 运行状况模型
服务结构引入了一个健康模型，它提供丰富、 灵活和可扩展的报告和评估功能，对于服务结构的实体。 服务结构组件的所有实体都报告开箱即用的健康。 用户服务可以丰富健康数据和特定于它们的逻辑，报告了自己或群集中的其他实体的信息。 若要了解详细信息，请阅读︰

- [介绍服务结构运行状况监视](service-fabric-health-introduction.md)
- [如何查看服务结构运行状况报告](service-fabric-view-entities-aggregated-health.md)
- [用于故障排除的系统运行状况报告](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)
- [添加自定义的服务结构运行状况报告](service-fabric-report-health.md)
 