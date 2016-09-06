---
ms.openlocfilehash: 21b56aef4f3bbd52ebf7a2410f200717cdbe015b
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="更改您的密码 StorSimple |Microsoft Azure" 
   description="介绍如何使用 StorSimple 管理器服务更改您 StorSimple 快照管理器和设备的管理员密码。" 
   services="storsimple" 
   documentationCenter="NA" 
   authors="SharS" 
   manager="carolz" 
   editor=""/>

<tags
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD" 
   ms.date="08/31/2015"
   ms.author="v-sharos@microsoft.com"/>

# 使用 StorSimple 管理器服务更改 StorSimple 密码

## 概述 

管理门户**配置**页中包含您可以重新配置由 StorSimple 管理器服务的 StorSimple 设备的所有设备参数。 本教程介绍如何使用**配置**页面更改您 StorSimple 快照管理器或设备的管理员密码。

## 更改 StorSimple 快照管理器密码

StorSimple 快照管理器软件驻留在 Windows 主机上，并使管理员能够管理的 StorSimple 设备上的本地窗体中的备份和快照的云。

StorSimple 快照管理器中配置设备，系统将提示您提供的设备的 IP 地址和密码来验证您的存储设备。 通过 Windows PowerShell 界面首次配置该密码。 有关详细信息，请参阅[第 3 步︰ 配置和注册设备通过 Windows PowerShell StorSimple](storsimple-deployment-walkthrough.md#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple) [部署内部 StorSimple 设备](storsimple-deployment-walkthrough.md)中。

在注册期间第一次通过 Windows PowerShell 界面设置的密码就可以更改通过管理门户。 执行以下步骤来更改 StorSimple 快照管理器密码。

#### 若要更改 StorSimple 快照管理器密码

1. 在门户中，单击**设备** > 为您的设备**配置**。

2. 向下滚动到**StorSimple 快照管理器**部分。 输入是 14 或 15 个字符的密码。 请确保密码包含大写字母、 小写字母、 数字和特殊字符的组合。

3. 确认该密码。

4. 单击页面底部的**保存**。

现在应更新 StorSimple 快照管理器密码。
 
## 更改设备的管理员密码

当您使用 Windows PowerShell 接口来访问 StorSimple 设备时，您需要输入设备管理员密码。 第一个 StorSimple 设备注册了一个服务，该接口的默认密码时，*密码 1*。 为了数据的安全，您需要在注册过程结束时更改该密码。 才可以退出注册过程而无需更改此密码。 有关详细信息，请参阅[第 3 步︰ 配置和注册设备通过 Windows PowerShell StorSimple](storsimple-deployment-walkthrough.md#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple) [部署内部 StorSimple 设备](storsimple-deployment-walkthrough.md)中。

在注册期间第一次通过 Windows PowerShell 界面设置的密码就可以更改通过管理门户。 执行以下步骤来更改设备管理员密码。

#### 要更改设备管理员密码

1. 在门户中，单击**设备** > 为您的设备**配置**。

2. 向下滚动到**设备管理员密码**部分。 提供管理员密码，其中包含从 8 到 15 个字符。 密码必须是大写字母、 小写字母、 数字和特殊字符的组合。

3. 确认该密码。

4. 单击页面底部的**保存**。

现在应更新设备管理员密码。 此修改的密码用于访问 Windows PowerShell 界面。

## 下一步行动

[了解更多关于 StorSimple 安全](storsimple-security.md)。

[了解更多关于修改设备的配置](storsimple-modify-device-config.md)。