---
ms.openlocfilehash: 5ceece56ab687320045a5491edfa009cce11520e
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="StorSimple 设备管理的 PowerShell |Microsoft Azure"
   description="了解如何使用 StorSimple 的 Windows PowerShell 来管理您的 StorSimple 设备。"
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carolz"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="08/28/2015"
   ms.author="alkohli@microsoft.com" />

# 使用 StorSimple 的 Windows PowerShell 来管理您的设备

## 概述

StorSimple 的 Windows PowerShell 提供了命令行界面，可用于管理您的 Microsoft Azure StorSimple 设备。 顾名思义，它是受约束的运行空间在构建一个基于 Windows PowerShell 的命令行界面。 在命令行的用户的角度来看，从有限的运行空间显示为 Windows PowerShell 的有限版本。 同时一些 Windows PowerShell 的基本功能，此接口具有更多面向管理 Microsoft Azure StorSimple 设备的专用的 cmdlet。 

本文介绍 Windows PowerShell StorSimple 功能，其中包括如何您可以连接到此接口，并包含指向分步过程或工作流，您可以使用此接口。 工作流包括如何注册您的设备在您的设备上配置网络接口、 安装更新程序要求的设备在维护模式下，更改设备状态中，并解决可能会遇到的任何问题。

阅读这篇文章之后，您将能够︰

- 连接到使用 Windows PowerShell StorSimple StorSimple 设备。

- 管理 Windows PowerShell 用于 StorSimple StorSimple 设备。

- StorSimple 在 Windows PowerShell 获取帮助。

>[AZURE.NOTE]   

