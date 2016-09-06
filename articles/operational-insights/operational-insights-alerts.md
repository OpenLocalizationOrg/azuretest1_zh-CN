---
ms.openlocfilehash: 19fe3e998a3930fe19117f6e800035a74228e9e6
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="从操作管理器查看预警"
   description="了解如何为被监视的服务器基础结构中操作管理器管理警报"
   services="operational-insights"
   documentationCenter=""
   authors="bandersmsft"
   manager="jwhit"
   editor="" />
<tags
   ms.service="operational-insights"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/05/2015"
   ms.author="banders" />



# 查看操作管理器警报

[AZURE.INCLUDE [operational-insights-note-moms](../../includes/operational-insights-note-moms.md)]

可以使用警报管理中 Microsoft Azure 运营见解之前，您必须安装该解决方案。 若要阅读更多关于安装解决方案，请参阅[设置您的工作区](operational-insights-setup-workspace.md)。 该解决方案只能通过向运营经理代理监视您的服务器时。 关于使用的运营洞察力的运营经理的信息，请参阅[连接到系统中心操作管理器从运营经验](operational-insights-connect-scom.md)。

安装解决方案后，您可以通过**警报管理**平铺在运营经验**概述**仪表板监视服务器查看通知。

![警报管理平铺的图像](./media/operational-insights-alerts/overview-alert.png)


您可以查看和管理 Microsoft Azure 运营洞察力警报从**通知**面板。 在仪表板，报警信息将显示按以下类别︰

- 活动警报
    - 关键警报
    - 警告警报
    - 通知来源
- 所有的活动通知
    - 显示百分比的警报都是关键，警告和信息性。
- 常见的警报查询
    - 此区域包含预构建的查询创建运营经验的软件开发团队可以帮助您开始使用预警。


警报会告诉您，检测到一个问题，警报会影响哪些服务器、 优先级和警报的名称。

![警报的仪表板的图像](./media/operational-insights-alerts/alert-drilldown1.png)

![警报的仪表板的图像](./media/operational-insights-alerts/alert-drilldown2.png)


在**警报管理**仪表板，您可以查看 Microsoft Azure 运营见解已找到的所有警报。

## 若要查看操作的见解警报

1. 在**概述**页中，单击**警报管理**拼贴。
2. 在**警报管理**仪表板，查看警报的类别并选择一个要使用的。
3. 单击平铺或任何项以在**搜索**页中查看有关它的详细的信息。
4. 通过使用所找到的信息，可以调查警报并确定可能需要解决其采取的其他操作。

[AZURE.INCLUDE [operational-insights-export](../../includes/operational-insights-export.md)]
