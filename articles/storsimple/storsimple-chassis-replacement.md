---
ms.openlocfilehash: 8b8095428bed60a5a11179506bbe303b2eba9fcc
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="更换机箱上的 StorSimple 设备 |Microsoft Azure"
   description="描述如何删除和更换机箱 StorSimple 主要设备或 EBOD 存储模块上。"
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

# 更换机箱 StorSimple 设备上

## 概述

本教程说明如何删除和替换 StorSimple 设备中的机箱。 StorSimple 8100 模型是单个盘柜设备 （一个机箱），而 8600 是双盘柜设备 （两个机箱）。 对于 8600 模式，有可能两个机箱的设备中可能会失败︰ 主盘柜或为 EBOD 的存储模块机箱机箱。

在任一情况下，更换机箱附带由 Microsoft 将为空。 无电源和冷却模块 (PCMs) 控制器模块、 固态磁盘驱动器 (SSDs)、 硬盘驱动器 (Hdd) 或 EBOD 模块将包括。

>[AZURE.IMPORTANT] 在拆卸和更换机箱之前, 查看[StorSimple 组件更换硬件](storsimple-hardware-component-replacement.md)中的安全信息。

## 卸下机箱

执行以下步骤以删除 StorSimple 设备机箱。

#### 若要删除一个机箱

1. 请确保 StorSimple 设备已关闭并断开所有电源。

2. 如果适用，删除所有网络和 SAS 电缆。

3. 从机架中卸下该设备。

4. 删除每个驱动器，并注意删除它们从其插槽。 有关详细信息，请参阅[删除磁盘驱动器](storsimple-disk-drive-replacement.md#remove-the-disk-drive)。

5. EBOD 存储模块 （如果这是失败的机箱） 上删除 EBOD 控制器模块。 有关详细信息，请参阅[删除 EBOD 控制器](storsimple-ebod-controller-replacement.md#remove-an-ebod-controller)。 

    上主盘柜 （如果这是失败的机箱），删除控制器并注意从中删除它们的插槽。 有关详细信息，请参阅[删除控制器](storsimple-controller-replacement.md#remove-a-controller)。

## 安装机箱

执行以下步骤以在 Microsoft Azure StorSimple 设备中安装机箱。

#### 安装机箱

1. 安装在机架机箱。 有关详细信息，请参阅[StorSimple 8100 设备的机架](storsimple-8100-hardware-installation.md#rack-mount-your-storsimple-8100-device)[安装式 StorSimple 8600 设备](storsimple-8600-hardware-installation.md#rack-mount-your-storsimple-8600-device)。

2. 机箱安装在机架中后，控制器模块安装在以前安装在同一个位置。

3. 在相同的位置和它们原先安装在插槽中安装驱动器。

    >[AZURE.NOTE] 一般情况下，我们建议您首先，在槽中放置 SSDs，然后安装硬盘。

2. 设备安装到机架和安装的组件，将设备连接到合适的电源，并打开该设备。 有关详细信息，请参阅[电缆 StorSimple 8100 设备](storsimple-8100-hardware-installation.md#cable-your-storsimple-8100-device)或[电缆 StorSimple 8600 设备](storsimple-8600-hardware-installation.md#cable-your-storsimple-8600-device)。

## 下一步行动

了解有关[StorSimple 更换硬件组件](storsimple-hardware-component-replacement.md)的详细信息。