>- StorSimple 的 cmdlet 的 Windows PowerShell 允许您从控制台或远程通过 Windows PowerShell 远程管理您的 StorSimple 设备。 有关每个可以在此接口中使用的各个 cmdlet 的详细信息，请转到[StorSimple 的 Windows PowerShell 的 cmdlet 参考](https://technet.microsoft.com/library/dn688168.aspx)。

>- Azure PowerShell StorSimple cmdlet 都允许您自动完成 StorSimple 的服务级别，并从命令行迁移任务的 cmdlet 的不同集合。 针对 StorSimple Azure PowerShell cmdlet 的详细信息，请转到[Azure StorSimple cmdlet 参考](https://msdn.microsoft.com/library/azure/dn920427.aspx)。

您可以访问 Windows PowerShell 的 StorSimple 使用下列方法之一︰

- [连接到 StorSimple 设备串行控制台](#connect-to-windows-powershell-for-storsimple-via-the-device-serial-console)
- [远程连接到使用 Windows PowerShell 的 StorSimple](#connect-remotely-to-storsimple-using-windows-powershell-for-storsimple)
    

## 连接到 Windows PowerShell 的 StorSimple，通过设备串行控制台

您可以[下载 PuTTY](http://www.putty.org/)或类似的终端仿真软件的 StorSimple 连接到 Windows PowerShell。 您需要配置 PuTTY 专门为访问 Microsoft Azure StorSimple 设备。 以下主题包含有关如何配置 PuTTy 并连接到该设备的详细的步骤。 此外阐释了不同的菜单选项中串行控制台。

### 有关串行控制台

当您访问显示接口的 StorSimple 设备通过串行控制台，标题消息 Windows PowerShell 时，跟菜单选项。 

标题消息包含基本的 StorSimple 设备信息，如模型、 名称、 已安装的软件版本和您正在访问的控制器的状态。 下图显示标题消息的一个示例。

![串行标题消息](./media/storsimple-windows-powershell-administration/IC741098.png)



>[AZURE.IMPORTANT] 标题消息可用于标识所连接到的控制器是否主动或被动。


下图显示了串行控制台菜单中是可用的各种运行空间选项。

![注册您的设备 2](./media/storsimple-windows-powershell-administration/IC740906.png)

您可以从以下设置中进行选择︰

1. **登录的完全访问权限**此选项允许您连接 （使用适当的凭据） 在本地控制器上的**SSAdminConsole**运行空间。 （本地控制器是您正在访问的通过 StorSimple 设备的串行控制台的控制器）。此外可以使用此选项允许 Microsoft 支持访问不受限制地运行空间 （支持会话） 以排除任何可能的设备故障。 使用选项 1 登录后，您可以允许 Microsoft 技术支持工程师来运行特定的 cmdlet 访问不受限制的运行空间。 有关详细信息，请参阅[启动支持会话](storsimple-contact-microsoft-support.md#start-a-support-session-in-windows-powershell-for-storsimple)。 此选项连接到本地控制器上的运行空间。

1. **登录到对等控制器具有完全访问权限**此选项是选项 1，相同之处在于您可以连接到对等的控制器上的**SSAdminConsole**运行空间 （使用适当的凭据）。 StorSimple 设备是高可用性设备具有主动-被动配置中的两个控制器，因为对等是指您通过串行控制台访问该设备中的另一个控制器）。
类似于选项 1，该选项还可允许 Microsoft 支持访问不受限制的等控制器上的运行空间。

1. **具有有限访问权限的连接**此选项用于访问 Windows PowerShell 有限模式中的接口。 不提示您提供访问凭据。 此选项与选项 1 和 2 的更多的限制运行空间连接。  下面是一些可通过选项 1 不能在此运行空间中执行的任务︰

    - 重置为出厂设置
    - 更改密码
    - 启用或禁用支持访问权限
    - 应用更新
    - 安装修补程序 
                                                

    >[AZURE.NOTE] **如果您忘记了的设备管理员密码并通过 1 或 2 的选项不能连接，这是首的选项。**

1. **更改语言**此选项允许您更改在 Windows PowerShell 界面的显示语言。 支持的语言有英语、 日语、 俄语、 法语、 南部朝鲜语、 西班牙语、 意大利语、 德语、 中文和葡萄牙语 （巴西）。

请确保使用以下的 PuTTY 设置可通过串行控制台连接到 Windows PowerShell 界面。

#### 若要配置 PuTTY

1. 在 PuTTY 重新配置对话框的**类别**窗格中，选择**键盘**。

1. 请确保选择了以下选项 （当您启动一个新会话时，这些是默认设置）。 

  	|键盘项目|选择|
  	|---|---|
  	|退格键|控制的吗？ () 127|
  	|开始和结束键|标准|
  	|功能键和数字键盘|Esc 键 [n ~|
  	|光标键的初始状态|常规|
  	|初始状态的数字小键盘|常规|
  	|启用额外的键盘功能|控件 Alt 是不同于 AltGr|

    ![受支持的 Putty 设置](./media/storsimple-windows-powershell-administration/IC740877.png)

1. 单击**应用**。

1. 在**类别**窗格中，选择**翻译**。

1. **远程字符集**列表框中，选择**utf-8**。

1. 下**线绘制字符的处理**时，选择**的线条绘图使用 Unicode 码位**。 下面的插图显示正确的 PuTTY 选取。

    ![UTF Putty 设置](./media/storsimple-windows-powershell-administration/IC740878.png)

1. 单击**应用**。


您现在可以使用 PuTTY 以连接到设备串行控制台，通过执行以下步骤︰

[AZURE.INCLUDE [storsimple-use-putty](../../includes/storsimple-use-putty.md)]


## 远程连接到使用 Windows PowerShell StorSimple StorSimple
可以使用 Windows PowerShell 远程连接到您的 StorSimple 设备。 通过这种方式连接时，将不会看到一个菜单。 （您看到菜单只有在您使用串行控制台设备上进行连接。使用 Windows PowerShell 远程处理时，您将连接到特定的运行空间。 您还可以指定显示语言。 

通过使用串行控制台菜单中的更改语言选项设置的语言显示语言无关。 如果没有指定要连接的设备的区域设置将自动选取远程 PowerShell。

>[AZURE.NOTE] 如果您正在使用 Microsoft Azure 虚拟主机和 StorSimple 虚拟设备，可以使用 Windows PowerShell 远程处理和虚拟的主机连接到虚拟设备。 如果您已设置用于保存来自 Windows PowerShell 会话信息的主机上的共享位置应该意识到每个人主体包括只有已通过身份验证的用户。 因此，如果有将此共享设置为允许访问每个人，您无需指定凭据连接会使用未经身份验证的匿名主体，会出现一个错误。 若要解决此问题，在共享上承载您必须启用来宾帐户，然后授予来宾帐户的完全访问权限的共享，或您必须指定有效的凭据，以及 Windows PowerShell cmdlet。


您可以使用 HTTP 或 HTTPS 连接通过 Windows PowerShell 远程处理。 使用下面的教程中的说明︰

- [使用 http 进行远程连接](storsimple-remote-connect.md#connect-through-http)
- [使用 https 远程连接](storsimple-remote-connect.md#connect-through-https)

## 连接安全注意事项

在决定如何将连接到 Windows PowerShell 的 StorSimple 时，考虑以下方面︰

- 直接连接到串行控制台设备是安全的但通过网络交换机连接到串行控制台不是。 通过网络交换机连接到串行设备时能安全风险的警惕。

- 通过 HTTP 会话连接可能会提供比通过网络连接到串行控制台通过更高的安全性。 虽然这不是最安全的方法，它是受信任的网络上可以接受。

- 通过使用自签名证书的 HTTPS 会话连接是最安全的和推荐的选项。


## 管理 Windows PowerShell 用于 StorSimple StorSimple 设备
下表显示了在 Windows PowerShell 界面的 StorSimple 设备中的所有常见的管理任务和复杂的工作流可以执行摘要。有关每个工作流的详细信息，请单击表中适当的项。

#### Windows PowerShell StorSimple 工作流

|如果您希望这样做...|使用此过程。|
|---|---|
|注册您的设备|[配置和注册为 StorSimple 使用 Windows PowerShell 的设备](storsimple-deployment-walkthrough.md#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple) |
|配置 web 代理服务器</br>查看 web 代理服务器设置|[配置 web 代理服务器为您的 StorSimple 设备](storsimple-configure-web-proxy.md)|
|修改您的设备上的数据 0 网络接口设置|[修改数据 StorSimple 设备 0 的网络接口](storsimple-modify-data-0.md)|
|停止一个控制器 </br> 重新启动或关闭控制器 </br> 关闭设备</br>将设备重置为出厂默认设置|[管理设备控制器](storsimple-manage-device-controller.md)|
|安装维护模式更新和修补程序|[更新您的设备](storsimple-update-device.md)|
|进入维护模式 </br>退出维护模式|[StorSimple 设备模式](storsimple-device-modes.md)|
|创建支持包</br>解密和编辑支持包|[创建和管理支持包](storsimple-create-manage-support-package.md)|
|启动支持会话</br>|[对于 StorSimple 启动 Windows PowerShell 支持会话](/storsimple-contact-microsoft-support.md#start-a-support-session-in-windows-powershell-for-storsimple)
 

## StorSimple 在 Windows PowerShell 中获取帮助

在 StorSimple 的 Windows PowerShell，cmdlet 的帮助是可用的。 这种帮助的在线的最新版本中也有，您可以使用它来更新您的系统上的帮助。

在此界面中获得帮助是类似于 Windows PowerShell，大部分的帮助与相关的 cmdlet 将起作用。 您可以在 TechNet 库找到帮助为 Windows PowerShell 在线︰[与 Windows PowerShell 的脚本](http://go.microsoft.com/fwlink/?LinkID=108518)。

下面是帮助的此 Windows PowerShell 的接口，包括如何更新帮助类型的简短说明。

#### 若要获取帮助的 cmdlet

- 要为任何 cmdlet 或函数获取帮助，请使用下面的命令︰ `Get-Help <cmdlet-name>`

- 要获得任何 cmdlet 的联机帮助，请与上一个 cmdlet`-Online`参数︰ `Get-Help <cmdlet-name> -Online`

- 对于完整的帮助，您可以使用 – 完全参数和示例，使用`–Examples`参数。

#### 若要更新的帮助

您可以轻松地更新 Windows PowerShell 界面中的帮助。 执行以下步骤来更新您的系统上的帮助。

#### 若要更新 cmdlet 的帮助

1. 使用**以管理员身份运行**选项启动 Windows PowerShell。

1. 在命令提示符下，键入︰ `Update-Help`

1. 将安装更新的帮助文件。

1. 帮助文件都安装后，请键入︰ `Get-Help Get-Command`。 这将显示一个列表的 cmdlet 都有帮助。


>[AZURE.NOTE] 任何运行空间中获取所有可用的 cmdlet 的列表，请登录到相应的菜单选项和运行`Get-Command`cmdlet。

## 下一步行动
如果执行一个以上工作流时，可与 StorSimple 设备中遇到任何问题，，请参阅[故障排除 StorSimple 部署工具](storsimple-troubleshoot-deployment.md#tools-for-troubleshooting-storsimple-deployments)。

