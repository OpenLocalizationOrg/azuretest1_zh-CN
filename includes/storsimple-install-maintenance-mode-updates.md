---
ms.openlocfilehash: 7384adffe83f6886ff38d7dc5684aefc5911203b
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="安装维护模式更新"
   description="说明如何使用 StorSimple 的 Windows PowerShell 安装维护模式更新。"
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carolz"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="08/18/2015"
   ms.author="alkohli" />

#### 安装维护模式更新通过 Windows PowerShell 的 StorSimple

1. 如果您没有这样做，访问设备串行控制台并选择选项 1，**登录的完全访问权限**。 

2. 键入的密码。 默认密码是**密码 1**。

3. 在命令提示符下，键入︰

     `Get-HcsUpdateAvailability` 
    
4. 如果有可用的更新和更新是否中断或无中断，您将收到通知。 若要应用中断更新，您需要将设备放入维护模式。 请参阅[步骤 2︰ 输入维护模式](storsimple-update-device.md#step2)有关的说明。

5. 您的设备是在维护模式下，在命令提示符下，键入︰ `Start-HcsUpdate`

6. 系统将提示您进行确认。 确认更新后，将在您正在访问的控制器上安装它们。 已安装这些更新后，控制器会重新启动。 

7. 监视更新的状态。 类型：

    `Get-HcsUpdateStatus`
    
    如果`RunInProgress`是`True`，更新程序仍在进行。 如果`RunInProgress`是`False`，则表明该更新已完成。  

7. 当当前控制器上安装了此更新并重新启动它时，连接到其他控制器，请执行步骤 1 到 6。

8. 这两种控制器进行更新后，退出维护模式。 请参阅[第 4 步︰ 退出维护模式](storsimple-update-device.md#step4)有关的说明。
