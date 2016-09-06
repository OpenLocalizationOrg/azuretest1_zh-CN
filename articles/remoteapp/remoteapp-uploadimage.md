---
ms.openlocfilehash: ffb48355e51f28f99a1cf6dd913a98602afb38f6
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---

<properties 
    pageTitle="上载的 Azure RemoteApp 的自定义图像"
    description="了解如何为 Azure 远程应用程序上载的自定义图像" 
    services="remoteapp" 
    documentationCenter="" 
    authors="ericorman" 
    manager="mbaldwin" />

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/12/2015" 
    ms.author="ericor" />



# 上载的 Azure RemoteApp 的自定义图像

既然您已创建的自定义模板图像或已经使用更改更新它，您需要将该图像上载到 Azure RemoteApp 图像库。 使用这些步骤。 


## 在开始之前

1.      验证自定义映像满足[图像要求](remoteapp-imagereqs.md)和[应用程序的要求](remoteapp-appreqs.md)。
2.      [Azure PowerShell 模块](../install-configure-powershell.md)安装。

## 逐步在如何上传自定义映像

1.      打开 Azure 管理门户和导航到远程应用程序页面。
2.      在**模板映像**选项卡上，单击页面底部的**上载**。
4.      输入图像的友好名称，并指定存储帐户位置。 请确保位置为远程应用程序集所在的位置或想要创建一个位置。 
5.      出现提示时，下载到本地 PC 的脚本。
6.      将文本框中的命令参数复制到剪贴板。
7.      打开提升权限的 Windows PowerShell 窗口。
8.      在提升的权限的 Windows PowerShell 窗口中，导航到您下载该脚本所在的目录。
9.      粘贴复制命令，然后按**Enter**。

    上载过程将开始和持续时间取决于许多因素，包括您的网络速度和图像的大小

11.    如果您上载不像这样由于网络中断或事情成功，您可以始终继续上载过程开始。 继续上载，请再次使用相同的命令行脚本的运行。

> [AZURE.WARNING] 永远不能修改上载脚本。 已经实现了特定检查，以确保图像符合图像需求和应用要求。 

## 常见的问题

- 请确保您使用 Windows PowerShell，不 Azure PowerShell。 您需要安装 Azure PowerShell 模块，因为某些模块需要上载过程。 
- 永远不会改变该脚本，验证有为了您的方便。
- 如果在上载过程中获取锁定 vhd 文件，复制文件，或再次将其移动到新位置，并且尝试上载。 可能有一些 Windows 进程，导致无法上载。  
 