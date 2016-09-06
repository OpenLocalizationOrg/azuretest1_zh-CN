---
ms.openlocfilehash: cd1d35659aafbc1562249ccb4fb5f6ce4fed87e4
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="退出维护模式"
   description="解释如何退出 StorSimple 维护模式，将设备返回到正常模式。"
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

#### 退出维护模式

1. 在命令提示符下键入︰

     `Exit-HcsMaintenanceMode`

2. 将显示一条警告消息和一条确认消息。 键入**Y**以退出维护模式。

    两个控制器会重新启动。 重新启动完成后，另一条消息会显示表明该设备处于普通模式。