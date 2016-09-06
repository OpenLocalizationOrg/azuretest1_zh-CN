---
ms.openlocfilehash: e84bef80cc4cfde53379c31fe547765197baf65c
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="计划在 Azure 自动化 runbook"
   description="描述如何创建在 Azure 自动化中的计划，以便您可以自动启动 runbook 在某一特定时间或定期计划。"
   services="automation"
   documentationCenter=""
   authors="bwren"
   manager="stevenka"
   editor="tysonn" />
<tags 
   ms.service="automation"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="06/30/2015"
   ms.author="bwren" />

# 计划在 Azure 自动化 runbook

若要安排在 Azure 自动化在指定时间启动 runbook，您将其链接到一个或多个计划。 运行一次或定期每隔指定的数目的小时数或天数，可以配置计划。 Runbook 可以被链接到多个计划，和计划可以有多个链接到它的运行手册。

## 创建计划

您可以使用 Azure 管理门户或 Windows PowerShell 创建新的计划。 您还可以创建新的计划，runbook 链接到使用 Azure 管理门户的计划时。

### 若要使用 Azure 管理门户创建一个新计划

1. 在 Azure 管理门户中，选择自动化，然后单击自动化帐户的名称。
1. 选择**资源**选项卡。
1. 在窗口的底部，单击**添加设置**。
1. 单击**添加时间表**。
1. 键入一个**名称**和 （可选）**描述**新的日程。
1. 选择是否计划将运行**一次**，**每小时**或**每日**。
1. 指定**开始时间**和其他选项，这取决于您选择的计划类型。

### 若要使用 Windows PowerShell 创建新的计划

可以使用[New AzureAutomationSchedule](http://msdn.microsoft.com/library/azure/dn690271.aspx) cmdlet 可以在 Azure 自动化中创建新的计划。 必须指定计划，是否它运行一次，每小时或每日的开始时间。

下面的示例命令显示如何创建新运行的计划，每天下午 3:30 开始于 2015 年 1 月 20 日。

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-DailySchedule"
    New-AzureAutomationSchedule –AutomationAccountName $automationAccountName –Name $scheduleName –StartTime "1/20/2015 15:30:00" –DayInterval 1

## 链接到 runbook 的计划

Runbook 可以被链接到多个计划，和计划可以有多个链接到它的运行手册。 如果 runbook 参数，然后可以为其提供值。 您必须提供所有必需的参数值，并可能提供的任何可选参数的值。  每次通过此计划启动 runbook 时，会使用这些值。  您可以将相同的 runbook 附加到另一个计划并指定不同的参数值。

### 若要链接到 Azure 管理门户网站 runbook 的计划

1. 在 Azure 管理门户中，选择**自动化**，然后单击自动化帐户的名称。
1. 选择**运行手册**选项卡。
1. 单击 runbook 计划的名称。
1. 单击**计划**选项卡。
2. 如果 runbook 不到计划的当前链接，然后将为您提供选项为**链接到新的计划**或**链接到现有的计划**。  如果 runbook 当前链接到一个时间表，请单击**链接**位于窗口的底部来访问这些选项。
1. 如果 runbook 参数，系统将提示您输入其值。  

### 若要链接到与 Windows PowerShell runbook 的计划

[注册 AzureAutomationScheduledRunbook](http://msdn.microsoft.com/library/azure/dn690265.aspx)可用于链接到 runbook 的计划。 您可以使用参数参数指定 runbook 参数的值。 指定参数值的详细信息，请参阅[启动 Runbook Azure 自动化中](automation-starting-a-runbook.md)。

下面的示例命令显示如何链接到与参数 runbook 的计划。

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Test-Runbook"
    $scheduleName = "Sample-DailySchedule"
    $params = @{"FirstName"="Joe";"LastName"="Smith";"RepeatCount"=2;"Show"=$true}
    Register-AzureAutomationScheduledRunbook –AutomationAccountName $automationAccountName –Name $runbookName –ScheduleName $scheduleName –Parameters $params

## 禁用一个时间安排

禁用计划时，链接到任何运行手册将不再运行该计划。 可以手动禁用时间表或它们在创建时设置过期时间的每小时和每日计划。 当达到过期时间时，该计划将被禁用。

### 要禁用时间表与 Azure 管理门户

您可以禁用时间表的计划详细信息页从 Azure 管理门户中的计划。

1. 在 Azure 管理门户中，选择自动化，然后单击自动化帐户的名称。
1. 选择资源选项卡。
1. 单击要打开其详细信息的页面的计划的名称。
2. 将**启用**更改为**否**。

### 若要禁用 Windows PowerShell 的日程

[一组 AzureAutomationSchedule](http://msdn.microsoft.com/library/azure/dn690270.aspx) cmdlet 可用于更改现有计划的属性。 若要禁用时间表，指定**false** **启用**参数。

下面的示例命令演示如何禁用调度。

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-DailySchedule"
    Set-AzureAutomationSchedule –AutomationAccountName $automationAccountName –Name $scheduleName –IsEnabled $false

## 相关的文章

- [在 Azure 自动化计划资产](http://msdn.microsoft.com/library/azure/dn940016.aspx)
- [开始在 Azure 自动化 Runbook](automation-starting-a-runbook.md)测试
