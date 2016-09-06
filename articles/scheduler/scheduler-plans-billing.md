---
ms.openlocfilehash: f2af3cab6c732c07c747ce238972a04651e6c9cc
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
 pageTitle="计划和 Azure 的计划程序中的计费" 
 description="" 
 services="scheduler" 
 documentationCenter=".NET" 
 authors="krisragh" 
 manager="dwrede" 
 editor=""/>
<tags 
 ms.service="scheduler" 
 ms.workload="infrastructure-services" 
 ms.tgt_pltfrm="na" 
 ms.devlang="dotnet" 
 ms.topic="article" 
 ms.date="08/04/2015" 
 ms.author="krisragh"/>
 
# 计划和 Azure 的计划程序中的计费

## 作业集合计划

作业集合是 Azure 计划程序中的计费实体。 作业集合包含大量的作业和进入如下所述的三个计划 – 自由、 标准和津贴 –。

|**作业收集计划**|**每个作业集合作业的最大值 #**|**最大重复周期**|**每个订阅的最大工作集**|**限制**|
|:---|:---|:---|:---|:---|
|**免费**|每个作业集合 5 作业|一次每小时。 不能执行作业通常比每小时一次|订阅被允许最大为 1 可用的作业集合|不能使用[HTTP 站授权对象](scheduler-outbound-authentication.md)
|**标准**|每个作业集合 50 作业|一次每分钟。 不能一次一分钟的时间通常执行作业|订阅被允许的最多 100 个标准作业集合|对计划程序的完整功能集的访问|
|**高级**|每个作业集合 50 作业|一次每分钟。 不能一次一分钟的时间通常执行作业|订阅被允许达 10000 个特优作业集合。 <a href="mailto:wapteams@microsoft.com">请与我们联系，</a>的详细信息。|对计划程序的完整功能集的访问|

## 升级和降级的作业集合计划

您可以升级或降级的作业集合计划随时之间的自由、 标准和特优计划。 但是，当降级到一个可用的作业集合，降级可能失败由于下列原因之一︰

- 可用的作业集合中已存在于订阅
- 作业在作业具有高重复频率为不允许在集合中可用的作业的作业。 允许自由作业集合中的最大重复周期是每小时一次
- 作业集合中有 5 个以上的作业
- 作业在作业具有 HTTP 或 HTTPS 使用[HTTP 站授权对象](scheduler-outbound-authentication.md)的操作

## 帐单邮寄地址和 Azure 计划

订阅不收取免费作业集合。 如果您有 100 多个标准的作业集合 （10 个标准计费单位），它是更好的交易，以高级规划中有所有的作业集合。

如果您有一个标准的作业集合和一个高级作业集合，您将记帐一个标准计费单元_和_一个高级计费单位。 基于活动的作业集合被设置为标准或特优; 数计划程序服务物料清单这在接下来的两节中进一步说明。

## 收费的标准单位

标准计费单位可以包括最多 10 个标准的作业集。 由于标准作业集合可以有最多 50 个作业每个作业的集合，一个标准计费单位允许订阅有 500 作业 – 最多每个月几乎 22 万个 job。

如果您有 1 到 10 的标准作业集合之间，您将货款 1 标准计费单位。 如果您有标准的作业集合提供 11 和 20 之间，则将货款 2 标准计费单位。 如果您有 21 和 30 个标准的作业集合之间，您将货款 3 标准计费单位，等等。

## 最优收费单位

特优的计费单位可以包括达 10000 个特优作业集合。 由于高级作业集合可以有最多 50 个作业每个作业的集合，一个高级计费单位允许让达 500000 作业 – 最多每个月约 22 亿美元 job 的订阅。

如果您有 1 到 10000 个特优作业集合之间，您将货款 1 高级计费单位。 如果您有 10,001 和 20000 特优作业集合之间，您的 2 个特优计费单元，将货款等等。

因此，最优的作业集合具有相同的功能作为标准作业集合而提供价格中断，您的应用程序需要大量的作业集合的情况下。 

## 计费和活动状态

作业集合都有效，除非整个订购已进入计费问题由于某些临时禁用状态。 确保作业集合不计入的唯一途径是到将其设置到_自由_计划或删除的作业集合。

尽管可能会禁用所有的作业，在作业集中在单个操作中，它不会更改作业集合记帐状态 — — 作业集合将_仍然_货款。 同样，空作业集合被视为有效，将货款。 

## 定价

有关定价的详细信息，请参阅[计划定价](http://azure.microsoft.com/pricing/details/scheduler/)。

## 请参见
 
 [计划是什么？](scheduler-intro.md)
 
 [计划程序的概念、 术语和实体层次结构](scheduler-concepts-terms.md)

 [开始管理门户中使用计划程序](scheduler-get-started-portal.md)

 [如何构建复杂的调度和高级的定期使用 Azure 的调度程序](scheduler-advanced-complexity.md)

 [计划程序 REST API 参考](https://msdn.microsoft.com/library/dn528946)   

 [计划程序 PowerShell Cmdlet 引用](scheduler-powershell-reference.md)
 
 [计划程序的高可用性和可靠性](scheduler-high-availability-reliability.md)

 [计划程序限制、 默认值和错误代码](scheduler-limits-defaults-errors.md)

 [计划程序的出站身份验证](scheduler-outbound-authentication.md)
  
