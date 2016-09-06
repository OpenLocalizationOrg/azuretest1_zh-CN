---
ms.openlocfilehash: b76167c5e1e280f06d01870736a7c1ce9d6f074d
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="修改数据 0 StorSimple 设备上的设置 |Microsoft Azure"
   description="了解如何使用 Windows PowerShell 的 StorSimple StorSimple 设备上的数据 0 网络接口重新配置。"
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carolz"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/02/2015"
   ms.author="alkohli" />

# 修改您的 StorSimple 设备上的数据 0 网络接口设置

## 概述

您的 Microsoft Azure StorSimple 设备具有六个网络接口，数据 0 到数据 5。 0 接口始终通过 Windows PowerShell 接口或串行控制台，配置，并且是自动启用云的数据。 首先通过配置的接口数据 0 StorSimple 设备在初始部署过程中安装向导。 当设备处于运行模式时，您可能需要重新配置数据 0 设置。 本教程提供了两种方法来修改数据 0 网络设置，对于 StorSimple 这两种通过 Windows PowerShell。

先阅读本教程中，您将能够︰

- 修改数据 0 网络安装向导设置
- 修改数据 0 的网络设置，通过`Set-HcsNetInterface`cmdlet


## 修改数据 0 的网络设置，通过设置向导
通过对 StorSimple 设备的 Windows PowerShell 接口连接并启动安装向导会话，您可以重新配置数据 0 的网络设置。 执行下列步骤来修改数据 0 设置︰

#### 若要修改数据 0 网络安装向导设置

1. 在串行控制台菜单中，选择选项 1，**登录的完全访问权限**。 当系统提示您提供该**设备的管理员密码**。 默认密码为`Password1`。

2. 在命令提示符下，键入︰


    `Invoke-HcsSetupWizard`

3. 将出现安装向导以帮助您配置数据 0 接口的设备。 IP 地址、 网关和子网掩码提供新值。

> [AZURE.NOTE] IPs 的固定的控制器需要通过 Azure 管理门户中的 StorSimple 设备的**配置**页进行重新配置。 有关详细信息，请转到[修改网络接口](storsimple-modify-device-config.md#modify-network-interfaces)。


## 修改数据集 HcsNetInterface cmdlet 通过 0 网络设置
另一种方法来重新配置数据使用的网络接口是的 0 `Set-HcsNetInterface` cmdlet。 该 cmdlet 执行从 Windows PowerShell StorSimple 设备的接口。 执行下列步骤来修改数据 0 设置︰ 

#### 若要修改数据集 HcsNetInterface cmdlet 通过 0 网络设置

1. 在串行控制台菜单中，选择选项 1，**登录的完全访问权限**。 当系统提示您提供设备的管理员密码。 默认密码为`Password1`。

2. 在命令提示符下，键入︰

    `Set-HCSNetInterface -InterfaceAlias Data0 -IPv4Address <> -IPv4Netmask <> -IPv4Gateway <> -Controller0IPv4Address <> -Controller1IPv4Address <> -IsiScsiEnabled 1 -IsCloudEnabled 1`
    
    在尖括号中为 0 的数据中键入下列值︰
                                            
    - IPv4 地址
    
    - IPv4 网关
    
    - IPv4 的子网掩码
    
    - IPv4 地址固定的控制器 0

    - IPv4 地址固定的控制器 1

## 下一步行动

若要配置数据 0 以外的网络接口，可以使用[管理门户中的配置页](storsimple-modify-device-config.md)。 如果在配置网络接口时，您会遇到任何问题，，请参阅[解决部署问题](storsimple-troubleshoot-deployment.md)。

