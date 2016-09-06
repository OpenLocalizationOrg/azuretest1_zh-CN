---
ms.openlocfilehash: 34439f372531a58f11f961a92e72ec15a61cef23
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="打开或关闭您的 StorSimple 设备 |Microsoft Azure"
   description="说明如何打开新的 StorSimple 设备、 设备已关闭或断电，打开和关闭正在运行的设备。"
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
   ms.date="08/19/2015"
   ms.author="alkohli" />

# 打开或关闭您的 StorSimple 设备 

## 概述

关闭 Microsoft Azure StorSimple 设备并不需要为系统正常运行的一部分。 但是，可能需要打开新的设备或设备不得不关闭。 通常情况下，在其中必须更换故障的硬件、 物理移动单位，或使故障设备的情况下需要关闭。 本教程介绍了开启和关闭 StorSimple 设备在不同情况下的必需的过程。

下表列出了用于打开和关闭您的 StorSimple 设备的各种方案，并提供适当的过程的链接。

|方案|参考主题|
|:-------|:---------------|
|打开一个新的设备|[打开一个新的设备](#turn-on-a-new-device)<ul><li>[新设备与主盘柜](#new-device-with-primary-enclosure-only)</li><li>[新的设备，采用 EBOD 盒](#new-device-with-ebod-enclosure)</li></ul>|
|打开后关闭设备|[打开后关闭设备](#turn-on-a-device-after-shutdown)<ul><li>[与主盘柜设备](#device-with-primary-enclosure-only)</li><li>[采用 EBOD 盒的设备](#device-with-ebod-enclosure)</li></ul>|
|在断电后打开设备|[在断电后打开设备](#turn-on-a-device-after-a-power-loss)<ul><li>[与主盘柜设备](#8100)</li><li>[采用 EBOD 盒的设备](#8600)</li></ul>|
|打开设备后主盘柜和 EBOD 连接已丢失|[后主在设备上打开和 EBOD 存储模块连接已丢失](#turn-on-a-device-after-the-primary-and-EBOD-enclosure-connection-is-lost)|
|关闭正在运行的设备|[请关闭正在运行的设备](#turn-off-a-running-device)<ul><li>[与主盘柜设备](#8100a)</li><li>[采用 EBOD 盒的设备](#8600a)</li></ul>|

## 打开一个新的设备

根据设备是 8100 还是 8600 模型，首次打开 Microsoft Azure StorSimple 设备的步骤不同。 8100 具有单一的主盘柜，而 8600 是双盘柜设备与主盘柜和 EBOD 箱。 以下各节介绍了两种模型的详细的步骤。

- [新设备与主盘柜](#new-device-with-primary-enclosure-only)

- [新的设备，采用 EBOD 盒](#new-device-with-ebod-enclosure)

### 新设备与主盘柜

该 StorSimple 8100 模型是单个盘柜设备。 您的设备包括冗余电源和冷却模块 (PCMs)。 同时 PCMs 必须安装和连接到不同的电源以确保高可用性。

执行以下步骤以便将连接您的设备的电源。

[AZURE.INCLUDE [storsimple-cable-8100-for-power](../../includes/storsimple-cable-8100-for-power.md)]

>[AZURE.NOTE]完成设备安装和电缆连接说明，请转到[安装 StorSimple 8100 设备](storsimple-8100-hardware-installation.md)。 请确保严格按照说明进行操作。

### 新的设备，采用 EBOD 盒

StorSimple 8600 模型具有主盘柜和 EBOD 箱。 这就需要进行一个电缆连接在一起，用于串行连接 SCSI (SAS) 连接和电源的单位。

第一次设置此设备，请为 SAS 电缆首先执行这些步骤，然后完成电源缆线连接的步骤。

[AZURE.INCLUDE [storsimple-sas-cable-8600](../../includes/storsimple-sas-cable-8600.md)]

[AZURE.INCLUDE [storsimple-cable-8600-for-power](../../includes/storsimple-cable-8600-for-power.md)]

>[AZURE.NOTE]完成设备安装和电缆连接说明，请转到[安装 StorSimple 8600 设备](storsimple-8600-hardware-installation.md)。 请确保严格按照说明进行操作。

## 打开后关闭设备

启用 Microsoft Azure StorSimple 设备后它已关闭的步骤是根据设备是 8100 还是 8600 模型不同。 8100 具有单一的主盘柜，而 8600 是双盘柜设备与主盘柜和 EBOD 箱。

- [与主盘柜设备](#device-with-primary-enclosure-only)

- [采用 EBOD 盒的设备](#device-with-ebod-enclosure)

### 与主盘柜设备

关机后使用下面的过程打开主盘柜和没有 EBOD 存储模块的 StorSimple 设备上。

#### 若要打开主盘柜的设备上

1. 确保电源切换两个电源冷却模块 (PCMs) 处于关闭位置。 如果交换机不在关闭位置，然后侧向到 OFF 位置，然后等待指示灯熄灭。

2. 通过翻转在两个位置对 PCMs 上的电源开关打开设备。 应打开该设备。

3. 检查以下项目，验证设备已完全打开︰

    1. 在这两个 PCM 模块确定指示灯呈绿色。

    2. 两个控制器上的状态指示灯呈稳定绿色。

    3. 闪烁蓝色指示灯上的一个控制器，它指示控制器处于活动状态。

    如果不满足以上任一条件，然后您的设备不是健康的。 请[与 Microsoft 支持部门联系](storsimple-contact-microsoft-support.md)。

### 采用 EBOD 盒的设备

关机后使用下面的过程打开主盘柜和 EBOD 存储模块的 StorSimple 设备上。 所述完全按顺序执行每个步骤。 如果不这样做可能会导致数据丢失。

#### 若要打开主和 EBOD 盘柜设备

1. 请确保 EBOD 存储模块连接到主盘柜。 有关详细信息，请参阅[安装 StorSimple 8600 设备](storsimple-8600-hardware-installation.md)。

2. 确保电源和冷却模块 (PCMs) 的 EBOD 和主盘柜处于关闭位置。 如果交换机不在关闭位置，然后侧向到 OFF 位置，然后等待指示灯熄灭。

3. EBOD 存储模块第一个通过启用翻转在两个位置对 PCMs 上的电源开关。 PCM 指示灯应呈绿色。 该设备上的绿色 EBOD 控制器指示灯指示 EBOD 存储模块上。

4. 通过翻转在两个位置对 PCMs 上的电源开关打开主盘柜。 现在应该在整个系统。

5. 请验证 SAS 指示灯呈绿色，从而保证 EBOD 箱和主存储模块之间的连接良好。

## 在断电后打开设备

电源中断可以关闭 Microsoft Azure StorSimple 设备。 电源的一个或两个电源上，电源中断可能会发生。 恢复步骤会有所不同取决于设备是 8100 或 8600 模型。 8100 具有单一的主盘柜，而 8600 是双盘柜设备与主盘柜和 EBOD 箱。 本部分描述了每个方案的恢复过程。

- [与主盘柜设备](#8100)

- [采用 EBOD 盒的设备](#8600)

### 与主盘柜设备 <a name="8100">

到一个其电源断电时，系统可以继续其正常运行。 但是，要确保设备的高可用性，请恢复供电电源越早越好。

如果电源中断或两个电源上的电源中断，系统将关闭以有序和受控的方式。 恢复电源后，系统将自动开启。

### 采用 EBOD 盒的设备 <a name="8600">

#### 在一个电源上的电源中断提供

到一个主盘柜或 EBOD 机箱上的电源断电时，系统可以继续正常操作。 但是，要确保设备的高可用性，请还原电源与电源连接越早越好。

#### 上主和 EBOD 箱两个电源上的电源中断

如果电源中断或两个电源上的电源中断，EBOD 盘柜将立即关闭并主盘柜将可控有序地关闭。 恢复电源后，装置将自动启动。

如果手动关闭电源，然后采取以下步骤来恢复系统供电。

1. 打开 EBOD 箱。

2. EBOD 存储模块后，打开主盘柜。

### EBOD 存储模块上的两个电源上的电源中断

当您设置了您的电缆时，您必须确保 EBOD 从未到不同的 PDU 的单独连接。 如果 EBOD 和主盘柜失败一次，系统将恢复。

如果只有两个电源上，EBOD 存储模块出现故障，系统将自动恢复。 执行以下步骤以打开系统，并将其恢复到正常状态︰

1. 如果开启了主盘柜，关闭电源和冷却模块 (PCMs)。

2. 等待几分钟以关闭系统。

3. 打开 EBOD 箱。

4. EBOD 存储模块后，打开主盘柜。

## 后主在设备上打开和 EBOD 存储模块连接已丢失

如果备用控制器和相应的 EBOD 控制器之间失去连接，设备继续工作。 如果系统活动控制器和相应的 EBOD 控制器之间的连接已丢失，应发生故障转移，该设备应继续能正常工作。

这两种串行连接 SCSI (SAS) 电缆被删除或 EBOD 箱和主存储模块之间的连接将被切断时，设备将停止工作。 在这种情况下，请执行以下步骤。

### 若要打开设备后连接已丢失

1. 访问设备的背面。

2. 如果 EBOD 存储模块之间的主盘柜的 SAS 电缆连接中断，EBOD 存储模块上的所有 SAS 航道指示灯都将熄灭。

3. 关闭 EBOD 盘柜和主盘柜的电源和冷却模块 (PCMs)。

4. 等待，直到这两个存储模块背面的所有指示灯都熄灭。

5. 重新插入 SAS 电缆，并确保 EBOD 盘柜和主盘柜连接良好。

6. EBOD 存储模块第一个通过启用这两个 PCM 转换开关到开位置。

7. 确保已启用的 EBOD 存储模块通过检查绿色指示灯不亮。

8. 打开主盘柜。

9. 确保已启用的主盘柜通过检查控制器绿色指示灯不亮。

10. 验证与主盘柜的 EBOD 存储模块连接是通过检查 SAS 航道指示灯 （四个每个 EBOD 控制器） 所有开好。

>[AZURE.IMPORTANT] 如果 SAS 电缆有缺陷或 EBOD 箱和主存储模块之间的连接时，不是什么好事，开启系统，它将进入恢复模式。 如果发生这种情况，请[与 Microsoft 支持部门联系](storsimple-contact-microsoft-support.md)。

## 请关闭正在运行的设备

正在运行的 Microsoft Azure StorSimple 设备可能需要关闭正在移动，停止服务，或有需要更换故障组件。 是根据 Microsoft Azure StorSimple 设备是否为 8100 或 8600 模型不同的步骤。 8100 具有单一的主盘柜，而 8600 是双盘柜设备与主盘柜和 EBOD 箱。 本节详细介绍的步骤关闭正在运行的设备。

- [与主盘柜设备](#8100a)

- [采用 EBOD 盒的设备](#8600a)

### 与主盘柜设备 <a name="8100a"> 

目前，并没有办法关闭正在运行的 StorSimple 设备从管理门户。 关闭它的唯一方法是通过 Windows PowerShell StorSimple。 要有序、 可控制的方式关闭该设备，访问 Windows PowerShell 的 StorSimple 并按照下面的步骤。

>[AZURE.IMPORTANT] 请不要关闭正在运行的设备通过设备背面的电源按钮。
>
>之前关闭设备，请确保所有设备组件都的正常。 在管理门户中，导航到**设备** > **维护** > **硬件状态**，并验证所有组件的状态为绿色。 这是只为状态良好的系统，则返回 true。 退出系统时向下更换故障组件，将看到失败 （红色） 或降级 （黄色） 的**硬件状态**中的各个组件的状态。

您可以连接到 Windows PowerShell 的 StorSimple 通过设备串行控制台或通过 Windows PowerShell 远程处理。 访问 Windows PowerShell 的 StorSimple 后，请执行以下步骤来关闭正在运行的设备。

#### 关闭正在运行的设备

1. 连接到设备的串行控制台。

2. 在出现的菜单上，请验证所连接到的控制器是**备用**控制器。 如果它不是备用控制器，从控制器，断开连接，连接到另一个控制器。

3. 在串行控制台菜单中，选择**选项 1** ，登录到备用控制器具有完全访问权限。

4. 在提示符下，键入︰ 

    `Stop-HCSController`

    这应该关闭当前的备用控制器。

    >[AZURE.IMPORTANT] 等待要前进到下一步之前完全关闭的控制器。

5. 要验证关闭已完成，请检查设备的背面。 控制器故障指示灯应呈稳定红色。

6. 连接到活动控制器通过串行控制台，请按照相同的步骤，关闭它。

7. 两个控制器具有完全关闭后，这两个控制器上的状态指示灯应闪烁红色。

8. 如果您需要这一次完全关闭该设备，则到 OFF 位置翻转电源和冷却模块 (PCMs) 上的电源开关。

9. 要验证设备已完全关闭，请检查该设备背面的所有灯都都熄灭。

### 采用 EBOD 盒的设备 <a name="8600a">

>[AZURE.IMPORTANT] 之前关闭主盘柜和 EBOD 存储模块时，确保所有设备组件运行状况良好。 在管理门户中，导航到**设备** > **维护** > **硬件状态**，并验证所有组件都是否正常运行。

#### 若要关闭具有 EBOD 存储模块的运行设备

1. 按照[与主盘柜设备](#8100a)中列出的所有步骤。

2. 关闭主盘柜后，关闭 EBOD 关闭电源和冷却模块 (PCM) 开关翻转。

3. 要验证已关闭 EBOD，检查 EBOD 机箱的后部上的所有指示灯都都熄灭。

>[AZURE.NOTE] 在系统关闭后，则不应直到卸下用于将 EBOD 存储模块连接到主盘柜的 SAS 电缆。

## 下一步行动

[与 Microsoft 客户支持](storsimple-contact-microsoft-support.md)如果您遇到问题时打开或关闭 StorSimple 设备。

