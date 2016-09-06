---
ms.openlocfilehash: 2c7c2555174631e8aa2b582dfce21f0baf9f1bae
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="进行备份"
   description="描述如何定义 StorSimple 备份策略。"
   services="storsimple"
   documentationCenter="NA"
   authors="SharS"
   manager="adinah"
   editor="tysonn" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="04/01/2015"
   ms.author="v-sharos" />

### 以进行备份

1. 在设备**快速启动**页上，单击**添加备份策略**。 这将启动添加备份策略向导。 

2. 在**定义您的备份策略**页上︰
  1. 提供名称，其中包含为您的备份策略的 3 和 150 个字符之间。
  2. 选择要备份的卷。 如果您选择多个卷，这些卷将被组合在一起以创建故障一致性的备份。
  3. 单击箭头图标 ![箭头图标](./media/storsimple-take-backup/HCS_ArrowIcon-include.png). 
  
    ![添加备份策略](./media/storsimple-take-backup/HCS_AddBackupPolicyWizard1M-include.png)

3. 在**定义日程安排**页中︰
  1. 从下拉列表中选择备份的类型。 恢复速度更快，选择**本地快照**。 数据可恢复性，为选择**云快照**。
  2. 指定以分钟、 小时、 天或周的备份频率。
  3. 选择保留时间。 保留选项取决于备份的频率。 例如，对于日常的策略，保留可以指定以周为单位，而每月策略的保留是以月为单位。
  4. 选择的开始时间和日期的备份策略。
  5. 选择**启用**复选框以启用该备份策略。 
  6. 单击检查图标 ![检查图标](./media/storsimple-take-backup/HCS_CheckIcon-include.png) 若要保存该策略。

    ![添加备份策略](./media/storsimple-take-backup/HCS_AddBackupPolicyWizard2M-include.png)
 
     现在可以将创建的卷数据的定时的备份的备份策略。

您已完成该设备的配置。 


