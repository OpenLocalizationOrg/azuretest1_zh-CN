---
ms.openlocfilehash: 6f28a5de122f24c01716f776d3d904033c32d4f8
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="扩展 API 端点 |Microsoft Azure" 
    description="在 Azure 机器学习中缩放 web 服务终结点" 
    services="machine-learning"
    documentationCenter="" 
    authors="hiteshmadan" 
    manager="padou" 
    editor=""/>

<tags
    ms.service="machine-learning"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="tbd" 
    ms.date="06/29/2015"
    ms.author="himad"/>


# 缩放的 API 端点

在 Azure 机器学习中的 web 服务终结点具有可调节级别以匹配该终结点将被消耗的速率。

有两个因素控制的调节在终结点上进行的数量︰
- 调节级别︰ 低或高。 付费的客户允许限制级别设置为高
- 最大并发调用︰ 4 中止值等级低;调节水平高 20-200


通常在其中需要低延迟的情况下使用同步 Api。 这里的延迟意味着它的 API 来完成一个请求，并不会考虑任何网络延迟的时间。 假设您有一个 API，50 毫秒延迟。 完全使用限制级别高的可用容量和最大的并发呼叫 = 20，您需要调用此 API 20 * 1000 / 50 = 400 次数 / 秒。 进一步扩展，为 200 的最大并发呼叫将允许您调用 API 4000 次每秒，假定 50 毫秒的延迟。

如果您打算调用 API 比最大值为 200 的并发呼叫将支持负载较高的您应在相同的 web 服务中创建多个终结点并随机分布负载，在所有这些。

请记住，使用非常高的并发数可能是有害的如果您不命中比率同样高的 API。 如果在高负载的配置 API 将负载相对较低，可能会滞后时间看到零星超时和/或峰值。

请注意，调整油门设置只会影响行为的同步 API (RR)。 如果您在同步 API 看到频繁的 503 服务不可用响应，应调整这些设置。

管理用户界面允许切换的限制级别。 要自定义的并发数目与限制级别高一起，请使用修补程序终结点 API。

- 打开 manage.windowsazure.com
- 导航到机器学习选项卡
- 单击工作区。
- 导航到具有终结点的 web 服务![导航到 web 服务](./media/machine-learning-scaling-endpoints/figure-1.png)

- 该终结点上单击，然后单击配置选项卡上的![导航到终结点配置](./media/machine-learning-scaling-endpoints/figure-2.png)


- 调节级别更改为高，然后单击保存。


 

测试
