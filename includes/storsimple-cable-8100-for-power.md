---
ms.openlocfilehash: a741daab62267bbb87177174dabca93a73fdea36
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="电缆连接您的电源的 StorSimple 8100 |Microsoft Azure"
   description="解释如何将附加的电源线，然后打开 StorSimple 8100 设备第一次。"
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
   ms.date="08/06/2015"
   ms.author="alkohli" />

#### 电源线

1. 请确保每个电源开关电源和冷却模块 (PCMs) 处于关闭位置。

2. 将电源线连接到 PCMs 主盘柜中的每个。

3. 将电源线附加到机架的电源配电单元 (Pdu) 中，如下图中所示。 请确保两个 PCMs 使用单独的电源。

    >[AZURE.IMPORTANT] 若要确保您的系统的高可用性，我们建议您严格遵守电源电缆连接方案如下图所示。 

    ![2U 设备的电源线](./media/storsimple-cable-8100-for-power/HCSCableYour2UDeviceforPower.png)

    **缆线连接到 8100 设备上的电源**

  	|对象中，从所有 TermSet 对象中返回不属于|说明|
  	|:----|:----------|
  	|1|PCM 0|
  	|2|控制器 1|
  	|3|控制器 0|
  	|4|PCM 1|
  	|5|配电设备|

4. 要打开系统，翻转在两个位置对 PCMs 上的电源开关。
