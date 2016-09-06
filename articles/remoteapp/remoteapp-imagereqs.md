---
ms.openlocfilehash: 240b86072a409b8a5373de0a252b611933674879
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---

<properties 
    pageTitle="Azure 的远程应用程序映像要求"
    description="了解如何创建图像用于 Azure RemoteApp 的要求" 
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
    ms.date="08/12/2015" 
    ms.author="elizapo" />



# Azure RemoteApp 图像的要求
Azure 的远程应用程序使用 Windows Server 2012 R2 图像来承载您想要与您的用户共享的所有程序。 若要创建自定义映像，可以使用现有的图像或[创建一个新](remoteapp-create-custom-image.md)开始。 

> [AZURE.TIP] 您知道 Azure RemoteApp 订阅 Windows Server 2012 R2 映像可用于创建您自己的模板图像 Azure VM 库中，您可以访问吗？ [签出](remoteapp-image-on-azurevm.md)。  


可以使用 Azure 远程应用程序上载的图像的要求是︰


- 自定义应用程序不在图像上本地存储数据。 这些图像是无状态的应只包含应用程序。
- 图像不包含数据，则可能会丢失。
- 图像大小应为 MBs 的倍数。 如果您试图上载不是完全相同的多个图像，传将失败。
- 图像大小必须是 127 GB 或更小。 
- 它必须是一个 VHD 文件 （VHDX 文件当前不支持）。
- VHD 不得第 2 代虚拟机。
- VHD 可以固定大小或动态扩展。 建议使用动态扩展 VHD，因为花费更少的时间要比大小固定 VHD 文件上载到 Azure。
- 必须使用主启动记录 (MBR) 分区样式初始化该磁盘。 GUID 分区表 (GPT) 分区形式不受支持。 
- VHD 必须包含 Windows Server 2012 R2 的单个安装。 它可以包含多个卷，但只有一个包含 Windows 安装。 
- 必须安装远程桌面会话主机 (RDSH) 角色和桌面体验功能。
- 远程桌面连接代理角色必须*不*被安装。
- 加密文件系统 (EFS)，则必须禁用它。
- 图像必须使用的参数的 SYSPREPed **/oobe / 一般化 /shutdown** （不要使用**/mode:vm**参数）。
- 不支持上载您的快照链从 VHD。
 
有关为 Azure RemoteApp 创建图像的详细信息，请参阅[创建 Azure RemoteApp 图像](remoteapp-imageoptions.md)。