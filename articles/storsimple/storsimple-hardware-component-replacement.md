---
ms.openlocfilehash: cf7826035cf314f72217394984859eb9a4b66dcf
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="StorSimple 组件更换硬件 |Microsoft Azure"
   description="描述如何安全地更换 PCMs、 电池、 控制器模块、 EBOD 控制器、 磁盘驱动器和 StorSimple 设备的机箱。"
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

# StorSimple 组件更换硬件

## 概述

硬件组件更换教程介绍 Microsoft Azure StorSimple 设备和拆下并更换它们的必要步骤的硬件组件。 这篇文章描述的安全图标、 提供指向详细教程，并列出了可更换的组件。

>[AZURE.IMPORTANT] 然后再尝试删除或替换任何 StorSimple 组件，确保您检查[安全图标约定](#safety-icon-conventions)和其他[安全预防措施](storsimple-safety.md)。
 
### 安全设置图标约定

下表描述了这些教程中使用的安全图标。 请注意这些安全图标所需执行的步骤，删除和替换设备组件。

| 图标 | 文字 | 其他信息 |
|:---- |:---- |:-----------|
|![警告图标](./media/storsimple-hardware-component-replacement/Warning.png)| **危险 ！** | 表示，如果无法避免，会导致死亡或严重伤害的危险情况。 这个信号词仅限于最极端的情况下。|
|![警告图标](./media/storsimple-hardware-component-replacement/Warning.png)| **警告 ！** | 表示，如果无法避免，会导致死亡或严重伤害的危险情况。|
|![警告图标](./media/storsimple-hardware-component-replacement/Caution.png)| **小心 ！** |表示，如果无法避免，可能会导致小或中等复杂程度的人身伤害的危险情况。|
|![通知图标](./media/storsimple-hardware-component-replacement/NoticeIcon.png)| **注意︰** | 指示被视为很重要，但不是会造成危险的信息。|
|![电击图标](./media/storsimple-hardware-component-replacement/Electric.png) | **电击危险** | 表明高电压。|
|![粗粗细图标](./media/storsimple-hardware-component-replacement/Weight.png)| **粗粗细**| |
|![没有用户可维修的部件图标](./media/storsimple-hardware-component-replacement/NoUserServiceableParts.png)| **没有用户可维修的部件** | 不能访问除非经过适当的培训。|
|![阅读说明图标](./media/storsimple-hardware-component-replacement/ReadInstructions.png)|**请先阅读所有说明**| |
|![提示危险图标](./media/storsimple-hardware-component-replacement/TipHazard.png)|**提示危险**| |

### 在开始之前

熟悉有关您在本教程中使用的设备和安全图标的安全信息。 有关完整的信息，请转到[安全地安装和运行您的 StorSimple 设备](storsimple-safety.md)。 请务必处理 StorSimple 设备之前，请查看[安全预防措施](storsimple-safety.md#handling-precautions)。 

尝试更换组件之前，请考虑以下信息。

![警告图标](./media/storsimple-hardware-component-replacement/Warning.png)![电击图标](./media/storsimple-hardware-component-replacement/Electric.png)**警告 ！** 

- 使您自己接地正确处理模块和组件的 StorSimple 设备时使用的静电放电或防静电垫。

- 请勿触摸任何电路。 处理组件，可能会泄露电路时，使用提供的句柄和参考线。

![警告图标](./media/storsimple-hardware-component-replacement/Warning.png)![注意图标](./media/storsimple-hardware-component-replacement/NoticeIcon.png)**注意︰**

当您更换的模块，**永远不会离开空托架中存储模块的后端**。 删除问题部分之前获得替换或空白的模块。

## 硬件组件更换过程

您的 Microsoft Azure StorSimple 设备由主和/或 EBOD 箱中的几个插件模块组成。 8100 具有单一的主盘柜，而 8600 是双盘柜设备与主盘柜和 EBOD 箱。

下表总结了在设备中的主要硬件组件。 请单击要转到相关的教程的**更换过程**列中的链接。

|组件|# 存在|插件模块？|更换过程
|:---------|:--------|:--------------|:---------------------|
| 机箱|1|否|[更换机箱 StorSimple 设备上](storsimple-chassis-replacement.md) |
|主控制器|2|是| [更换控制器模块 StorSimple 设备上](storsimple-controller-replacement.md) |
|764W 电源和冷却模块 (PCMs)|2|是| [在 StorSimple 设备更换的电源和冷却模块](storsimple-power-cooling-module-replacement.md) |
|备用电池|2|是| [更换备用电池模块 StorSimple 设备上](storsimple-battery-replacement.md) |
|磁盘驱动器|12|是| [更换您的 StorSimple 设备上的磁盘驱动器](storsimple-disk-drive-replacement.md) |

**表 1**在主盘柜中的硬件组件

主盘柜和 EBOD 存储模块不同的 I/O 模块。 此外，PCMs 具有不同的功率。 在主盘柜 PCMs 是 764 W，而 EBOD 存储模块中的那些是 580 w。主盘柜在 PCMs 中还包含一个备用电池模块。

|组件|# 存在|插件模块？| 更换过程
|:---------|:--------|:--------------|:---------------------|
|机箱|1|否| [更换机箱 StorSimple 设备上](storsimple-chassis-replacement.md) |
|EBOD 控制器|2|是| [更换您的 StorSimple 设备上的 EBOD 控制器](storsimple-EBOD-controller-replacement.md) |
|580W 电源和冷却模块 (PCMs)|2|是| [在 StorSimple 设备更换的电源和冷却模块](storsimple-power-cooling-module-replacement.md) |
|磁盘驱动器|12|是| [更换您的 StorSimple 设备上的磁盘驱动器](storsimple-disk-drive-replacement.md) |

**表 2**EBOD 存储模块中的硬件组件

下面的正面和背面图中突出显示设备上的插件模块。 这些关系图可用于确定各种插件模块的位置是否需要更换。 前图显示磁盘驱动器和 EBOD 存储模块和主盘柜放映后的图的插件模块。

![Frontplane 的磁盘驱动器的设备](./media/storsimple-hardware-component-replacement/IC741028.png)

**图 1** 在设备正面的

|对象中，从所有 TermSet 对象中返回不属于|说明|
|:----|:----------|
|0-11|磁盘驱动器 （共 12 个）|

主箱和 EBOD 盘柜有驱动器托架模块。 机箱可容纳 12 个 3.5 英寸磁盘驱动器以 3 4 格式排列。

![背板的设备主盘柜模块](./media/storsimple-hardware-component-replacement/IC740994.png)

**图 2** 背面主盘柜

|对象中，从所有 TermSet 对象中返回不属于|说明|
|:----|:----------|
|1|PCM 0|
|2|PCM 1|
|3|控制器 0|
|4|控制器 1|

![背板设备 EBOD 存储模块插件模块](./media/storsimple-hardware-component-replacement/IC769599.png)

**图 3** EBOD 存储模块背面

|对象中，从所有 TermSet 对象中返回不属于|说明|
|:----|:----------|
|1|PCM 0|
|2|PCM 1|
|3|EBOD 控制器 0|
|4|EBOD 控制器 1|

## 现场可替换单元

以下可现场更换单元 (Fru) StorSimple 设备有︰

- 机箱 （包括集成的操作面板）

- 764 W 交流电源和冷却模块

- 580 W 交流电源和冷却模块

- 硬磁盘驱动器与驱动器托架模块

- 控制器模块

- EBOD 控制器模块

- 备用电池模块

- 机架安装导轨套件

请[与 Microsoft 支持部门联系](storsimple-contact-microsoft-support.md)以订购任何这些替换的设备。

## 下一步行动

尝试更换 StorSimple 硬件组件之前，请检查所有[安全信息](storsimple-safety.md)。
