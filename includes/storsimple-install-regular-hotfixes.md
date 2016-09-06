---
ms.openlocfilehash: be90573f622e95abe650fdaf10a7bd71e7113828
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="安装常规的修复程序"
   description="说明如何使用 StorSimple 的 Windows PowerShell 安装常规的修复程序。"
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="adinah"
   editor="NA" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="07/28/2015"
   ms.author="alkohli" />

#### StorSimple 为安装 Windows PowerShell 通过常规修补程序

1. 连接到设备的串行控制台。 有关详细信息，请参阅[第 1 步︰ 连接到串行控制台](storsimple-update-device.md#step1)。

2. 在串行控制台菜单中，选择选项 1，**登录的完全访问权限**。 键入的密码。 默认密码是**密码 1**。

3. 在命令提示符下，键入︰

    `Start-HcsHotfix`

       >[AZURE.IMPORTANT]
       >
       >- 此命令仅适用于常规的修复程序。 只有一个控制器上运行此命令时，但这两种控制器将被更新。
       >- 您可能会发现控制器故障转移在更新过程中;但是，在故障转移不会影响系统的可用性或操作。

4. 出现提示时，提供包含修复程序文件的网络共享文件夹的路径。

5. 系统将提示您进行确认。 键入**Y**以继续安装修复程序。
