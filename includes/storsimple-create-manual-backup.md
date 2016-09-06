---
ms.openlocfilehash: 84742897bca65f43d4bb8d1e5c95367039f8c0cd
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="创建手动备份"
   description="说明如何启动手动、 按需备份作业。"
   services="storsimple"
   documentationCenter="NA"
   authors="SharS"
   manager="carolz"
   edito**r="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="08/13/2015"
   ms.author="v-sharos" />

#### 若要创建手动备份

1. 在**设备**页中，请转到**备份策略**选项卡。 此选项卡列出以表格的形式，包括您要备份的卷的策略的所有备份策略。

2. 除第一列的相应行中，单击任意位置选择的策略。 在页面的底部，单击**备份**。 该按钮将展开以显示备份选项︰ 本地快照和云快照。 

3. 当您选择其中一个选项时，系统将提示您进行确认。 单击**是**。 

    ![创建手动备份](./media/storsimple-create-manual-backup/HCS_CreateManualBackup1-include.png)
 
    这将启动一个作业来创建快照。 成功创建该作业后，您会看到页面底部的通知。

4. 要监视作业，请单击**查看作业**在通知区域 （在页面的底部）。 

    ![监视手动备份](./media/storsimple-create-manual-backup/HCS_CreateManualBackup2-include.png)

5. 在备份作业完成后，请转到**备份目录**选项卡。

6. 将筛选器选项设置为适当的设备，备份策略和时间范围。 单击检查图标 ![检查图标](./media/storsimple-create-manual-backup/HCS_CheckIcon-include.png) 之后设置筛选器。

  备份应显示在目录中显示的备份集列表中。
