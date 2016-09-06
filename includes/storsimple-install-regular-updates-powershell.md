---
ms.openlocfilehash: cbc886d839d8a9936147fe62375452f19922b761
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="StorSimple 为安装 Windows PowerShell 通过定期更新"
   description="说明如何使用 StorSimple 更新功能和 Windows PowerShell 的 StorSimple 安装的定期更新。"
   services="storsimple"
   documentationCenter="NA"
   authors="SharS"
   manager="adinah"
   editor="tysonn" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="04/27/2015"
   ms.author="v-sharos" />

#### StorSimple 为安装 Windows PowerShell 通过定期更新

1. 打开设备串行控制台并选择选项 1，**登录的完全访问权限**。 键入的密码。 默认密码是*密码 1*。 

2. 在命令提示符下，键入︰

     `Get-HcsUpdateAvailability`
    
    如果有可用的更新和更新是否中断或无中断，您将收到通知。

3. 在命令提示符下，键入︰

     `Start-HcsUpdate`

    更新过程将开始。

> [AZURE.IMPORTANT]
>
> - 此命令仅适用于定期更新。 只有一个控制器上运行此命令时，但这两种控制器将被更新。 
> - 您可能会发现控制器故障转移在更新过程中;但是，在故障转移不会影响系统的可用性或操作。
