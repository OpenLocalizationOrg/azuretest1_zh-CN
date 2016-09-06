---
ms.openlocfilehash: 93008c3c6b64cea38a2959535f256f793853de1a
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="替换的 StorSimple EBOD 控制器 |Microsoft Azure"
   description="解释如何删除和替换一个或两个 StorSimple 8600 设备上的 EBOD 控制器。"
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
   ms.date="08/12/2015"
   ms.author="alkohli" />

# 更换您的 StorSimple 设备上的 EBOD 控制器

## 概述

本教程介绍如何更换有缺陷的 EBOD 控制器模块 Microsoft Azure StorSimple 设备上。 若要替换的 EBOD 控制器模块，您需要︰

- 删除有问题的 EBOD 控制器
- 安装新的 EBOD 控制器

在开始之前，请考虑以下信息︰

- 空 EBOD 模块必须插入到所有未使用的插槽中。 如果一个插槽处于打开状态，将不正常冷却盘柜。

- EBOD 控制器是可热更换，可以删除或替换。 已更换前，请勿移除出现故障的模块。 当您启动更换过程时，必须在 10 分钟内完成它。

>[AZURE.IMPORTANT] 在拆卸和更换 EBOD 控制器之前, 查看[StorSimple 组件更换硬件](storsimple-hardware-component-replacement.md)中的安全信息。

## 删除 EBOD 控制器

在替换之前失败的 EBOD 控制器模块在 StorSimple 设备中，请确保其他 EBOD 控制器模块活动并且正在运行。 下面的过程和表说明了如何删除 EBOD 控制器模块。

#### 若要删除 EBOD 模块

1. 打开管理门户网站。

2. 导航到**设备** > **维护** > **硬件状态**，并验证活动的 EBOD 控制器模块指示灯的状态为绿色，以及失败的 EBOD 控制器模块指示灯呈红色。

3. 找到位于该设备失败的 EBOD 控制器模块。

4. 删除 EBOD 模块从系统中取出前连接到控制器的 EBOD 控制器模块的缆线。

5. 记下确切的 SAS 端口已连接到控制器的 EBOD 控制器模块。 将要求您替换 EBOD 模块后，将系统还原到此配置。 

    >[AZURE.NOTE] 通常情况下，这将是在下图中标记为**在主机**上的端口 A。

    ![背板的 EBOD 控制器](./media/storsimple-ebod-controller-replacement/IC741049.png)

     **图 1**EBOD 模块背面

  	|对象中，从所有 TermSet 对象中返回不属于|说明|
  	|:----|:----------|
  	|1|故障指示灯|
  	|2|电源指示灯|
  	|3|连接的 SAS 接口|
  	|4|SAS 指示灯|
  	|5|仅供工厂使用的串行端口|
  	|6|端口 （主机）|
  	|7|端口 B （主机出）|
  	|8|端口 C （仅供工厂使用）|

## 安装新的 EBOD 控制器

下面的过程和表介绍了如何在 StorSimple 设备中安装 EBOD 控制器模块。

#### 安装 EBOD 控制器

1. 请检查有损坏，尤其是接口连接器 EBOD 设备。 如果针脚弯曲，请不要安装新的 EBOD 控制器。

2. 与拉开的闩锁，将模块滑入机箱直到闩锁接合。

    ![安装 EBOD 控制器](./media/storsimple-ebod-controller-replacement/IC741050.png)

    **图 2**安装 EBOD 控制器模块

3. 关闭闩锁。 您应听到咔嗒声如闩。

    ![释放闩锁 EBOD](./media/storsimple-ebod-controller-replacement/IC741047.png)

    **图 3**关闭 EBOD 模块的锁闩

4. 重新连接电缆。 使用替换之前存在的具体配置。 下图和下表有关如何将电缆连接的详细信息，请参阅。

    ![4U 设备的电源线](./media/storsimple-ebod-controller-replacement/IC770723.png)

    **图 4** 重新连接缆线

  	|对象中，从所有 TermSet 对象中返回不属于|说明|
  	|:----|:----------|
  	|1|主盘柜|
  	|2|PCM 0|
  	|3|PCM 1|
  	|4|控制器 0|
  	|5|控制器 1|
  	|6|EBOD 控制器 0|
  	|7|EBOD 控制器 1|
  	|8|EBOD 存储模块|
  	|9|配电设备|

## 下一步行动

了解有关[StorSimple 更换硬件组件](storsimple-hardware-component-replacement.md)的详细信息。