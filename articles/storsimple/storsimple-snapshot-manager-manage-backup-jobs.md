---
ms.openlocfilehash: e662ceffef1e363d94c7337f7465ed34f3814c12
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="使用 StorSimple 快照管理器来查看和管理备份作业 |Microsoft Azure"
   description="介绍如何使用 StorSimple 快照管理器 mmc 管理单元来查看和管理计划，当前正在运行和已完成的备份作业。"
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


# 使用 StorSimple 快照管理器可以查看和管理备份作业

## 概述

在**作用域**窗格中的**作业**节点显示**计划**、**过去 24 小时**和**运行**的备份任务启动交互方式或通过配置的策略。 

本教程介绍如何使用**作业**节点以显示有关计划、 最新的并且当前正在运行备份作业的信息。 （作业和相应的信息的列表显示在**结果**窗格中。此外，可以用鼠标右键单击列出的作业，并查看上下文菜单，其中列出了可用的操作。

## 查看已计划的作业

使用以下过程以查看计划备份作业。

#### 若要查看已排定的作业

1. 单击桌面图标以启动 StorSimple 快照管理器。 

2. 在**作用域**窗格中，展开**作业**节点，然后单击**计划**。 在**结果**窗格中显示以下信息︰

    - **名称**– 计划的快照的名称

    - **下次运行**的日期和时间的下一步计划快照

    - **上一次运行**的日期和时间的最新计划快照

    >[AZURE.NOTE] 对于一次性仅快照，**下次运行**和**最后一次运行**将是相同的。 
 
    ![计划的备份作业](./media/storsimple-snapshot-manager-manage-backup-jobs/HCS_SSM_Jobs_scheduled.png) 
 
3. 若要在一个特定的作业上执行其他操作，用鼠标右键单击**结果**窗格中的作业名称，从菜单选项中选择。

## 查看最近使用的作业

使用以下过程可查看备份和还原在过去 24 小时中已完成的作业。

#### 若要查看最近使用的作业

1. 单击桌面图标以启动 StorSimple 快照管理器。

2. 在**作用域**窗格中，展开**作业**节点中，并单击**最后一个 24 小时**。 **结果**窗格显示最近 24 小时内 （到 64 作业的最大） 备份作业。 在**结果**窗格根据您指定的**视图**选项中显示以下信息︰

    - **名称**– 计划的快照的名称。
 
    - **已开始**-日期和时间开始快照时。

    - **已停止**-日期和时间时该快照已完成或已终止。

    - 时间**已过去**– 之间**已启动**和**停止**的时间量。

    - **状态**--在最近完成的作业的状态。 **成功**指示已成功创建备份。 **失败**表明该作业未成功运行。

    - **信息**-失败的原因。

    - **字节处理 (MB)** --（单位为 Mb) 处理，该卷组中的数据量。 

    ![在过去的 24 小时运行的作业](./media/storsimple-snapshot-manager-manage-backup-jobs/HCS_SSM_Jobs_Last_24_hours.png) 

3. 若要在一个特定的作业上执行其他操作，用鼠标右键单击**结果**窗格中的作业名称，从菜单选项中选择。

    ![删除作业](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Delete_backup.png) 
     
## 查看当前正在运行的作业

使用以下过程来查看当前正在运行的作业。

#### 若要查看当前正在运行的作业

1. 单击桌面图标以启动 StorSimple 快照管理器。

2. 在**作用域**窗格中，展开**作业**节点，然后单击**运行**。 根据您指定的**视图**选项，在 [**结果**] 窗格中显示以下信息︰ 

    - **名称**– 计划的快照的名称。

    - **已开始**-日期和时间开始快照时。

    - **检查点**– 当前的备份操作。

    - **状态**-完成的百分比。
    
    - **经过**– 备份开始后经过的时间量。 

    - **平均吞吐量 (MB)** — 中庸的数据量传送，以兆字节 (Mb) 为单位表示。

    - **字节处理 (MB)** --（单位为 Mb) 处理，该卷组中的数据量。

    - **个字节写入 (MB)** -写入 （以 Mb) 备份的数据量。

    ![当前正在运行的作业](./media/storsimple-snapshot-manager-manage-backup-jobs/HCS_SSM_Jobs_running.png)

3. 若要在一个特定的作业上执行其他操作，用鼠标右键单击**结果**窗格中的作业名称，从菜单选项中选择。

## 下一步行动

[使用 StorSimple 快照管理器来管理备份的目录](storsimple-snapshot-manager-manage-backup-catalog.md)。















            


 

