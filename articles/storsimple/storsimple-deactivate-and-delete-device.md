---
ms.openlocfilehash: 0d57264dd0b05a35553a7a7a131068a32cb7a699
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="停用和删除 StorSimple 设备 |Microsoft Azure"
   description="描述如何从服务中删除 StorSimple 设备的第一次停用，然后将其删除。"
   services="storsimple"
   documentationCenter=""
   authors="SharS"
   manager="carolz"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/01/2015"
   ms.author="v-sharos" />

# 停用和删除 StorSimple 设备

本教程说明如何从服务中删除 StorSimple 设备的停用它，然后将其删除。

>[AZURE.NOTE] 您可以删除它之前，必须停用设备。

## 停用设备

您可能希望使故障设备。 在这种情况下，该设备将需要将其停用。 停用服务器设备和相应的 StorSimple 管理器服务之间的连接。 

>[AZURE.WARNING] 停用将是永久性的操作，并且不能撤消。 已停用的设备不能与 StorSimple 管理器服务注册，除非第一次重置工厂。 

当您停用设备时，被存储在本地设备的任何数据将不再可访问。 仅在云中存储设备相关联的数据可以恢复。 一旦停用，需要被重置之前可重复使用的现有或新服务的工厂设备。 工厂重置过程删除本地存储在设备的所有数据。 因此，非常重要之前停用设备进行云快照的所有数据。 这将允许您在以后的阶段中恢复所有数据。 

对于 StorSimple 的虚拟设备，停用将删除虚拟机和其设置时创建的资源。 虚拟设备被停用后，它不能恢复到以前的状态。 停用 StorSimple 虚拟设备之前，请确保停止或删除客户端和依赖于该虚拟设备的主机。

### 停用和删除数据

如果您感兴趣完全删除设备并不想保留的设备数据，然后执行下列操作︰  

1. 停用设备之前, 必须删除所有卷的容器 （和卷） 与该设备相关联。 已删除关联的备份后，您只能删除卷的容器。

2. 停用设备。 有关说明，请转到[步骤停用](#steps-to-deactivate)。

3. 停用后，可以完全删除该设备。 有关说明，请转到[删除设备](#delete-a-device)。

### 停用并保留数据

如果感兴趣删除该设备，但希望保留设备数据，然后执行下列操作︰  

1. 停用设备。 卷的所有容器和设备的快照将保持。 有关说明，请转到[步骤停用](#steps-to-deactivate)。

2. 您现在可以在卷容器和相关联的快照。 有关过程，请转到[StorSimple 设备的故障切换和灾难恢复](storsimple-device-failover-disaster-recovery.md)。

3. 后停用和故障切换，可以完全删除该设备。 有关说明，请转到[删除设备](#delete-a-device)。

### 若要停用的步骤

使用下面的过程中停用的设备中删除它的准备。

#### 若要停用设备

1. 在 StorSimple 管理器服务**设备**页上，选择您想要停用，并在页面的底部，单击**停用**该设备。

2. 将显示一条确认消息。 单击**是**以继续。 停用进程将启动，并需要几分钟才能完成。

    在 StorSimple 的虚拟设备，停用将导致以下操作︰

      - 删除 StorSimple 虚拟设备。

      - OSDisk 和 StorSimple 虚拟设备创建的数据磁盘被删除。

      - 托管服务和虚拟资源调配期间创建的网络将保留。 如果您没有使用这些实体，则应手动删除它们。

      - 云由 StorSimple 虚拟设备创建的快照将被保留。

<!--After the device is deactivated, you will need to perform a failover before you can delete it completely. For failover instructions, go to [Failover and disaster recovery for your StorSimple device](storsimple-device-failover-disaster-recovery.md).-->
 
## 删除设备

您可以删除那些已经停用的设备。 删除一个设备会删除列表中的设备连接到该服务。 该服务可以不再管理已删除的设备。

#### 要删除设备

1. 在 StorSimple 管理器服务**设备**页上，选择您想要删除一个已停用的设备。

2. 在页的底部，单击**删除**。

3. 系统将提示您进行确认。 单击**是**以继续。

它可能需要几分钟时间，要删除的设备。

## 下一步行动
若要还原为出厂默认值已停用的设备，请转到[重置为出厂默认设置的设备](storsimple-manage-device-controller.md#reset-the-device-to-factory-default-settings)。

获得技术支持，[请联系 Microsoft 技术支持](storsimple-contact-microsoft-support.md)。