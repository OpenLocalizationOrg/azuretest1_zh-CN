---
ms.openlocfilehash: 2f42e687cd8fe0955428b429b5a4f2e907ea8cc8
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="管理您的 StorSimple 备份目录 |Microsoft Azure"
   description="说明如何使用 StorSimple 管理器服务备份目录页列表，选择，并删除了卷的备份集。"
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
   ms.date="09/01/2015"
   ms.author="v-sharos" />

# 使用 StorSimple 管理器服务来管理您的备份目录

## 概述

StorSimple 管理器服务**备份目录**页将显示执行手动或自动备份时创建的所有备份集。 您可以使用此页可以列出所有的备份的备份策略或选择一个卷或删除备份，或使用备份来恢复或克隆卷。

本教程说明如何列出、 选择，和删除备份集。 若要了解如何从备份中还原您的设备，请转到[还原您的设备的备份集](storsimple-restore-from-backup-set.md)。 若要了解如何克隆卷，请转到[克隆 StorSimple 卷](storsimple-clone-volume.md)。

![备份目录](./media/storsimple-manage-backup-catalog/HCS_BackupCatalog.png) 

**备份目录**页提供了一个查询以缩小备份设置所选内容。 您可以筛选检索时，根据以下参数的备份集︰

- **设备**-设备创建的备份集。

- **备份策略或卷**— 卷与此备份集的备份策略。

- **From 和 To** – 创建备份集时的日期和时间范围。

已筛选的备份集是然后表格根据下列属性︰

- **名称**– 卷与备份集的备份策略的名称。

- **大小**— 备份集的实际大小。

- **创建**在日期和时间创建备份时。 

- **类型**– 可本地快照或云快照备份集。 本地快照是本地存储在设备上的所有卷数据的备份，而云快照是指驻留在云环境中的卷数据的备份。 本地快照提供了访问速度更快，而选择云快照数据可恢复性。

- **启动者**– 按计划或手动用户可以自动启动备份。 可以使用备份策略来计划备份。 或者，可以使用**执行备份**选项以进行交互式的备份。

## 为卷设置列表备份
 
完成以下步骤来列出所有的备份卷。

#### 对列表的备份集

1. 在 StorSimple 管理器服务页上，单击**备份目录**选项卡。

2. 选项，如下所示的筛选器︰

    1. 选择适当的设备。

    2. 在下拉列表中，选择要查看的相应备份的卷。

    3. 指定的时间范围。

    4. 单击检查图标 ![检查图标](./media/storsimple-manage-backup-catalog/HCS_CheckIcon.png) 要执行该查询。
 
    与所选的卷备份应出现在备份集的列表。

## 选择备份集

完成以下步骤以选择一个备份集卷或备份策略。

#### 若要选择备份集

1. 在 StorSimple 管理器服务页上，单击**备份目录**选项卡。

2. 选项，如下所示的筛选器︰

    1. 选择适当的设备。

    2. 在下拉列表中，选择您想要选择备份的卷或备份策略。

    3. 指定的时间范围。

    4. 单击检查图标 ![检查图标](./media/storsimple-manage-backup-catalog/HCS_CheckIcon.png) 要执行该查询。

    与所选卷的备份或备份策略应显示在列表的备份集。

3. 选择并展开一个备份集。 **还原和删除**选项将显示在页面底部。 您可以在所选的备份集执行上述任一操作。

## 删除备份集

如果希望不再保留与它关联的数据，则删除备份。 执行以下步骤来删除一个备份集。

#### 若要删除备份集

1. 在 StorSimple 管理器服务页上，单击**备份目录选项卡上**。

2. 选项，如下所示的筛选器︰

    1. 选择适当的设备。

    2. 在下拉列表中，选择您想要选择备份的卷或备份策略。

    3. 指定的时间范围。

    4. 单击检查图标 ![检查图标](./media/storsimple-manage-backup-catalog/HCS_CheckIcon.png) 要执行该查询。

    与所选卷的备份或备份策略应显示在列表的备份集。

3. 选择并展开一个备份集。 **还原和删除**选项将显示在页面底部。 单击**删除**。

4. 在进度和成功完成时删除操作时，您将收到通知。 完成删除操作后，请刷新此网页上的查询。 已删除备份集将不再显示在列表的备份集。

## 下一步行动

了解如何使用还原[您的设备从备份集](storsimple-restore-from-backup-set.md)的备份目录。
