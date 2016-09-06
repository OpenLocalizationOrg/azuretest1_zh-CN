---
ms.openlocfilehash: 0fab4488f85bb501a0df0aa6ee0e0745ca12c7cb
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="在 StorSimple 设备上安装更新 1 |Microsoft Azure"
   description="说明如何在您的设备上安装 StorSimple 8000 系列更新 1。"
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="adinah"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="08/31/2015"
   ms.author="alkohli" />

# 在 StorSimple 设备上安装更新 1

## 概述

本教程介绍如何运行更新 1 之前的软件版本的 StorSimple 设备上安装更新 1。 无法运行您的设备，通常可用 (GA) 发布、 更新 0.1、 0.2，更新或更新 0.3 软件。  

在此安装时，如果您的设备在运行之前更新 1 版本然后执行检查您的设备上。 这些检查确定硬件状态和网络连接方面的设备运行状况。

系统将提示您执行手动预检查以确保︰

- 固定的 Ip 控制器可路由，并且可以连接到互联网。 这些 IPs 用于处理对 StorSimple 设备的更新。 您可以在每个控制器上运行以下 cmdlet 进行测试︰

    `Test-Connection -Source <Fixed IP of your device controller> -Destination <Any IP or computer name outside of datacenter network> `
 
    **测试连接固定的 Ip 可以连接到 Internet 时的输出示例**

        
        Controller0>Test-Connection -Source 10.126.173.91 -Destination bing.com
        
        Source    Destination   IPV4Address      IPV6Address
        ----------------- -----------  -----------
        HCSNODE0  bing.com      204.79.197.200
        HCSNODE0  bing.com      204.79.197.200
        HCSNODE0  bing.com      204.79.197.200
        HCSNODE0  bing.com      204.79.197.200
    
        Controller0>Test-Connection -Source 10.126.173.91 -Destination  204.79.197.200

        Source    Destination     IPV4Address    IPV6Address
        ----------------- -----------  -----------
        HCSNODE0  204.79.197.200  204.79.197.200
        HCSNODE0  204.79.197.200  204.79.197.200
        HCSNODE0  204.79.197.200  204.79.197.200
        HCSNODE0  204.79.197.200  204.79.197.200
        
        


- 之前更新设备，我们建议您采取设备数据的云快照。 

已验证并确认 （上图） 的手动检查之后，将执行一组自动更新前检查。 其中包括︰

- **控制器的运行状况检查**以验证设备控制器正常运行并处于联机状态。

- **硬件组件运行状况检查**，以验证 StorSimple 设备上的所有硬件组件都正常运行。

- **检查数据 0** ，以验证您的设备上启用了数据 0。 如果未启用此接口，则您需要启用它，然后重试。

- **数据 2 和 3 的数据检查**，以验证未启用数据 2 和 3 的数据网络接口。 如果已经启用了这些接口，然后您需要禁用它们，然后再尝试更新您的设备。 要从设备运行正式提供软件更新时，才执行此检查。 运行版本 0.1、 0.2 或 0.3 的设备不需要这种检查。

- 在运行之前更新 1 版本的任何设备上的**网关检查**。 仅在已配置的网络接口数据 0 以外的网关的设备上执行此检查。
 
如果所有更新前检查成功完成都后，将仅应用更新 1。 StorSimple 设备上应用更新 1 之后，未来的更新将不具有数据 2 和 3 的数据检查和网关检查。 其他预检查将会发生。

## 使用管理门户安装更新 1

我们建议使用 Azure 管理门户来更新正在运行的正式发行版本的设备。 执行以下步骤来更新您的设备。

[AZURE.INCLUDE [storsimple-install-update-via-portal](../../includes/storsimple-install-update-via-portal.md)]


## 故障排除更新失败

**如果看到升级前检查已失败的通知了吗？**

如果预先检查失败，请确保已看好详细的通知栏在页面底部。 这提供了哪些预检查已失败的指导。 下图显示了这样的通知会显示一个实例。 在这种情况下，出现故障的控制器运行状况检查和硬件组件运行状况检查。 在**硬件状态**部分中，您可以看到控制器 0 和控制器 1 组件，需要引起注意。 
 
  ![前检查失败](./media/storsimple-install-update-1/HCS_PreUpdateCheckFailed-include.png)

您需要确保两个控制器正常运行并处于联机状态。 您还需要确保 StorSimple 设备中的所有硬件组件都显示为正常维护页上。 然后，您可以尝试安装更新。 如果不能修复硬件组件问题，您将需要与 Microsoft 支持部门联系为下一步行动。

**如果您收到"无法安装更新"错误消息，并提出建议，请参阅更新故障排除指南，以确定失败的原因是？**

对于这一可能的原因可能是您没有连接到 Microsoft 更新服务器。 这是需要进行手动检查。 如果您失去与更新服务器的连接，则更新作业就会失败。 通过从 Windows PowerShell 接口 StorSimple 设备的运行以下 cmdlet，您可以检查连接性︰

 `Test-Connection -Source <Fixed IP of your device controller> -Destination <Any IP or computer name outside of datacenter>`

这两种控制器上运行该 cmdlet。
 
如果您已验证连接性存在，并且您继续发生此问题，请联系 Microsoft 技术支持下一步行动。

## 下一步行动

了解有关[Microsoft Azure StorSimple](storsimple-overview.md) 
