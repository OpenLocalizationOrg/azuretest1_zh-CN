---
ms.openlocfilehash: a6e18b21a4e8f3766d0127305f0bb6106973a4fd
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="创建自定义的 StorSimple 备份策略"
   description="说明如何使用 StorSimple 管理器服务以创建自定义的备份策略。"
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
   ms.date="08/13/2015"
   ms.author="v-sharos" />

#### 若要创建自定义的备份策略

1. 在**设备**页中，单击**备份策略**，然后单击**添加**。

2. 在**添加备份策略**对话框中，**定义您的备份策略**下︰

    1. 指定一个备份策略的名称。

    2. 选择要添加到此策略的卷。 您可以通过从下拉列表中选择添加多个卷。

    3. 单击检查图标 ![检查图标](./media/storsimple-add-backup-policy/HCS_CheckIcon-include.png).

     策略创建成功后，将通知您。 备份策略页也会更新以显示新创建的策略。

4. 单击策略名称 （第一列） 来向下钻取到刚创建的策略的详细信息。

5. 单击**管理计划**。

6. 在**管理计划**对话框中︰

    1. 选择**创建新**以添加另一个计划。

    2. 从下拉列表中，选择为**本地**或**云**快照的备份类型。

    3. 指定以分钟、 小时、 天或周的备份频率。

    4. 选择保留。 保留选项取决于备份的频率。 例如，对于日常的策略，保留可以指定以周为单位，而保留每月策略是以月为单位。
 
    5. 选择的开始时间和日期的策略。

    6. 选择复选框以启用该策略。

7. 单击检查图标 ![检查图标](./media/storsimple-add-backup-policy/HCS_CheckIcon-include.png) 若要完成。

8. 您将返回到策略的详细信息。 单击**保存**以保存对该策略所做的更改。 在保存策略时，您将收到通知。

9. 向后定位到**备份策略**页。 备份策略的表格式列表将被更新，以显示已修改的策略。

    ![自定义的备份策略](./media/storsimple-create-custom-backup-policy/HCS_CustomBackupPolicyM-include.png).


