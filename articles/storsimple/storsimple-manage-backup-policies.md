---
ms.openlocfilehash: 6c4adf98afcb198784cdeac29618ba5a71d40c63
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="管理您的 StorSimple 备份策略 |Microsoft Azure"
   description="说明如何使用 StorSimple 管理器服务创建和管理手动备份、 备份时间表和备份的保留。"
   services="storsimple"
   documentationCenter="NA"
   authors="SharS"
   manager="carolz"
   editor=""/>
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="08/27/2015"
   ms.author="v-sharos"/>

# 使用 StorSimple 管理器服务来管理备份策略

## 概述

本教程介绍如何使用 StorSimple 管理器服务**备份策略**页来控制备份过程和 StorSimple 卷的备份保留。 它还介绍了如何完成手动备份。

**备份策略**页允许您管理备份策略，并安排本地和云快照。 （备份策略用于配置备份时间表和集合的卷的备份保留。）备份策略，您可以同时执行多个卷的快照。 这意味着备份创建的备份策略将故障一致性副本。 本页面列出了备份策略、 及其类型、 关联的卷、 备份保留的数量和启用这些策略的选项。

**备份策略**页也允许您筛选现有的备份策略由一个或多个以下字段︰

- **策略名称**– 与该策略关联的名称。 不同类型的策略包括︰

   - 计划的策略，由用户显式地创建。
   - 自动策略，在创建卷时启用了此卷选项的默认备份时创建的。 这些策略被命名为 VolumeName_Default，卷名称是指通过管理门户中的用户配置的 StorSimple 卷的名称。 自动策略导致每日云快照设备时间 22:30 开始。
   - 导入的策略，最初的创建在 StorSimple 快照管理器。 这些具有标记描述 StorSimple 快照管理器主机从导入策略。

- **卷**与策略相关联的卷。 创建备份时，备份策略相关联的所有卷都组合在一起。

- **上次成功备份**--日期和时间的拍摄与此策略的上次成功备份。

- **下一次备份**--日期和时间将由这项策略的下一步计划备份。

- **计划**– 与备份策略相关的计划的数量。

您可以从此页执行的常用的操作是︰

- 添加备份策略 
- 添加或修改计划 
- 删除备份策略 
- 执行手动备份 
- 使用多个卷和时间安排创建自定义的备份策略 

## 添加备份策略

添加自动安排的备份的备份策略。 管理门户添加备份策略 StorSimple 设备中执行以下步骤。 添加策略后，您可以定义一个计划 (请参阅[添加或修改计划](#add-or-modify-a-schedule)。

[AZURE.INCLUDE [storsimple-add-backup-policy](../../includes/storsimple-add-backup-policy.md)]


## 添加或修改计划

您可以添加或修改附加到 StorSimple 设备上现有的备份策略的计划。 管理门户添加或修改一个计划中执行以下步骤。

[AZURE.INCLUDE [storsimple-add-modify-backup-schedule](../../includes/storsimple-add-modify-backup-schedule.md)]

## 删除备份策略

在管理门户中删除 StorSimple 设备上的备份策略，请执行以下步骤。

[AZURE.INCLUDE [storsimple-delete-backup-policy](../../includes/storsimple-delete-backup-policy.md)]


## 执行手动备份

管理门户创建一个单独的卷点播 （手动） 备份中执行以下步骤。

[AZURE.INCLUDE [storsimple-create-manual-backup](../../includes/storsimple-create-manual-backup.md)]

## 使用多个卷和时间安排创建自定义的备份策略

若要创建一个自定义的备份策略，具有多个卷和时间安排的管理门户中执行以下步骤。

[AZURE.INCLUDE [storsimple-create-custom-backup-policy](../../includes/storsimple-create-custom-backup-policy.md)]


## 下一步行动

了解如何[使用 StorSimple 快照管理器来查看和管理备份作业](storsimple-snapshot-manager-manage-backup-jobs.md)。 