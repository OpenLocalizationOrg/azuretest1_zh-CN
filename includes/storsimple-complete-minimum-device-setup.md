---
ms.openlocfilehash: 9379cfca7b609149766d74402d91d4944f896a33
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="完成最基本的设备安装程序"
   description="介绍了如何完成所需最低 StorSimple 设备配置。"
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
   ms.date="08/07/2015"
   ms.author="v-sharos" />

#### 若要完成最小的 StorSimple 设备安装

1. 在**设备**页中，选择该设备，请单击要转到特定设备页的设备名称的箭头。 

    ![与设备联机设备页面](./media/storsimple-complete-minimum-device-setup/HCS_DevicesPageM-include.png) 

2. 单击快速启动图标![快速启动图标](./media/storsimple-complete-minimum-device-setup/HCS_QuickStartIcon-include.png)访问设备快速起始页。 单击以启动**配置设备**向导**完成设备安装**。

    ![设备快速起始页](./media/storsimple-complete-minimum-device-setup/Device_Quick_Start_page_1M.png)

2. 在**基本设置**页上，执行下列操作︰
  1. 提供一个**好记的名称**为您的设备。 设备的默认名称反映了设备型号和序列号等信息。 您可以分配最多 64 个字符来管理您的设备的友好名称。
  2. 设置**时区**在其中部署设备的地理位置。 您的设备将用于所有的预定操作该时区。
  3. 在**DNS 设置**，提供**辅助 DNS 服务器**的地址。 如果您使用的 IPv6，将基于 Windows PowerShell 界面中提供的 IPv6 前缀填充该字段。 
  如果没有配置辅助 DNS 服务器，您将不能保存您的设备配置。
  4. 在启用 iSCSI 接口启用至少一个用于 iSCSI 的网络。 至少一个网络接口必须支持云的和一个接口需要被支持 iSCSI 的。 0 数据是自动启用云了。
 
      ![StorSimple 最小设备安装基本设置](./media/storsimple-complete-minimum-device-setup/HCS_MinDeviceSetupBasicSettings1-include.png)

3. 单击箭头图标。 ![StorSimple 箭头图标](./media/storsimple-complete-minimum-device-setup/HCS_ArrowIcon-include.png)

4. 在**网络接口**页中，提供了控制器 0 和控制器 1 固定的 IP 地址。 如果接口为 IPv4 配置数据 0，固定的 IP 地址需要提供以 IPv4 格式。 如果您提供一个前缀的 IPv6 配置，固定的 IP 地址将自动填充这些字段中。


    > [AZURE.NOTE] 
    > 
    > - 固定的 IP 地址的控制器需要访问该设备的 IP 地址的子网内免费 IPs。
    > - 控制器固定的 IP 地址用于维修设备的更新，因此固定的 Ip 必须是可路由，并且能够连接到互联网。

    ![StorSimple 最小设备安装网络接口](./media/storsimple-complete-minimum-device-setup/HCS_MinDeviceSetupNetworkInterfaces2-include.png)

5. 单击检查图标![StorSimple 检查图标](./media/storsimple-complete-minimum-device-setup/HCS_CheckIcon-include.png)。
  您将返回到设备**快速启动**页。

 > [AZURE.NOTE] 通过访问**配置**页，可以在任何时候修改所有其他设备设置。
