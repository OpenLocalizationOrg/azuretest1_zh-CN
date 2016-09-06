---
ms.openlocfilehash: aa65a59e17801009ca65be36355e9a929598de02
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
 pageTitle="计划程序的高可用性和可靠性" 
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
 
 
# 计划程序的高可用性和可靠性

## Azure 的计划程序的高可用性

作为一个核心 Azure 平台服务，Azure 计划具有高可用性和地理冗余服务部署和地理区域作业的复制功能。

### Geo 冗余服务部署

Azure 计划程序可通过目前的 Azure 中几乎每个地理区域的用户界面。 Azure 计划程序中可用的区域列表是[在此处列出](http://azure.microsoft.com/regions/#services)。 如果数据中心承载的区域中呈现时不可用，Azure 计划程序的故障切换功能是这样的服务可以从另一个数据中心。

### 地理区域作业复制

不仅是 Azure 计划程序可用于管理请求，但您自己的工作也是各地区复制前端。 当一个地区处于停机时，Azure 计划故障转移，并确保从成对的地理区域中的另一个数据中心中运行作业时。

例如，如果您已经在美国南部中部创建作业，Azure 计划程序自动复制在美国中北部的作业。 当美国南部中部有故障，可确保 Azure 计划运行作业时，从美国中北部。 [此处提供了成对 Azure 区域的列表](https://msdn.microsoft.com/library/azure/dn758204.aspx)。

![][1]

因此，Azure 计划程序可确保您的数据总是在 Azure 出现故障时的相同更广泛地理区域内。 因此，您不需要重复的工作只是为了添加高可用性 — Azure 计划程序自动提供了高可用性功能，作业。

## Azure 的计划程序的可靠性

Azure 计划保证其自身的高可用性和用户创建作业，采用不同的方法。 例如，您的作业可能会调用 HTTP 端点不可用。 Azure 的调度程序却试图通过为您提供备用选项来处理故障成功，执行您的工作。 Azure 的调度程序执行这两种方式︰

### 通过"retryPolicy"的可配置重试策略

Azure 的调度程序允许您配置重试策略。 默认情况下，如果作业失败，计划程序会尝试作业再次四次，以 30 秒的时间间隔。 您可以重新配置此重试策略可以更大胆 （例如，十倍，在 30 秒内） 或松散 （例如，两次，每天的时间间隔。

举例说明时，这可能会有所帮助，您可以创建一个作业运行每周一次并调用一个 HTTP 端点。 如果 HTTP 端点不下几个小时，当作业运行时，可能不希望等待一多周作业重新运行，因为即使默认重试策略将会失败。 在这种情况下，可能会重新尝试 （例如） 每隔三个小时的标准重试策略配置而不是每隔 30 秒。

若要了解如何配置重试策略，请参阅[retryPolicy](scheduler-concepts-terms.md#retrypolicy)。

### 通过"errorAction"的备用端点可配置性

如果目标端点 Azure 调度程序作业仍然无法到达，Azure 计划回到替代错误处理终结点之后重试策略。 如果配置了备用的错误处理端点，Azure 计划程序调用它。 与其他终结点，您自己的作业是面对失败高度可用。

作为一个示例下, 图中 Azure 计划遵循其重试策略来袭纽约 web 服务。 重试失败后，它会检查是否存在备用。 然后将继续，并开始向相同的重试策略与备用服务器发出请求。

![][2]

请注意，同样的重试策略适用于原始操作和替代错误处理操作。 也很可能有替代错误操作的操作类型不同于主操作的操作类型。 例如，虽然主操作可能会调用一个 HTTP 端点，错误操作而可能存储队列操作执行错误日志记录。

若要了解如何配置替换终结点，请参阅[errorAction](scheduler-concepts-terms.md#action-and-erroraction)。

## 请参见
 
 [计划是什么？](scheduler-intro.md)
 
 [计划程序的概念、 术语和实体层次结构](scheduler-concepts-terms.md)
 
 [开始管理门户中使用计划程序](scheduler-get-started-portal.md)
 
 [计划和 Azure 的计划程序中的计费](scheduler-plans-billing.md)
 
 [如何构建复杂的调度和高级的定期使用 Azure 的调度程序](scheduler-advanced-complexity.md)
 
 [计划程序 REST API 参考](https://msdn.microsoft.com/library/dn528946)   
 
 [计划程序 PowerShell Cmdlet 引用](scheduler-powershell-reference.md)
 
 [计划程序限制、 默认值和错误代码](scheduler-limits-defaults-errors.md)
 
 [计划程序的出站身份验证](scheduler-outbound-authentication.md)
 
 
[1]: ./media/scheduler-high-availability-reliability/scheduler-high-availability-reliability-image1.png

[2]: ./media/scheduler-high-availability-reliability/scheduler-high-availability-reliability-image2.png

 
