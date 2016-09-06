---
ms.openlocfilehash: 55c1163c4295d65cabad58e1b3d92c092fe45307
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="在 Azure 自动化计划 |Microsoft Azure"
   description="使用自动化计划安排在 Azure 自动化功能以自动启动运行手册。  本文介绍如何创建日程。"
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
   ms.date="08/18/2015"
   ms.author="bwren" />

# 在 Azure 自动化的计划

使用自动化计划来安排自动运行的运行手册。  这可能是一个单个日期和时间运行一次 runbook。  也可以是定期计划启动 runbook 多次。  计划通常不能从访问运行手册。

## Windows PowerShell Cmdlet

下表中的 cmdlet 用于创建和管理 Windows PowerShell Azure 自动化中的变量。 他们寄送为[Azure PowerShell 模块](../powershell-install-configure.md)的一部分。

|Cmdlet|说明|
|:---|:---|
|[获得 AzureAutomationSchedule](http://msdn.microsoft.com/library/dn690274.aspx)|检索一个时间表。|
|[新 AzureAutomationSchedule](http://msdn.microsoft.com/library/dn690271.aspx)|创建一个新计划。|
|[删除 AzureAutomationSchedule](http://msdn.microsoft.com/library/dn690279.aspx)|删除一个计划。|
|[一组 AzureAutomationSchedule](http://msdn.microsoft.com/library/dn690270.aspx)|设置现有计划的属性。|
|[获得 AzureAutomationScheduledRunbook](http://msdn.microsoft.com/library/dn913778.aspx)|检索计划运行手册。|
|[注册 AzureAutomationScheduledRunbook](http://msdn.microsoft.com/library/dn690265.aspx)|将 runbook 与一个计划关联起来。|
|[取消注册 AzureAutomationScheduledRunbook](http://msdn.microsoft.com/library/dn690273.aspx)|Dissociates 计划从 runbook。|

## 创建新的计划

### 若要使用 Azure 门户创建一个新计划


1. 从自动化您的帐户，请单击窗口顶部的**资产**。
1. 在窗口的底部，单击**添加设置**。
1. 单击**添加时间表**。
1. 完成该向导，请单击该复选框以保存新的变量。

### 若要使用 Azure 预览门户创建一个新计划

1. 从自动化您的帐户，单击以打开刀片式服务器**资产**的**资产**部分。
1. 单击以打开**计划**刀片式服务器**计划**部分。
1. 单击顶部的刀片式服务器**添加时间表**。
1. 完成窗体并单击**创建**以保存新的计划。

### 若要使用 Windows PowerShell 创建新的计划

[新建 AzureAutomationSchedule](http://msdn.microsoft.com/library/dn690271.aspx) cmdlet 创建一个新计划，并设置为现有计划。  下面的示例 Windows PowerShell 命令创建新的计划名为我的每日日程安排，明天在中午开始，并激发每一天年︰

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "My Daily Schedule"
    $startTime = (Get-Date).Date.AddDays(1).AddHours(12)
    $expiryTime = $startTime.AddYears(1)
    
    New-AzureAutomationSchedule –AutomationAccountName $automationAccountName –Name $scheduleName –StartTime $startTime –ExpiryTime $expiryTime –DayInterval 1


## 请参见
- [计划在 Azure 自动化 runbook](automation-scheduling-a-runbook.md)
 
测试
