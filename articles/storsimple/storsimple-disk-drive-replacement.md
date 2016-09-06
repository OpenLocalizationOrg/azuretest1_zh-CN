---
ms.openlocfilehash: ad36b7f367b5f002e199b8b8566760d9faef2987
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="替换磁盘驱动器上的 StorSimple 设备 |Microsoft Azure"
   description="介绍如何更换磁盘驱动器 StorSimple 主要设备或 EBOD 存储模块上。"
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carolz"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="08/31/2015"
   ms.author="alkohli" />

# 更换您的 StorSimple 设备上的磁盘驱动器

## 概述

本教程介绍了如何删除和更换出现故障或发生故障的硬磁盘驱动器的 Microsoft Azure StorSimple 设备上。 若要替换磁盘驱动器，您需要︰

- 松开 antitamper 锁

- 删除磁盘驱动器

- 安装替换的磁盘驱动器

>[AZURE.IMPORTANT] 之前卸下和更换的磁盘驱动器，查看[StorSimple 组件更换硬件](storsimple-hardware-component-replacement.md)中的安全信息。

## 松开 antitamper 锁

此过程说明如何可以占用或未占用时替换磁盘驱动器 StorSimple 设备上的 antitamper 锁。 Antitamper 锁在驱动器托架手柄，将调整，则其访问方式中的句柄的闩锁明小光圈。 提供驱动器设置为锁定位置锁。

#### 若要打开 antitamper 锁

1. 小心地插入 lock 键 （Microsoft 提供了一个"篡改"T10 螺丝刀） 到该句柄中口径和基座。 

    >[AZURE.NOTE] 如果激活 antitamper 锁，红色指示灯将会出现在光圈。

    ![锁定的磁盘驱动器](./media/storsimple-disk-drive-replacement/IC741056.png)

    **图 1**防篡改锁卡入到位

  	|对象中，从所有 TermSet 对象中返回不属于|说明|
  	|:----|:----------|
  	|1|指标口径|
  	|2|Antitamper 锁|

2. 旋转以逆时针方向的键，直到键上方口径中看不到红的指示器。

3. 删除的项。

    ![取消锁定的驱动器](./media/storsimple-disk-drive-replacement/IC741057.png)

    **图 2**取消锁定的驱动器

4. 现在可以删除磁盘驱动器。

按照按相反的顺序来进行锁定的步骤。

## 删除磁盘驱动器

StorSimple 设备支持 RAID 10 – 类似存储空间配置。 这意味着它可以有一个出现故障的磁盘，固态驱动器 (SSD) 正常工作，或硬盘驱动器 (HDD)。 

>[AZURE.IMPORTANT]
>
>- 如果系统中有多个出现故障的磁盘，不要删除多个 SSD 或硬盘从任何时刻系统的时间。 这样做可能会导致数据丢失。
>
>- 请确保将 SSD 替换放在以前包含 SSD 的插槽中。 同样，将以前包含硬盘插槽更换硬盘。
>
>- 在管理门户中，插槽编号从 0-11。 因此，如果该门户显示插槽 2 中的一个磁盘出现故障，在设备上，寻找第三个插槽中出现故障的磁盘从左上方。

驱动器可卸下和更换系统工作。

#### 若要删除驱动器

1. 若要确定出现故障的磁盘，在管理门户中，导航到**设备** > **维护** > **硬件状态**。 在主盘柜和/或 EBOD 存储模块 （如果您使用的 8600 模型） 中，磁盘可能会失败，因为查看磁盘**共享组件**和下**EBOD 存储模块共享组件**的状态。 任何一个存储模块中的故障的磁盘将显示红色状态。

2. 查找主盘柜或 EBOD 存储模块前面的驱动器。 故障磁盘，琥珀色指示灯将会亮起。

3. 如果磁盘已解锁，请继续执行下一步。 如果磁盘已被锁定，则解除其锁定[Disengage antitamper 锁](#disengage-the-antitamper-lock)中的过程。

4. 按驱动器托架模块，黑色的闩锁并将驱动器托架手柄抬出，拉出机箱的前面。 

    ![释放磁盘驱动器的句柄](./media/storsimple-disk-drive-replacement/IC741051.png)

    **图 3**松开驱动器句柄

5. 驱动器托架手柄拉开，驱动器托架滑出机箱。 

    ![滑动从磁盘驱动器的磁盘](./media/storsimple-disk-drive-replacement/IC741052.png)
    
    **图 4**滑动磁盘驱动器托架

## 安装替换的磁盘驱动器

驱动器发生故障 Microsoft Azure StorSimple 设备中，并已经将其删除后，请按照以下过程将其替换为新的驱动器。

#### 若要插入一个驱动器

1. 确保完全扩展驱动器托架手柄，如下图中所示。

    ![磁盘驱动器与扩展的句柄](./media/storsimple-disk-drive-replacement/IC741044.png)

    **图 5**驱动器与扩展的句柄

2. 驱动器托架滑入机箱中。 

    ![可调磁盘插入磁盘驱动器托架](./media/storsimple-disk-drive-replacement/IC741045.png)

    **图 6** 驱动器托架滑入机箱

3. 插入驱动器托架，同时继续将驱动器托架推入机箱，直到驱动器托架手柄将其锁定位置嵌入关闭驱动器托架手柄。

4. 通过打开锁螺钉四分之一顺时针到位使用由 Microsoft （篡改 Torx 螺丝刀） 力保托架手柄锁键。

5. 验证更换的成功，并通过访问管理门户并导航到**维护**操作的驱动器是 > **硬件状态**。 在**共享组件**或**EBOD 存储模块共享组件**，驱动器状态应该是绿色的表示它健康。

    >[AZURE.NOTE] 它可能需要几个小时要更换后变为绿色的磁盘状态。

## 下一步行动

了解有关[StorSimple 更换硬件组件](storsimple-hardware-component-replacement.md)的详细信息。