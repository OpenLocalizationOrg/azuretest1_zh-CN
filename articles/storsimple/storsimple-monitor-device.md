---
ms.openlocfilehash: c3a858713dfc222215760b9b2f7b26f2c6c51f14
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="监视您的 StorSimple 设备 |Microsoft Azure"
   description="介绍如何使用 StorSimple 管理器服务监视 I/O 性能、 容量利用率、 网络吞吐量和设备性能。"
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carolz"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="09/02/2015"
   ms.author="alkohli" />

# 使用 StorSimple 管理器服务来监视您的 StorSimple 设备 

## 概述

StorSimple 管理器服务可用于监视 StorSimple 解决方案中的特定设备。 您可以创建自定义图表基于 I/O 性能、 容量利用率、 网络吞吐量和设备性能指标。 

Azure 门户中查看某个特定设备的监控信息，选择 StorSimple 管理器服务，请单击**监视器**选项卡，然后从设备列表中选择。 **监视器**页面包含以下信息。

## 输入/输出性能 

**输入/输出性能**跟踪指标写入操作之间或者 iSCSI 发起程序接口主机服务器和设备或设备和云与相关的读取数。 这一性能可以衡量特定卷、 一个特定卷的容器，或卷中的所有容器中。

下面的图表显示设备，生产设备的所有卷到启动器的 IOs。 绘制的指标读取和写入字节 / 秒、 读取和写入每秒 IO 操作和读取和写入延迟。

![从发起到设备的 IO 性能](./media/storsimple-monitor-device/StorSimple_IO_Performance_For_InitiatorTODevice_For_AllVolumesM.png)

对于相同的设备，用于将数据从设备到云卷的所有容器，绘制 IOs。 在此设备上，数据仅为线性层和任何已洒到云。 没有发生从设备到云的读写操作。 因此，在图表中的峰值是心跳会检查设备和服务之间的频率所对应的时间间隔为 5 分钟。 

![从云设备 IO 性能](./media/storsimple-monitor-device/StorSimple_IO_Performance_For_DeviceTOCloud_For_AllVolumeContainersM.png)


对于相同的设备，云快照拍摄卷数据，下午 2:00 开始。 这将导致数据从设备传输到云。 读写都曾担任过云对此持续时间。 IO 图表显示在拍摄快照时的时间所对应的各种统计数据的峰值。 

![云后云快照设备的 IO 性能](./media/storsimple-monitor-device/StorSimple_IO_Performance_For_DeviceTOCloud_For_AllVolumeContainers2M.png)


## 容量利用率 

**产能利用率**将跟踪数据存储空间使用的卷、 卷容器或设备的量相关的度量标准。 您可以创建基于主存储、 云存储或存储设备的容量利用率报告。 在特定卷、 特定卷容器或卷中的所有容器，可以衡量产能利用率。

主，云服务器和存储设备的容量可以说明如下︰

- **主存储容量利用率**显示之前消除重复数据并将数据压缩到 StorSimple 卷写入的数据量。 以下图表显示了 StorSimple 设备的主存储容量利用率，云快照拍摄前后。 考虑到这是不仅是卷数据，云快照不应更改主存储。 如您所见，该图表将显示主产能利用率因拍摄云快照不会改变。 请注意，云启动快照在约下午 2:00 在该设备上。

    ![主产能利用率在云快照之前](./media/storsimple-monitor-device/StorSimple_PrimaryCapacityUtil_For_AllVolumes2M.png)
    
    ![在云快照后主容量利用率](./media/storsimple-monitor-device/StorSimple_PrimaryCapacityUtil_For_AllVolumes1M.png)


- **云存储容量利用率**显示使用云存储量。 消除重复此数据并将其压缩。 此数量包括云快照可能包含数据，并不反映在任何主要卷并保留的旧版或需要保留目的。 可以比较主和云存储占用图，以了解数据缩减率，尽管数量不是准确。 下表显示了云存储容量利用率之前的 StorSimple 设备和云快照备份之后。 在大约下午 2:00 在该设备上启动时云快照，您可以看到云容量利用率拍摄在同一时间，从 5.73 MB 增加到 4.04 GB。

    ![在云快照之前云容量利用率](./media/storsimple-monitor-device/StorSimple_CloudCapacityUtil_For_AllVolumeContainers2M.png)

    ![云在云快照后的容量利用率](./media/storsimple-monitor-device/StorSimple_CloudCapacityUtil_For_AllVolumeContainers1M.png)


- **设备存储容量利用率**显示设备，这将是多主存储利用率，因为它包含了 SSD 线性层的总利用率。 这一层包含大量数据上还存在该设备的其他层。 SSD 线性层中的产能，以便在新数据到达时，旧数据移动到硬盘层 （此时它消除重复和压缩） 和以后云循环。

    随着时间的推移主产能利用率和设备产能利用率很有可能增加在一起直到数据开始进行分层到云。 到那时，设备产能利用率将可能开始稳定较水平，但主要的产能利用率将提高写入更多数据时。

    以下图表显示了 StorSimple 设备的主存储容量利用率，云快照拍摄前后。 在下午 2:00 开始的云快照和设备容量利用率开始减少在那时。 设备存储容量利用率关闭了从 11.58 GB 为 7.48 GB。 这表明，很可能是未压缩的数据的线性 SSD 层中被消除重复、 压缩以及移入硬盘层。 请注意，是否设备已有的 SSD 和 HDD 层中的数据量大，您可能无法看到此下降。 在此示例中，设备具有少量的数据。

    ![前云快照设备容量利用率](./media/storsimple-monitor-device/StorSimple_DeviceCapacityUtil2M.png)

    ![之后云快照设备容量利用率](./media/storsimple-monitor-device/StorSimple_DeviceCapacityUtil1M.png)


## 网络吞吐量

**网络吞吐量**跟踪相关的主机服务器和设备以及设备和云之间从 iSCSI 发起程序网络接口传输的数据量的指标。 您的设备上，可以为每个 iSCSI 网络接口监视此统计数据。

下面的图表显示为 0 的数据和数据 4 1 千兆以太网网络接口设备上的网络吞吐量。 在此情况下，0 是云-启用数据而数据 4 已启用 iSCSI 的。 您可以看到 StorSimple 设备的入站和出站通信。 请注意图表从下午 3:24 中的平线限于对我们收集的数据仅为每个 5 分钟，并应忽略的事实。 

![Data4 的网络吞吐量](./media/storsimple-monitor-device/StorSimple_NetworkThroughput_Data0M.png)

![Data4 的网络吞吐量](./media/storsimple-monitor-device/StorSimple_NetworkThroughput_Data4M.png)


## 设备性能 

**设备性能**跟踪与您的设备的性能有关的指标。 下面的图表显示生产中设备的 CPU 使用率统计信息。

![设备的 CPU 利用率](./media/storsimple-monitor-device/StorSimple_DeviceMonitor_DevicePerformance1M.png)

## 下一步行动

[了解如何使用 StorSimple 管理器服务设备仪表板](storsimple-device-dashboard.md)。
