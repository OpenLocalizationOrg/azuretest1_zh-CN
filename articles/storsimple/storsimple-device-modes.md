---
ms.openlocfilehash: 075d64d045d4251166ea1a047863c91e0c2752bc
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="StorSimple 设备模式更改 |Microsoft Azure"
   description="介绍了 StorSimple 设备模式并解释了如何使用 Windows PowerShell 的 StorSimple 设备模式更改。"
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carolz"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/01/2015"
   ms.author="alkohli" />

# 更改您的 StorSimple 设备的设备模式

本文提供了 StorSimple 设备可以进行操作的各种模式的简要说明。 StorSimple 设备可以正常三种模式︰ 普通、 维护和恢复。 

阅读这篇文章之后，您将了解︰

- StorSimple 设备模式
- 如何确定使用哪种模式 StorSimple 设备正在
- 如何更改从正常到维护模式，*反之亦然*


只能通过 StorSimple 设备的 Windows PowerShell 接口执行上面的管理任务。

## 有关 StorSimple 设备模式

StorSimple 设备可以在普通、 维护或恢复模式中运行。 下面简要描述每一种模式。

### 普通模式

该速度定义为完全配置的 StorSimple 设备的正常操作模式。 默认情况下，您的设备应在正常模式下。

### 维护模式

有时可能需要放入维护模式的 StorSimple 设备。 这种模式允许您在设备上执行维护和安装中断更新，如与磁盘固件。

您可以将系统置于维护模式只能通过 Windows PowerShell 为 StorSimple。 在此模式下，所有的 I/O 请求已暂停。 如非易失性随机访问存储器 (NVRAM) 服务或群集服务也会被停止。 两个控制器重新启动时进入或退出此模式。 退出维护模式时，所有服务将继续，并且应该运行状况良好。 这可能需要几分钟的时间。

>[AZURE.NOTE] **在正常工作的设备只支持维护模式。 在其中一个或两个控制器无法正常工作的设备上不支持它。**
</br>

### 恢复模式

恢复模式可以描述为"与网络支持 Windows 安全模式"。 恢复模式下闭合 Microsoft 技术支持团队，并允许他们在系统上执行诊断程序。 恢复模式的主要目标是检索系统日志。

如果您的系统将进入恢复模式，您应该对下一步行动联系 Microsoft 技术支持。 有关详细信息，请转到[与 Microsoft 客户支持联系](storsimple-contact-microsoft-support.md)。

>[AZURE.NOTE] **不能将设备放置在恢复模式下。 如果该设备是处于错误的状态，恢复模式尝试进入的 Microsoft 技术支持人员可以检查其状态的设备。**

## 确定 StorSimple 设备模式

#### 若要确定当前的设备模式

1. 按照[使用 PuTTY 连接到设备串行控制台](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console)登录到设备的串行控制台。
2. 查看设备的串行控制台菜单中标题消息。 此消息明确表示设备是否处于维护或恢复模式。 如果邮件不包含任何属于系统模式的特定信息，该设备将处于普通模式。

## StorSimple 设备模式更改 

您可以将 StorSimple 设备进入维护模式 （从普通模式） 执行维护或安装维护模式更新。 执行以下过程以进入或退出维护模式。

> [AZURE.IMPORTANT] 之前进入维护模式，验证这两个设备控制器通过访问管理门户中的**维护**页上的**硬件状态**都正常。 如果控制器不是健康的对下一步行动联系 Microsoft 技术支持。 有关详细信息，请转到[与 Microsoft 客户支持联系](storsimple-contact-microsoft-support.md)。

#### 进入维护模式

1. 按照[使用 PuTTY 连接到设备串行控制台](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console)登录到设备的串行控制台。

2. 在串行控制台菜单中，选择选项 1，**登录的完全访问权限**。 出现提示时，提供该**设备的管理员密码**。 默认密码︰ `Password1`。

3. 在命令提示符下，键入 

    `Enter-HcsMaintenanceMode`

4. 您将看到一条警告消息，告诉您维护模式将会破坏所有的 I/O 请求和服务器连接到管理门户，并将提示您进行确认。 键入**Y**以进入维护模式。

5. 两个控制器会重新启动。 重新启动完成后，另一条消息将显示表明该设备处于维护模式。


#### 退出维护模式

1. 登录到设备的串行控制台。 请从您的设备是在维护模式下标题消息。

2. 在命令提示符下，键入︰

    `Exit-HcsMaintenanceMode`

3. 将显示一条警告消息和一条确认消息。 键入**Y**以退出维护模式。

4. 两个控制器会重新启动。 重新启动完成后，另一条消息会显示表明该设备处于普通模式。


## 下一步行动

了解如何在 StorSimple 设备上[应用普通和维护模式更新](storsimple-update-device.md)。

