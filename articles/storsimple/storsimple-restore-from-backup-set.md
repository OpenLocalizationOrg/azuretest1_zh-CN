---
ms.openlocfilehash: c9d9f7213fbb9c03382a4ce0093e531b1b856a7d
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="从备份中还原 StorSimple 卷 |Microsoft Azure"
   description="说明如何使用 StorSimple 管理器服务备份目录页可以从备份集还原 StorSimple 卷。"
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
   ms.date="08/28/2015"
   ms.author="v-sharos" />

# 从备份集还原 StorSimple 卷

## 概述

**备份目录**页将显示执行手动或自动备份时创建的所有备份集。 您可以使用此页可以列出所有的备份的备份策略或选择一个卷或删除备份，或使用备份来恢复或克隆卷。

 ![备份目录页](./media/storsimple-restore-from-backup-set/HCS_BackupCatalog.png)

本教程介绍如何使用**备份目录**页可以从备份集还原您的设备。

## 如何使用备份目录 

**备份目录**页提供的查询，可帮助您缩小您的备份设置所选内容。 您可以筛选检索基于以下参数的备份集︰

- **设备**-设备创建的备份集。
- **备份策略**或**卷**— 卷与此备份集的备份策略。
- **从**和**到**– 创建备份集时的日期和时间范围。

已筛选的备份集是然后表格根据下列属性︰

- **名称**– 卷与备份集的备份策略的名称。
- **大小**— 备份集的实际大小。
- **创建日期**– 当创建备份的日期和时间。 
- **类型**– 可本地快照或云快照备份集。 本地快照是本地存储在设备上的所有卷数据的备份，而云快照是指驻留在云环境中的卷数据的备份。 本地快照提供了访问速度更快，而选择云快照数据可恢复性。
- **通过启动**– 根据计划或手动用户可以自动启动备份。 （您可以使用备份策略来计划备份。 或者，您可以使用**执行备份**选项进行交互式备份。）

## 如何从备份中还原 StorSimple 卷

**备份目录**页可用于从特定备份还原 StorSimple 卷。 但是请记住，，恢复卷将还原到备份时状态的卷。 备份操作之后添加的任何数据都将丢失。

> [AZURE.WARNING] 从备份中还原将替换现有备份卷。 这可能会导致备份之后写入任何数据丢失。


### 若要从备份集中恢复

1. 在 StorSimple 管理器服务页上，单击**备份目录**选项卡。

    ![备份目录](./media/storsimple-restore-from-backup-set/HCS_Restore.png)

2. 选择备份集，如下所示︰
  1. 选择适当的设备。
  2. 在下拉列表中，选择您想要选择备份的卷或备份策略。
  3. 指定的时间范围。
  4. 单击检查图标 ![检查图标](./media/storsimple-restore-from-backup-set/HCS_CheckIcon.png) 要执行该查询。
 
    与所选卷的备份或备份策略应显示在列表的备份集。

3. 展开要查看的相关联的卷的备份集。 这些卷必须脱机主机和设备上之前可以将其还原。 访问**卷容器**页上的卷，然后按照中[采取离线的卷](storsimple-manage-volumes.md#take-a-volume-offline)以使其脱机的步骤。

    >  [AZURE.IMPORTANT] 请确保已经记录了脱机主机上的卷首先之前您设备上采取离线的卷。 如果在主机上不会离线的卷，它可能会导致数据损坏。

4. 向后定位到**备份目录**选项卡上，选择一个备份集。

5. 单击页面底部的**还原**。

6. 系统将提示您进行确认。 

    ![确认页](./media/storsimple-restore-from-backup-set/HCS_ConfirmRestore.png)

7. 查看还原信息，然后单击检查图标![检查图标](./media/storsimple-restore-from-backup-set/HCS_CheckIcon.png)。 这将启动您可以通过访问**作业**页查看恢复作业。 

8. 在还原完成后，可以验证卷的内容由来自备份的卷。

## 下一步行动

了解如何[管理 StorSimple 卷](storsimple-manage-volumes.md)。
