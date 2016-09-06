---
ms.openlocfilehash: d313a65d9c64ed368bdc90d331edaf182dbaf2e2
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
 pageTitle="Azure 计划是什么？ |Microsoft Azure"
 description="Azure 时间表允许您以声明方式描述在云中运行的操作。 然后，再安排并自动运行这些操作。"
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
 ms.topic="get-started-article"
 ms.date="08/04/2015"
 ms.author="krisragh"/>

# Azure 计划是什么？

Azure 时间表允许您以声明方式描述在云中运行的操作。 然后，再安排并自动运行这些操作。  计划程序使用[Azure 门户](scheduler-get-started-portal.md)、 代码、 [REST API](https://msdn.microsoft.com/library/dn528946)或 Azure PowerShell 执行此操作。

计划程序创建、 维护，并调用计划的工作。  计划程序不会承载任何工作负荷或运行任何代码。 只有_调用_代码驻留在其他地方的 it — 在 Azure，内部，或使用另一个提供程序。 通过 HTTP、 HTTPS 或存储队列，它将调用。

调度程序安排[作业](scheduler-concepts-terms.md)，保留作业执行结果历史记录，其中一个可以审查和明确可靠地计划要运行的工作负载。 Azure WebJobs （Azure 应用程序服务中的 Web 应用程序功能的一部分） 和其他 Azure 日程安排功能在后台使用计划程序。 [计划程序 REST API](https://msdn.microsoft.com/library/dn528946)可帮助管理这些操作的通信。 在这种情况下，调度程序支持[复杂的调度和高级的定期](scheduler-advanced-complexity.md)轻松。

有同样适用于计划程序使用的几种方案。 例如︰

+ _重复应用程序操作︰_定期收集数据从 Twitter 提要。
+ _每日维护︰_每天删除操作的日志、 执行备份和其他维护任务。 例如，管理员可以选择要备份的数据库，在凌晨 1:00 接下来的九个月的每一天。

调度程序允许您创建、 更新、 删除、 查看和使用的脚本，并在门户中，以编程方式管理作业和[作业集合](scheduler-concepts-terms.md)。

## 请参见

 [Azure 计划程序概念、 术语和实体层次结构](scheduler-concepts-terms.md)

 [开始在 Azure 门户使用 Azure 计划程序](scheduler-get-started-portal.md)

 [计划和 Azure 计划程序中的计费](scheduler-plans-billing.md)

 [如何构建复杂的调度和高级的定期使用 Azure 调度程序](scheduler-advanced-complexity.md)

 [Azure 计划其余部分 API 参考](https://msdn.microsoft.com/library/dn528946)

 [Azure 计划 PowerShell cmdlet 引用](scheduler-powershell-reference.md)

 [Azure 的计划程序的高可用性和可靠性](scheduler-high-availability-reliability.md)

 [Azure 计划限制、 默认值和错误代码](scheduler-limits-defaults-errors.md)

 [Azure 计划程序的出站身份验证](scheduler-outbound-authentication.md)
