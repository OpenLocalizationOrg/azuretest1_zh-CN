---
ms.openlocfilehash: fcf8aa8b1f16d9f26e4eabed1e7b208ddd3406ec
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="更换电池上的 StorSimple 设备 |Microsoft Azure"
   description="描述如何删除、 替换和维护 StorSimple 设备上的电池备份模块。"
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

# 更换备用电池模块 StorSimple 设备上

## 概述

主盘柜电源和冷却模块 (PCM) Microsoft Azure StorSimple 设备上有更多的电池组。 此包提供了电源，使 StorSimple 设备可以将数据保存到主盘柜的交流电源断电时。 这个电池包被称为*备用电池模块*。 备用电池模块只存在在 StorSimple 设备中 （EBOD 存储模块不包含备份电池模块） 主盘柜。 

本教程介绍如何︰

- 卸下电池备份模块 
- 安装新的电池备份模块
- 维护备份电池模块

>[AZURE.IMPORTANT] 在拆卸和更换备用电池模块之前, 查看[简介 StorSimple 组件更换硬件](storsimple-hardware-component-replacement.md)中的安全信息。

## 卸下电池备份模块

您的 Microsoft Azure StorSimple 设备的备用电池模块是可现场更换的单元。 它将安装在 PCM 之前，电池模块应存储在其原包装。

#### 要卸下备用电池模块

1. 在管理门户中，导航到**设备** > **维护** > **硬件状态**。 在**共享组件**看的电池的状态。

2. 确定电池未能 PCM。 图 1 显示了 StorSimple 设备的背面。

    ![背板的设备主盘柜模块](./media/storsimple-battery-replacement/IC740994.png)

    **图 1**主要设备的背面显示 PCM 和控制器模块

  	|对象中，从所有 TermSet 对象中返回不属于|说明|
  	|:----|:----------|
  	|1|PCM 0|
  	|2|PCM 1|
  	|3|控制器 0|
  	|4|控制器 1|

    如图 2 中的数字 3 所示，监控指标相对应的 PCM 0 到**电池故障**指示灯将亮起。

    ![背板的设备 PCM 监视指示灯](./media/storsimple-battery-replacement/IC740992.png)

    **图 2**PCM 的背面显示监视指示灯

  	|对象中，从所有 TermSet 对象中返回不属于|说明|
  	|:---|:-----------|
  	|1|交流电源出现故障|
  	|2|风扇故障|
  	|3|电池故障|
  	|4|PCM 确定|
  	|5|直流电源故障|
  	|6|电池状态良好|

3. 若要删除出现故障的电池与 PCM，请按照[删除 PCM](storsimple-power-cooling-module-replacement.md#remove-a-pcm)。

4. 删除 PCM，抬起并向上旋转电池模块手柄，如下图所示，然后将其拉到移除电池。

    ![PCM 从卸下电池](./media/storsimple-battery-replacement/IC741019.png)

    **图 3**PCM 从取出电池

5. 将模块放置在包装可现场更换的单元。

6. 返回的正确处理和处理 Microsoft 有缺陷的单位。

## 安装新的电池备份模块

执行以下步骤来安装更换电池模块 StorSimple 设备的主盘柜中。

#### 要安装电池模块

1. PCM 在正确的方向放置备用电池模块中。

2. 按下电池模块句柄一直要固定接头。

3. 通过[更换电源和 StorSimple 设备上冷却模块](storsimple-power-cooling-module-replacement.md)中的指南来替换主盘柜中的 PCM。

4. 完成更换后，访问管理门户和导航**设备**到 > **维护** > **硬件状态**，并验证以确保安装成功的电池的状态。 如果指示灯显示绿色状态，电池是正常的。

## 维护备份电池模块

在设备中，备用电池模块电源丢失事件期间提供电源控制器。 它允许将关键数据控制的方式关闭之前保存在 StorSimple 设备。 在 PCMs 中两个完全充电电池，使用该系统可以处理两个连续丢失事件。

在管理门户中，**维护**页上的**硬件状态**指明有故障的电池还是当即将淘汰。 由**PCM 0 中的电池**或**电池在 PCM 1** **共享组件**下指示电池状态。 此页面将显示**DEGRADED**状态，为接近结束的生命，并达到淘汰的**失败**。 

>[AZURE.NOTE] 它只需要充电时，电池可以报告**失败**。
 
如果显示**DEGRADED**状态，我们建议以下行动纲领︰

- 系统可能会遇到新的电源中断或电池可能正在进行定期维护。 在继续操作之前的 12 个小时观察系统。

    - 如果状态仍是后 12 个小时的连续连接到交流电源控制器和 PCMs 运行**性能下降**，则需要更换电池。 请更换备用电池模块[与 Microsoft 支持部门联系](storsimple-contact-microsoft-support.md)。

    - 如果状态变为确定在 12 小时后，电池是否运行，以及它只需要维护费用。

- 如果没有交流电源的相关的损失，PCM 已打开并连接到交流电源，电池需要更换。 [与 Microsoft 客户支持联系](storsimple-contact-microsoft-support.md)以订购替换备用电池模块。

>[AZURE.IMPORTANT] 根据国内和地方法规出现故障的电池的处置。 

## 下一步行动

了解有关[StorSimple 更换硬件组件](storsimple-hardware-component-replacement.md)的详细信息。
