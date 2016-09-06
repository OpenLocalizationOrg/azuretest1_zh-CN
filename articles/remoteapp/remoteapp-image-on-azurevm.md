---
ms.openlocfilehash: 5ae8fc9a5c6002e3107636ec36f3984884f35fc1
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="创建基于 Azure VM 的 Azure RemoteApp 图像"
    description="了解如何开始与 Azure 的虚拟机为 Azure RemoteApp 创建图像。" 
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



# 创建基于 Azure 的虚拟机的 Azure RemoteApp 图像

您可以从 Azure 的虚拟机创建 Azure RemoteApp 图像 （它包含您的集合中共享的应用程序）。 您还可以选择使用我们添加到 Azure VM 映像库符合 Azure RemoteApp 图像要求的所有虚拟机映像-可用于该虚拟机映像作为起始点自己的虚拟机，如果需要。 只是查找媒体库中的"Windows 服务器远程桌面会话主机"图像。

有两个步骤来创建您自己的图像基于 Azure VM-创建图像，然后将其上载从 Azure VM 库到 Azure 远程应用程序。

## 创建自定义图像基于 Azure VM

使用下列步骤来创建基于 Azure VM 映像。

1. 创建 Azure 的虚拟机。 从 Azure 的虚拟机的图像库，您可以使用"Windows 服务器远程桌面会话主机"图像。 此图像可以满足所有 Azure RemoteApp 模板图像要求。 

    有关详细信息，请参阅[创建一个虚拟机运行 Windows](virtual-machines-windows-tutorial.md)。

2. 连接到虚拟机并安装和配置您想要通过远程应用程序共享的应用程序。 请确保执行您的应用程序所需的任何其他 Windows 配置。 

    有关详细信息，请参阅[如何登录到虚拟机运行 Windows 服务器](virtual-machines-log-on-windows-server.md)。 

3. 如果您使用的 Windows 服务器远程桌面会话主机图像，还有一个包含的验证脚本，将确保您的 VM 达到 RemoteApp pre-reqs. 若要运行脚本，请双击桌面上的**ValidateRemoteAppImage** 。 请确保脚本报告的所有错误都固定在继续下一步之前。

4. SYSPREP 一般化和捕获映像。 有关说明，请参阅[如何捕获 Windows 虚拟机作为模板使用](../virtual-machines-capture-image-windows-server.md)。

 

## 将图像导入到 Azure RemoteApp 图像库

使用下列步骤来将新图像导入到 Azure 远程应用程序︰

1. 在**模板映像**选项卡︰
    - 如果您不有任何现有图像，请单击**上载或导入模板图像**。 
    - 如果您已经有了至少一个图像，请单击**+**若要添加新的映像。

2. 选择**导入您的虚拟机从一个图像**库，然后单击**下一步**。

3. 在下一页上，从列表中选择您的自定义图像，并确认您遵循创建您的映像时，列出的步骤。 单击**下一步**。
4. 为新的远程应用程序图像输入一个名称，选择该位置，然后单击复选标记以启动导入过程。

> [AZURE.NOTE] 您可以从任何支持到 Azure Azure 远程应用程序所支持的任何位置通过 Azure 的虚拟机的 Azure 位置导入图像。 根据位置导入可能需要多达 25 分钟。

现在您准备创建新收藏集，或者[云](remoteapp-create-cloud-deployment.md)集合或[混合](remoteapp-create-hybrid-deployment.md)，具体取决于您的需要。
 