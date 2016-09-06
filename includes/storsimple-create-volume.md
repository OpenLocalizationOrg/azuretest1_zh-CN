---
ms.openlocfilehash: f67903ef41600756a2e48c983648026589f5a0aa
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="创建卷"
   description="解释如何添加 StorSimple 设备上的卷。"
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
   ms.date="8/21/15"
   ms.author="v-sharos" />

#### 若要创建卷

1. 在设备**快速启动**页上，单击**添加卷**。 添加卷向导将启动。

2. 在添加卷向导中，在**基本设置**下︰
   1. 键入卷的**名称**。
   2. 指定卷的**配置的容量**。 **卷容量必须介于 1 GB 和 64 TB 之间。**
   3. 在下拉列表中，选择卷的**使用类型**。 对于较少经常访问归档数据，选择**存档卷**。 对于所有其他类型的数据，选择**分层卷**。 （分层的卷了以前称为主要卷）。
   4. 单击箭头图标 ![箭头图标](./media/storsimple-create-volume/HCS_ArrowIcon-include.png) 若要转到的下一页。

     ![添加卷](./media/storsimple-create-volume/HCS_AddVolume1M-include.png)

3. 在**其他设置**对话框中，添加一个新的访问控制记录 (ACR):
   1. 为您的 ACR 提供**名称**。
   2. 在**iSCSI 启动器名称**，提供了 iSCSI 限定名称 (IQN) 的 Windows 主机。 如果您没有 IQN，转到[获取 Windows Server 主机的 IQN](#get-the-iqn-of-a-windows-server-host)。
   3. 在**默认备份此卷？**，选择**启用**复选框。 默认备份会创建一个策略，22:30 （设备时间） 每一天执行，创建该卷云快照。

     > [AZURE.NOTE] 在此处启用备份后，它无法恢复。 您将需要编辑要修改此设置的卷。

     ![添加卷](./media/storsimple-create-volume/HCs_AddVolume2M-include.png)

4. 单击检查图标 ![检查图标](./media/storsimple-create-volume/HCS_CheckIcon-include.png). 将与指定的设置创建一个卷。



