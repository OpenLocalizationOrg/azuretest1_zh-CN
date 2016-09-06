---
ms.openlocfilehash: 2a83cc62579e7242eb68ab0dfbff00b2ca9e82f4
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="电缆连接您的电源的 StorSimple 8600 |Microsoft Azure"
   description="解释如何将附加的电源线，然后打开 StorSimple 8600 设备第一次."
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

#### 电源设备的电缆连接

>[AZURE.NOTE] 这两个 StorSimple 设备上的存储模块包括冗余 PCMs。 对于每个存储模块，必须安装和连接到不同的电源以确保高可用性 PCMs。

1. 请确保所有 PCMs 上的电源开关处于关闭位置。

2. 上主盘柜，这两种 PCMs 连接电源线。 电源线中电源缆线连接图，用红色标识下方。

3. 请确保两个 PCMs 上主盘柜使用独立的电源。

4. 电源缆线连接图中所示，请将电源线连接到机架式配电设备上的电源。

5. 为 EBOD 的存储模块，请重复步骤 2 到 4。

6. 通过在每个 PCM 到开位置上切换电源开关打开 EBOD 箱。

7. 验证通过检查 EBOD 控制器的背面的绿色指示灯已亮开启 EBOD 箱。

8. 通过翻转到开位置的每个 PCM 开关打开主盘柜。

9. 验证系统在通过确保设备控制器指示灯已经打开了。

10. 请确保 EBOD 控制器和设备控制器之间的连接通过验证在 EBOD 控制器上的 SAS 端口旁边的四个指示灯呈绿色处于活动状态。

    >[AZURE.IMPORTANT] 若要确保您的系统的高可用性，我们建议您严格遵守电源电缆连接方案如下图所示。

    ![4U 设备的电源线](./media/storsimple-cable-8600-for-power/HCSCableYour4UDeviceforPower.png)

    **电源布线**

  	|对象中，从所有 TermSet 对象中返回不属于|说明|
  	|:----|:----------|
  	|1|主盘柜|
  	|2|PCM 0|
  	|3|PCM 1|
  	|4|控制器 0|
  	|5|控制器 1|
  	|6|EBOD 控制器 0|
  	|7|EBOD 控制器 1|
  	|8|EBOD 存储模块|
  	|9|配电设备|
