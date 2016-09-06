---
ms.openlocfilehash: ea3de41b9dab52d9ce63fe384c666db12d34caaf
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="使用 StorSimple 快照管理器可以创建和管理卷组 |Microsoft Azure"
   description="介绍如何使用 StorSimple 快照管理器 mmc 管理单元来创建和管理卷组。"
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

# 使用 StorSimple 快照管理器来创建和管理卷组

## 概述

可以使用**作用域**窗格中的**卷组**节点可以将卷分配给卷组、 查看一个卷组有关的信息、 计划备份，和编辑卷组。 

卷组是用来确保应用程序一致性的备份相关的卷池。 有关详细信息，请参阅[卷和卷组](storsimple-what-is-snapshot-manager.md#volumes-and-volume-groups)和[Windows 卷阴影复制服务与集成](storsimple-what-is-snapshot-manager.md#integration-with-windows-volume-shadow-copy-service)。

>[AZURE.IMPORTANT] 卷组配置时，不要混合使用群集共享卷 (Csv) 和非 Csv 在同一卷组中。 StorSimple 快照管理器不支持 Csv 和非 Csv 的混合中的同一个快照。
 
![卷组节点](./media/storsimple-snapshot-manager-manage-volume-groups/HCS_SSM_Volume_groups.png)

**图 1: StorSimple 快照管理器卷组节点** 

本教程介绍了如何使用 StorSimple 快照管理器︰

- 查看有关您的卷组的信息 
- 创建一个卷组
- 备份卷组
- 编辑一个卷组
- 删除一个卷组

所有这些操作，还提供**在操作**窗格上。
 
## 查看卷组

如果您单击**卷组**节点时，**结果**窗格将显示有关每个卷组，这取决于所做的列选择的以下信息。 （在**结果**窗格中的列进行配置。 右键单击**卷**节点，选择**视图**，然后选择**添加/删除列**）。

结果列 | 说明 
:--------------|:------------ 
名称           | **名称**列中包含的卷组的名称。
应用程序	    | **应用程序**列显示当前已安装并运行在 Windows 主机上的 VSS 编写器。
选择       | **选定**列显示在卷组中所包含的卷的数目。 零 (0) 表示没有应用程序与该卷组中的卷相关联。
导入       | **导入**列显示导入卷的数目。 设置为**True**时，此列将指出卷组从 Microsoft Azure 管理门户导入并不是在 StorSimple 快照管理器。
 
>[AZURE.NOTE] 在 Azure 管理门户中的**备份策略**选项卡上，也会显示 StorSimple 快照管理器卷组。
 
## 创建一个卷组

使用以下过程创建一个卷组。

#### 若要创建一个卷组

1. 单击桌面图标以启动 StorSimple 快照管理器。 

2. 在**作用域**窗格中，右键单击**卷组**，然后单击**创建卷组**。 

    ![创建卷组](./media/storsimple-snapshot-manager-manage-volume-groups/HCS_SSM_Create_volume_group.png)
 
    显示**创建卷组**对话框。 

    ![创建卷组对话框](./media/storsimple-snapshot-manager-manage-volume-groups/HCS_SSM_CreateVolumeGroup_dialog.png) 

3.  输入以下信息︰ 

    1. 在**名称**框中，键入新的卷组的唯一名称。 

    2. 在**应用程序**框中，选择与要添加到卷组的卷关联的应用程序。 

        只有这些应用程序使用 Azure StorSimple 卷，并让 VSS 编写器为其启用**应用程序**框中列出。 只有当所有卷的编写器已意识到 Azure StorSimple 卷启用了 VSS 编写器。 如果应用程序框中为空，然后不安装任何应用程序都支持 VSS 编写器和使用 Azure StorSimple 卷。 （目前，Azure StorSimple 支持 Microsoft Exchange 和 SQL Server。有关 VSS 编写器的详细信息，请参阅[Windows 卷阴影复制服务与集成](storsimple-what-is-snapshot-manager.md#integration-with-windows-volume-shadow-copy-service)。

        如果您选择的应用程序，将自动选择与之相关联的所有卷。 相反，如果您选择使用特定应用程序相关联的卷时，该应用程序会自动选中**应用程序**框中。 

    3. 在**卷**框中，选择 Azure StorSimple 卷添加到卷组。 

      - 您可以包括单个或多个分区的卷。 （多个分区卷可以是动态磁盘或基本磁盘带有多个分区。包含多个分区的卷被视为一个单元。 因此，如果只有其中一个分区添加到卷组，所有其他分区会自动添加到该卷组在同一时间。 将多个分区卷添加到卷组之后，多个分区卷继续被视为一个单元。

      - 您可以通过不向其分配任何卷创建空卷组。 

      - 不要混合使用群集共享卷 (Csv) 和非 Csv 在同一卷组中。 StorSimple 快照管理器不支持混合使用 CSV 卷和非 CSV 卷中的同一个快照。 

4. 单击**确定**以保存该卷组。

## 备份卷组

使用下面的过程备份卷组。

#### 要备份的卷组

1. 单击桌面图标以启动 StorSimple 快照管理器。

2. 在**作用域**窗格中，展开**卷组**节点，右键单击一个卷组名称，，然后单击**执行备份**。 

    ![立即备份卷组](./media/storsimple-snapshot-manager-manage-volume-groups/HCS_SSM_Take_backup.png)

3. 在**执行备份**对话框中，选择**本地快照**或**云快照**，，然后单击**创建**。 

    ![采用备份对话框](./media/storsimple-snapshot-manager-manage-volume-groups/HCS_SSM_TakeBackup_dialog.png) 

4. 要确认备份正在运行，请展开**作业**节点，然后单击**运行**。 应列出备份。

5. 若要查看已完成的快照，展开**备份目录**节点，展开卷组名，然后单击**本地快照**或**云快照**。 如果它成功完成，则会列出备份。 

## 编辑一个卷组

请按下列步骤编辑一个卷组。

#### 若要编辑一个卷组

1. 单击桌面图标以启动 StorSimple 快照管理器。

2. 在**作用域**窗格中，展开**卷组**节点，右键单击一个卷组名称，然后单击**编辑**。 

3. 显示**创建卷组**对话框。 您可以更改**名称**、**应用程序**和**卷**项。 

4. 单击**确定**以保存所做的更改。

## 删除一个卷组

使用以下过程删除一个卷组。 

>[AZURE.WARNING] 这也会删除关联的卷组中的所有备份。

#### 若要删除一个卷组

1. 单击桌面图标以启动 StorSimple 快照管理器。 

2. 在**作用域**窗格中，展开**卷组**节点，右键单击一个卷组名称，然后单击**删除**。 

3. 显示**删除卷组**对话框。 在文本框中，键入**确认**，然后单击**确定**。 

    从**结果**窗格的列表中消失的已删除的卷组，并从备份目录中删除与该卷组相关联的所有备份。

## 下一步行动

[使用 StorSimple 快照管理器可以创建和管理备份策略](storsimple-snapshot-manager-manage-backup-policies.md)。
