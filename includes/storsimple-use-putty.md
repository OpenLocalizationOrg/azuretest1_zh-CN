---
ms.openlocfilehash: 60fce5ef13cbb7f4fbf5680be2173bc41188afba
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="使用 PuTTY 连接到串行控制台设备"
   description="解释如何使用 PuTTY 终端仿真软件的 StorSimple 设备连接。"
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
   ms.date="04/01/2015"
   ms.author="v-sharos" />

#### 要通过串行控制台连接

1. 将串行电缆连接到设备 （直接或通过 USB 串行适配器）。

2. 打开**控制面板**，然后再打开**设备管理器**。

3. 标识的 COM 端口，如下图中所示。

     ![通过串行控制台连接](./media/storsimple-use-putty/HCS_ConnectingDeviceS-include.png)

4. 启动 PuTTY。 

5. 在右窗格中，将**连接类型**更改为**串行**。

6. 在右窗格中，键入适当的 COM 端口。 请确保串行配置参数设置如下︰
  - 速度︰ 115200
  - 数据位︰ 8
  - 停止位︰ 1
  - 奇偶校验︰ 无
  - 流控制︰ 无

    这些设置如下图所示。

     ![PuTTY 设置](./media/storsimple-use-putty/HCS_PuttyConfig-include.png) 

    > [AZURE.NOTE] 如果默认流控制设置不起作用，请尝试将流控制设置为 XON/xoff 协议。

7. 单击**打开**以启动串行会话。
 