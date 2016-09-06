---
ms.openlocfilehash: 2b36fa23c853282535dd17349f6f30e459c12c15
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="更新您的 StorSimple 设备 |Microsoft Azure"
   description="说明如何使用 StorSimple 更新功能来安装常规和维护模式更新和修补程序。"
   services="storsimple"
   documentationCenter="NA"
   authors="SharS"
   manager="adinah"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="08/19/2015"
   ms.author="v-sharos" />

# 更新您的 StorSimple 设备

## 概述

StorSimple 更新功能允许您方便地使您的 StorSimple 设备保持最新。 根据更新类型中，可以将更新应用到通过 Microsoft Azure 管理门户或通过 Windows PowerShell 接口的设备。 本教程中介绍的更新类型以及如何安装每个。

您可以应用两种类型的设备更新︰ 

- 常规 （或正常模式） 更新
- 维护模式更新

您可以安装在管理门户或 Windows PowerShell; 通过定期更新但是，您必须使用 Windows PowerShell 安装维护模式的更新。 

每个更新类型是单独设置，如下所述。

### 定期更新

定期更新是无中断设备处于正常模式时可以安装的更新。 这些更新将应用于每个设备控制器通过 Microsoft 更新网站。 

> [AZURE.IMPORTANT] 控制器故障转移可能会在更新过程中发生。 但是，这不会影响系统的可用性或操作。

- 有关如何安装管理门户通过定期更新的详细信息，请参阅[安装通过管理门户网站的定期更新](#install-regular-updates-via-the-management-portal)。

- 您还可以为 StorSimple 安装通过 Windows PowerShell 的定期更新。 有关详细信息，请参阅[安装 Windows PowerShell 的 StorSimple 通过定期更新](#install-regular-updates-via-windows-powershell-for-storsimple)。

### 维护模式更新

维护模式更新被中断的更新，如磁盘固件升级或 USM 的固件升级。 这些更新要求置于维护模式下的设备。 有关详细信息，请参阅[步骤 2︰ 输入维护模式](#step2)。 您不能使用管理门户安装维护模式更新。 相反，您必须使用 StorSimple Windows PowerShell。 

有关如何安装维护模式更新的详细信息，请参阅[通过 StorSimple 的 Windows PowerShell 的安装维护模式更新](#install-maintenance-mode-updates-via-windows-powershell-for-storsimple)。

> [AZURE.IMPORTANT] 维护模式更新必须分别应用于每个控制器。 

## 安装通过管理门户网站的定期更新

管理门户可以用于将更新应用到 StorSimple 设备。

[AZURE.INCLUDE [storsimple-install-updates-manually](../../includes/storsimple-install-updates-manually.md)]

## StorSimple 为安装 Windows PowerShell 通过定期更新

或者，可以使用 Windows PowerShell 的 StorSimple 应用常规 （正常模式） 更新。

> [AZURE.IMPORTANT] 虽然您可以安装 StorSimple 使用 Windows PowerShell 的定期更新，我们强烈建议您安装通过管理门户网站的定期更新。 从开始更新 1，将从该门户安装更新之前执行前检查。 这些预检查将抢占故障，并确保更流畅。 

[AZURE.INCLUDE [storsimple-install-regular-updates-powershell](../../includes/storsimple-install-regular-updates-powershell.md)]

## StorSimple 为安装 Windows PowerShell 通过维护模式更新

使用 Windows PowerShell 的 StorSimple 到 StorSimple 设备应用维护模式更新。 在此模式下，所有的 I/O 请求已暂停。 如非易失性随机访问存储器 (NVRAM) 服务或群集服务也会被停止。 这两种控制器重新启动时进入或退出此模式。 退出此模式时，所有服务将继续，并且应该运行状况良好。 （这可能需要几分钟）。

如果您需要应用维护模式更新，您将收到通过管理门户通知您有必须安装的更新。 此警报会包括使用 Windows PowerShell 的 StorSimple 安装更新的说明。 更新您的设备后，可以使用相同的过程设备改为常规模式。 有关分步说明，请参阅[第 4 步︰ 退出维护模式](#step4)。

> [AZURE.IMPORTANT] 
> 
> - 在进入维护模式之前, 验证两个设备控制器通过检查**硬件状态**管理门户中的**维护**页上的健康。 如果控制器不是健康的对下一步行动联系 Microsoft 技术支持。 有关详细信息，请转到与 Microsoft 客户支持联系。 
> - 当您是在维护模式下时，您需要一个控制器，然后在其他控制器首先应用此更新。

### 步骤 1︰ 连接到串行控制台 <a name="step1">

首先，使用 PuTTY 等应用程序来访问串行控制台。 下面的过程解释如何使用 PuTTY 连接到串行控制台。

[AZURE.INCLUDE [storsimple-use-putty](../../includes/storsimple-use-putty.md)]

### 第 2 步︰ 进入维护模式 <a name="step2">

连接到控制台之后，确定是否存在要安装，并进入维护模式安装这些更新。

[AZURE.INCLUDE [storsimple-enter-maintenance-mode](../../includes/storsimple-enter-maintenance-mode.md)]

### 步骤 3︰ 安装更新 <a name="step3">

接下来，安装更新。

[AZURE.INCLUDE [storsimple-install-maintenance-mode-updates](../../includes/storsimple-install-maintenance-mode-updates.md)]
 
### 步骤 4︰ 退出维护模式 <a name="step4">

最后，退出维护模式。

[AZURE.INCLUDE [storsimple-exit-maintenance-mode](../../includes/storsimple-exit-maintenance-mode.md)]

## 安装修复程序通过 Windows PowerShell StorSimple

与 Microsoft Azure StorSimple 的更新，请从共享文件夹安装修补程序。 与更新，有两种类型的修补程序︰ 

- 常规的修复程序 
- 维护模式修复程序  

下面的过程说明如何使用 StorSimple 的 Windows PowerShell 安装常规和维护模式修复程序。

[AZURE.INCLUDE [storsimple-install-regular-hotfixes](../../includes/storsimple-install-regular-hotfixes.md)]

[AZURE.INCLUDE [storsimple-install-maintenance-mode-hotfixes](../../includes/storsimple-install-maintenance-mode-hotfixes.md)]

## 什么情况更新执行出厂重置的设备？

如果设备被重置为出厂设置，则所有更新都都将丢失。 出厂重置设备注册和配置后，您需要手动安装 StorSimple 通过管理门户和/或 Windows PowerShell 的更新。 有关工厂的详细信息将重置，请参阅[重置为出厂默认设置的设备](storsimple-manage-device-controller.md#reset-the-device-to-factory-default-settings)。

## 下一步行动

[了解如何使用 StorSimple 来管理您的 StorSimple 设备的 Windows PowerShell](storsimple-windows-powershell-administration.md)。
 