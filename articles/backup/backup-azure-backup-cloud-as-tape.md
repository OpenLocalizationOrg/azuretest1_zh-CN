---
ms.openlocfilehash: e0bfd9e5660bc633fa13b2c40f8a3c8071a4feea
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="使用 Azure 备份来替换您的磁带基础结构 |Microsoft Azure"
   description="了解如何备份 Azure 提供类似磁带的语义，这使您可以备份和还原在 Azure 中的数据"
   services="backup"
   documentationCenter=""
   authors="Jim-Parker"
   manager="jwhit"
   editor=""/>
<tags
   ms.service="backup"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="storage-backup-recovery"
   ms.date="07/01/2015"
   ms.author="jimpark"; "aashishr"/>

# 使用 Azure 备份来替换您的磁带基础架构
Azure 备份和系统中心 Data Protection Manager 的客户将能够︰
- 备份时间表的最适合他们的组织需要的数据
- 将备份数据保留更长的时间段
- 请长期保留的部分 （而不是磁带） 需要的 Azure。

本文介绍了客户如何启用备份和保留策略。 使用磁带来解决其 long term 保留客户现在需要有一个强大和可行的替代方案具有此功能的可用性。 在 Azure 备份最新版本中启用的功能 (位于[此处](http://aka.ms/azurebackup_agent))。 SCDPM 客户将需要使用此功能之前将移动到 UR5。

## 备份计划是什么？
备份时间表表示备份操作的频率。 例如，下面的屏幕中的设置指示每天在下午 6 时和午夜，将执行备份。

![每日计划](./media/backup-azure-backup-cloud-as-tape/dailybackupschedule.png)

客户还可以安排每周一次的备份。 例如，下面的屏幕中的设置说明，备份将每个备用星期天和星期三在上午 9: 30 和凌晨 1 点。

![每周计划](./media/backup-azure-backup-cloud-as-tape/weeklybackupschedule.png)

## 保留策略是什么？
保留策略指定的持续时间必须为其存储备份。 而不是只指定备份的所有点"平面策略"，客户可以指定在进行备份时根据不同的保留策略。 例如，每个季度结束时采取的备份点可能需要更长的持续时间为审核目的而执行每日备份时间点作为操作恢复点，需要保留 90 天保留。

![保留策略](./media/backup-azure-backup-cloud-as-tape/retentionpolicy.png)

在此策略中指定的"保留点"的总数为 90 （每磅为单位） + 40 (每一个 10 年的同期) = 130。

## 示例-将两者放在一起

![屏幕的示例](./media/backup-azure-backup-cloud-as-tape/samplescreen.png)

1. **每天保留策略**︰ 执行每日备份存储为 7 天。
2. **每周保留策略**︰ 在午夜，星期六下午 6 点进行日常的备份将保留 4 周
3. **每月保留策略**︰ 在午夜，每个月最后一个周六的下午 6 点的备份将保留供 12months
4. **每年的保留策略**︰ 在每年的三月最后一个星期六午夜的备份将保留 10 年

"保留点"总数 （客户可以从其恢复数据点） 上图中按以下方式计算︰

- 2 点，每日 7 天 = 14 恢复点
- 2 点每 4 周 = 8 周的恢复点
- 2 点每 12 个月 = 24 个月的恢复点
- 1 点，每年每 10 年 = 10 恢复点

恢复点的总数是 56。

## 高级的配置
通过在上面的屏幕中，单击**修改**，客户可以进一步指定保留时间安排的灵活性。

![Modify](./media/backup-azure-backup-cloud-as-tape/modify.png)

## 下一步行动
详细信息请参阅 Azure 备份

- [Azure 备份简介](backup-introduction-to-azure-backup.md)
- [请尝试 Azure 的备份](backup-try-azure-backup-in-10-mins)

测试
