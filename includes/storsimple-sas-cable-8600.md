---
ms.openlocfilehash: 3dc49ef1f98a60c5f9f67588de6c10c8263fbb8b
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="缆线为 8600 Storsimple SA |Microsoft Azure"
   description="解释了如何在 StorSimple 8600 设备连接 SAS 电缆。"
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

#### 若要将 SAS 电缆连接

1. 确定主和 EBOD 箱。 两个存储模块可以通过看一看他们各自的背平面标识。 请参阅下面的图像的指导。 

    ![备份的主平面和 EBOD 箱](./media/storsimple-sas-cable-8600/HCSBackplaneofprimaryandEBODenclosure.png)

    **后端主要和 EBOD 箱的视图**

  	|对象中，从所有 TermSet 对象中返回不属于|说明|
  	|:----|:----------|
  	|1|主盘柜|
  	|2|EBOD 存储模块|

2. 查找序列号主和 EBOD 箱。 序列号标签粘贴到每个存储模块的后耳。 序列号必须在这两种箱上相同。 [联系 Microsoft 技术支持](storsimple-contact-microsoft-support.md)立即如果序列号不匹配。 请参阅下图，找到序列号。

    ![显示序列号存储模块的后视图](./media/storsimple-sas-cable-8600/HCSRearviewofenclosureindicatinglocationofserialnumbersticker.png)

    **序列号标签的位置**

  	|对象中，从所有 TermSet 对象中返回不属于|说明|
  	|:----|:----------|
  	|1|Ear 存储模块|

3. 使用提供的 SAS 电缆将 EBOD 存储模块连接到主盘柜，如下所示︰

    1. 标识主盘柜和 EBOD 存储模块上的四个 SAS 端口。 SAS 端口都标记为 EBOD 的主盘柜和 CTRL EBOD 柜上，SAS 电缆图，下面所示。

    2. 使用提供的 SAS 电缆连接到 CTRL 端口的 EBOD 端口。

    3. 控制器 0 上的 EBOD 端口时应连接至端口 EBOD 控制器 0 上。 控制器的 EBOD 端口时应连接至端口 EBOD 控制器。 请参阅下图的指南。 
                                                                    
     ![SAS 电缆连接您的设备](./media/storsimple-sas-cable-8600/HCSSAScablingforyourdevice.png)

     **SAS 电缆连接**

  	|对象中，从所有 TermSet 对象中返回不属于|说明|
  	|:----|:----------|
  	|A|主盘柜|
  	|B|EBOD 存储模块|
  	|1|控制器 0|
  	|2|控制器 1|
  	|3|EBOD 控制器 0|
  	|4|EBOD 控制器 1|
  	|5、 6|SAS 端口 (标记为 EBOD) 的主盘柜|
  	|7、 8|SAS 端口 EBOD 盘柜 (标记为 CTRL)|
