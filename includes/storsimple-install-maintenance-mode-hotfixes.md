---
ms.openlocfilehash: b7ab485ab77e42c77050b69654c543084aa930b6
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="安装维护模式修复程序"
   description="介绍如何使用 Windows PowerShell 的 StorSimple 安装维护模式修复程序。"
   services="storsimple"
   documentationCenter="NA"
   authors="SharS"
   manager="carolz"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="08/19/2015"
   ms.author="v-sharos" />

#### 为 StorSimple 安装维护模式修复程序通过 Windows PowerShell

> [AZURE.IMPORTANT] 在维护模式下，您需要一个控制器，然后在另一个控制器上首次应用此修复程序。

1. 将设备置于维护模式。 请参阅[步骤 2︰ 输入维护模式](storsimple-update-device.md#step2)有关说明如何进入维护模式。

2. 要应用此修补程序，请键入︰

     `Start-HcsHotfix` 

3. 出现提示时，提供包含修复程序文件的网络共享文件夹的路径。

4. 系统将提示您进行确认。 键入**Y**以继续安装修复程序。

5. 在一个控制器上应用此修复程序后，登录到另一个控制器上。 应用此修复程序，就像以前的控制器。

6. 在应用修补程序之后，请退出维护模式。 请参阅[第 4 步︰ 退出维护模式](storsimple-update-device.md#step4)有关的说明。
