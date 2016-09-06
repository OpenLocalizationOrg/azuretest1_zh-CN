---
ms.openlocfilehash: ce96b83a65db14d077b128a96568af6aef13875a
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="进入维护模式"
   description="说明如何将 StorSimple 设备放入维护模式。"
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
   ms.date="04/21/2015"
   ms.author="v-sharos" />

#### 进入维护模式

1. 在串行控制台菜单中，选择选项 1，**登录的完全访问权限**。

2. 键入的密码。 默认密码是**密码 1**。

3. 在命令提示符下，键入

     `Enter-HcsMaintenanceMode`

4. 您将看到一条警告消息，告诉您维护模式将会破坏所有的 I/O 请求和服务器连接到管理门户，并将提示您进行确认。 键入**Y**以进入维护模式。

    两个控制器会重新启动。 重新启动完成后，另一条消息将显示表明该设备处于维护模式。
