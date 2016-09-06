---
ms.openlocfilehash: 9084f93367db3d6d345cb91006b6979f59b56ceb
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="装载、 初始化和格式化卷"
   description="解释如何配置 StorSimple 设备上的卷。"
   services="storsimple"
   documentationCenter="NA"
   authors="SharS"
   manager="adinah"
   edito**r="tysonn" />
<tags 
   ms.se**rvice="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="04/29/2015"
   ms.author="v-sharos" />

#### 装载、 初始化，并在格式化卷

1. 启动 Microsoft iSCSI 发起程序。

2. 在**iSCSI 发起程序属性**窗口上的**搜索**选项卡上，单击**发现门户**。

3. 在**发现目标入口**对话框中，提供您的 iSCSI 已启用网络接口的 IP 地址，然后单击**确定**。 

4. 在**iSCSI 发起程序属性**窗口中，在**目标**标签中，找到**Discovered 的目标**。 设备状态应显示为**非活动**。

5. 选择目标设备，然后单击**连接**。 连接设备后，状态变为**已连接**。 （有关使用 Microsoft iSCSI 发起程序的详细信息，请参阅[安装和配置 Microsoft iSCSI 启动器][1]）。

6. 在 Windows 主机上，按下 Windows 徽标键 + X，然后单击**运行**。 

7. 在**运行**对话框中，键入**Diskmgmt.msc**。 单击**确定**，和**磁盘管理**对话框将出现。 右窗格中将显示您主机上的卷。

8. 在**磁盘管理**窗口中已装入的卷将显示如下图中所示。 用鼠标右键单击发现的卷 （单击磁盘名称），然后单击**联机**。

     ![格式化卷的初始化](./media/storsimple-mount-initialize-format-volume/HCS_InitializeFormatVolume-include.png) 

9. 用鼠标右键单击该卷 （单击磁盘名称），然后单击**初始化**。

10. 要设置格式的简单卷，请执行以下步骤︰
  1. 选择该卷，请右键单击它 （单击权限区域中），然后单击**新建简单卷**。
  2. 在新建简单卷向导中，指定卷的大小和驱动器号，并配置为 NTFS 文件系统卷。
  3. 指定 64 KB 的分配单元大小。 此分配单元大小适用于 StorSimple 解决方案中使用的重复数据消除算法。
  4. 执行快速格式化。

<!--Link references-->
[1]: https://technet.microsoft.com/library/ee338480(WS.10).aspx
