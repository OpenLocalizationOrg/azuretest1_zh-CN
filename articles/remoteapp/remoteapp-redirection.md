---
ms.openlocfilehash: e4d720a894a6a9bce106858edd45e47e48f1463b
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="在 Azure 远程应用程序中使用重定向" 
    description="了解如何配置和远程应用程序中使用重定向" 
    services="remoteapp" 
    documentationCenter="" 
    authors="lizap" 
    manager="mbaldwin" />

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/10/2015" 
    ms.author="elizapo" />

# 在 Azure 远程应用程序中使用重定向

设备重定向允许用户与远程应用程序使用连接到其本地计算机、 电话或 tablet 设备进行交互。 例如，如果您提供了通过 Azure RemoteApp 的 Skype，您的用户需要使用 Skype 其 PC 上安装的照相机。 这也是适用于打印机、 扬声器、 监视器和 USB 连接的外围设备。

远程应用程序利用以提供重定向的远程桌面协议 (RDP) 和 RemoteFX。

## 默认情况下启用哪些重定向？
当使用远程应用程序时，默认情况下启用以下重定向。 圆括号中的信息显示的 RDP 设置。

- （**在该计算机上播放**） 的本地计算机上播放声音。 () audiomode:i:0
- 捕获音频从本地计算机，并发送到远程计算机 （**来自此计算机的记录**）。 () audiocapturemode:i:1
- 打印到本地打印机 (redirectprinters:i:1)
- COM 端口 (redirectcomports:i:1)
- 智能卡设备 (redirectsmartcards:i:1)
- 剪贴板 （能够复制并粘贴） (redirectclipboard:i:1)
- 清除类型字体平滑 (允许字体平滑显示︰ i:1)
- 将所有受支持即插即用设备重定向。 (devicestoredirect:s: *)

## 什么其他重定向是可用的？
默认情况下，两个重定向选项将被禁用︰

- 重定向驱动器 （驱动器映射）︰ 本地计算机的驱动器成为在远程会话中已映射的驱动器。 在远程会话中工作时，这样可以保存或打开的文件从您的本地驱动器。 
- USB 重定向︰ 您可以使用在远程会话中连接到本地计算机的 USB 设备。

## 更改远程应用程序中重定向的设置
可以通过使用 SDK 的 Microsoft Azure PowerShell 更改集合的设备重定向设置。 安装新的 PowerShell 和 SDK 后，首先将其配置为[如何安装和配置 Azure PowerShell](../powershell-install-configure.md)所述管理您的订阅。

然后，使用与以下内容类似的命令设置自定义 RDP 属性︰

    Set-AzureRemoteAppCollection -CollectionName <collection name>  -CustomRdpProperty "drivestoredirect:s:*`nusbdevicestoredirect:s:*"
    
（请注意， *' n*用作各个属性之间的分隔符）。

若要获取自定义 RDP 属性配置的列表，请运行以下 cmdlet。 请注意，只有自定义属性如下所示输出结果，并不是默认的属性︰  

    Get-AzureRemoteAppCollection -CollectionName <collection name> 
 
当您设置自定义属性必须指定所有自定义属性每次;否则则设置会恢复为已禁用。   

### 常见的示例
使用以下 cmdlet 启用驱动器重定向︰  

    Set-AzureRemoteAppCollection -CollectionName <collection name>  -CustomRdpProperty "drivestoredirect:s:*”

使用此 cmdlet 启用 USB 和驱动器重定向︰ 

    Set-AzureRemoteAppCollection -CollectionName <collection name>  -CustomRdpProperty "drivestoredirect:s:*`nusbdevicestoredirect:s:*"

使用此 cmdlet 禁用剪贴板共享︰  

    Set-AzureRemoteAppCollection -CollectionName <collection name>  -CustomRdpProperty "redirectclipboard:i:0”

> [AZURE.IMPORTANT] 一定要完全注销该集合中的所有用户 （并不只是断开连接） 之前测试更改。 为了确保完全注销用户，请转到 Azure 门户中集合中的**会话**选项卡并注销断开连接或登录的任何用户。 有时可能需要几秒钟以便本地驱动器在会话中显示在资源管理器。

## 更改 Windows 客户端上的 USB 重定向设置

如果您想要连接到远程应用程序的计算机上使用 USB 重定向，有 2 需要发生的操作。 1-需要使用 Azure PowerShell 启用 USB 重定向集合级别您的管理员联系。 2-在每个设备上需使用 USB 重定向，您需要启用组策略，允许它。 此步骤将需要为每个想要使用 USB 重定向的用户执行。
   
> [AZURE.NOTE] 对于 Windows 计算机仅支持 USB 与 Azure 远程应用程序的重定向。

### 启用 USB 找不到远程应用程序的重定向
使用以下 cmdlet 启用 USB 重定向集合级别︰

    Set-AzureRemoteAppCollection -CollectionName <collection_name> -CustomRdpProperty "nusbdevicestoredirect:s:*"

### 启用客户端计算机的 USB 重的定向

配置您的计算机上的 USB 重定向设置︰

1. 打开本地组策略编辑器 (GPEDIT。MSC)。 （在命令提示符下运行 gpedit.msc）。
2. 打开**计算机配置 \windows 组件 \ 远程桌面服务桌面连接 Client\RemoteFX USB 设备重定向**。
3. 双击**该计算机从其它支持 RemoteFX USB 设备的允许 RDP 重定向**。
4. 选择**已启用**，然后选择**管理员和 RemoteFX USB 重定向访问权限的用户**。
5. 具有管理权限，打开一个命令提示符并运行以下命令︰ 

        gpupdate /force
6. 重新启动计算机。

您还可以使用组策略管理工具来创建并应用您的域中所有计算机的 USB 重定向策略︰

1. 以域管理员身份登录到域控制器。
2. 打开组策略管理控制台。 (请单击**开始 > 管理工具 > 组策略管理**。)
3. 导航到要为其创建策略的组织单位的域。
4. 右键单击**默认域策略**，然后单击**编辑**。
5. 打开**计算机配置 \windows 组件 \ 远程桌面服务桌面连接 Client\RemoteFX USB 设备重定向**。
6. 双击**该计算机从其它支持 RemoteFX USB 设备的允许 RDP 重定向**。
7. 选择**已启用**，然后选择**管理员和 RemoteFX USB 重定向访问权限的用户**。
8. 单击**确定**。  