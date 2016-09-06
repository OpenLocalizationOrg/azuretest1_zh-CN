---
ms.openlocfilehash: e542c0d94437a2ea2a66469c69ed76ef0bc77026
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="操作的见解功能指南"
    description="操作的见解是使 IT 管理员能够获得深入了解跨内部和云环境的分析服务。 它使您能够与计算机实时和历史数据，以快速开发自定义的见解，并为分析数据提供了 Microsoft 和社区开发模式。"
    services="operational-insights"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="operational-insights"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/06/2015"
    ms.author="banders"/>

# 操作的见解功能指南

[AZURE.INCLUDE [operational-insights-note-moms](../../includes/operational-insights-note-moms.md)]

这篇文章可以帮助您了解解决哪些问题可以帮助运营洞察力、 运营洞察力环境由组成的以及如何创建操作见解帐户和登录到该服务。

## 转换的机器数据

操作的见解是使 IT 管理员能够获得深入了解跨内部和云环境的分析服务。 它使您能够与计算机实时和历史数据，以快速开发自定义的见解，并为分析数据提供了 Microsoft 和社区开发模式。

与运营经验，可以为具有以下功能的操作情报转换机数据。


|**图标** | **要使用的内容** | **它的作用**|
|---|---|---|
|![](./media/operational-insights-feature-guide/cap-plan.png) | [容量规划](operational-insights-capacity.md) | 可以使用 Microsoft Azure 运营见解中的容量规划解决方案可帮助您了解您的服务器基础架构的容量。 |
| ![](./media/operational-insights-feature-guide/update.png) | [系统更新评估](operational-insights-updates.md) | 可以使用 Microsoft Azure 运营洞察力在系统更新解决方案可帮助您对您的基础架构中的服务器应用缺少的更新。 |
| ![](./media/operational-insights-feature-guide/log-search.png) | [日志搜索](operational-insights-search.md) | 日志搜索功能用于创建对变换、 筛选器和报表的查询结果。 搜索使用事件数据和 IIS 日志日志搜索整个运营的见解。 |
| ![](./media/operational-insights-feature-guide/malware.png) | [恶意软件评估](operational-insights-antimalware.md) | 可以使用 Microsoft Azure 运营洞察力在反恶意软件解决方案可帮助您保护您的基础架构中的服务器不受恶意软件。 |
| ![](./media/operational-insights-feature-guide/sec-audit.png) | [安全和审核](operational-insights-security-audit.md) | 您可以使用安全和审核解决方案全面了解您的组织的 IT 安全状况与需要您注意的显著问题的内置的搜索查询。 |
| ![](./media/operational-insights-feature-guide/assessment.png) | [活动目录和 SQL 评估](operational-insights-assessment.md) | 评估解决方案可用于定期评估风险和服务器环境的运行状况。 |
| ![](./media/operational-insights-feature-guide/alert.png) | [警报管理](operational-insights-alerts.md) | 警报管理解决方案可用于从服务器监视的操作管理器管理警报。 |


您还可以︰

- **计算机将数据发送到系统使用或不使用代理或结合系统中心操作管理器**。 有关详细信息，请参阅︰
    - [从系统中心操作管理器连接到操作的见解](operational-insights-connect-scom.md)
    - [计算机直接连接到操作的见解](operational-insights-direct-agent.md)
    - [分析中 Microsoft Azure 服务器中的数据](operational-insights-analyze-data-azure.md)
- **执行以上各项在路上使用移动应用程序**
    - 有关 Windows Phone 应用程序的详细信息，请参阅[操作见解移动应用程序](http://www.windowsphone.com/en-us/store/app/operational-insights/4823b935-83ce-466c-82bb-bd0a3f58d865)。

## 建议的操作环境

操作的见解环境组成︰

- Microsoft Azure 托管工作区是 Azure 帐户的容器
- 在云中托管运营洞察力 web 服务
- 直接连接到 web 服务或者单独代理
- 或附加到系统中心操作管理器，但不需要服务


如果您使用早期版本的操作调用系统中心顾问的见解，可能必须审查软件安装在您的本地环境。 但是，审查软件不支持与运营经验。

作为运营经理使用操作见解软件服务包括一个或多个管理组和每个管理组的至少一个代理。 操作管理器代理从服务器收集数据并进行分析通过使用解决方案 （类似于在系统中心操作管理器管理包）。 定期发送数据从运营经理到运营洞察力的 web 服务 （如果需要，并通过代理服务器传递），不包含任何内容被存储在任何操作管理器数据库中，因此没有额外的负载放置在它们上。

同样，在单独的计算机上安装的代理可以直接连接到 web 服务发送收集到的数据进行处理。

每个解决方案中的数据是分析、 编制索引，并运营的见解门户中显示。 您可以查看所有通知和相关补救指南、 配置评估、 基础结构容量问题、 系统更新状态、 反恶意软件警告和日志数据。 您还可以执行详细的临时日志搜索和研究。

![操作的见解概述关系图的图像](./media/operational-insights-feature-guide/environment.png)

### 哪里有操作可用的见解？
在美国位于 Microsoft Azure 运营的见解。 尽管运营洞察力的语言是英语，服务中提供了大量更多的市场。 有关信息，请参阅[国际可用性](http://go.microsoft.com/fwlink/?LinkId=229842)。
