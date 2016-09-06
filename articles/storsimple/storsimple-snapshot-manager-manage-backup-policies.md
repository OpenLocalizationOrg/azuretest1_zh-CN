---
ms.openlocfilehash: 1bd722692e562fd589e43475a57e31fabd840de8
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="使用 StorSimple 快照管理器可以创建和管理备份策略 |Microsoft Azure"
   description="介绍如何使用 StorSimple 快照管理器 mmc 管理单元来创建和管理控制定时的备份的备份策略。"
   services="storsimple"
   documentationCenter="NA"
   authors="SharS"
   manager="carolz"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="08/17/2015"
   ms.author="v-sharos" />

# 使用 StorSimple 快照管理器来创建和管理备份策略

## 概述

备份策略创建计划备份卷的数据，在本地或在云中。 创建一个备份策略时，您还可以指定保留策略。 （您可以保留最多 64 快照）。有关备份策略的详细信息，请参阅[备份类型和备份策略](storsimple-what-is-snapshot-manager.md#backup-types-and-backup-policies)。

本教程介绍如何︰

- 创建一个备份策略 
- 编辑备份策略 
- 删除备份策略 

## 创建一个备份策略

请按下列步骤创建新的备份策略。

#### 若要创建一个备份策略

1. 单击桌面图标以启动 StorSimple 快照管理器。

2. 在**作用域**窗格中，右键单击**备份策略**，然后单击**创建备份策略**。

    ![创建一个备份策略](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Create_BU_policy.png)

    **创建策略**对话框中出现。 

    ![创建策略的常规选项卡](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Create_policy_general.png)

3. 在**常规**选项卡，请填写以下信息︰

   1. 在**名称**文本框中，键入策略的名称。

   2. 在**卷组**文本框中，键入与该策略关联的卷组的名称。

   3. 选择**本地快照**或**云快照**。

   4. 选择要保留的快照数。 如果选择了**全部**，将为 64 快照保留 （最大）。 

4. 单击**计划**选项卡。

    ![创建策略的计划选项卡](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Create_policy_schedule.png)

5. 在**日程安排**选项卡，请填写以下信息︰ 

   1. 单击**启用**复选框来安排下一次备份。

   2. 在**设置**中，选择**一次**、**每日**、**每周**或**每月**。 

   3. 在**开始**文本框中，单击日历图标并选择一个开始日期。

   4. 在**高级设置**可以设置可选重复计划和结束日期。

   5. 单击**确定**。

创建一个备份策略后，在**结果**窗格中将显示以下信息︰

- **名称**– 备份策略的名称。

- **类型**--本地快照或云快照。

- **卷组**– 与该策略关联的卷组。

- **保留**– 保留; 的快照数最大值为 64。

- **创建**-创建该策略时的日期。

- **已启用**— 策略是否当前效果︰ **True**指示是有效;**False**指示它不起作用。 

## 编辑备份策略

请按下列步骤编辑一个现有的备份策略。

#### 若要编辑备份策略

1. 单击桌面图标以启动 StorSimple 快照管理器。 

2. 在**作用域**窗格中，单击**备份策略**节点。 所有备份的策略都将出现在**结果**窗格中。 

3. 右键单击您想要编辑的策略，然后单击**编辑**。 

    ![编辑备份策略](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Edit_BU_policy.png) 

4. **创建策略**窗口出现时，输入您的更改，然后单击**确定**。 

## 删除备份策略

使用以下过程删除备份策略。

#### 若要删除备份策略

1. 单击桌面图标以启动 StorSimple 快照管理器。 

2. 在**作用域**窗格中，单击**备份策略**节点。 所有备份的策略都将出现在**结果**窗格中。 

3. 右键单击您想要删除的备份策略，然后单击**删除**。 e
4. 出现确认消息时，请单击**是**。

    ![删除备份策略确认](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Delete_BU_policy.png)

## 下一步行动

[使用 StorSimple 快照管理器来查看和管理备份作业](storsimple-snapshot-manager-manage-backup-jobs.md)。