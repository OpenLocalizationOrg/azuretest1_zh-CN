---
ms.openlocfilehash: f1392f391748c134fe09f8cb00fe7dc8cc855bc4
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="捕获映像运行 Windows 服务器的虚拟机"
    description="了解如何捕获的运行 Windows Azure 虚拟机 (VM) 映像。"
    services="virtual-machines"
    documentationCenter=""
    authors="KBDAzure"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/16/2015"
    ms.author="kathydav"/>

#如何捕获 Windows 虚拟机用作图像#

本文演示如何捕获以便您可以使用它为图像创建其他虚拟机运行 Windows Azure 的虚拟机。 此映像包含操作系统磁盘和任何附加到虚拟机的数据磁盘。 不包括网络配置，因此您将需要创建其他使用该模板的虚拟机配置的那些。

Azure 存储**我的图像**下面的图像。 这是存储您已上载的所有图像的位置相同的位置。 关于图像的详细信息，请参阅[关于 Azure 中的虚拟机映像] []。

##在开始之前##

这些步骤假定已经创建 Azure 的虚拟机并配置操作系统，包括附加任何数据磁盘。 如果尚未尚未执行此操作，请参阅以下说明︰

- [创建自定义虚拟机运行 Windows] []
- [如何附加到虚拟机的数据磁盘] []

> [AZURE.WARNING] 捕获时，并不是作为一种方法来备份虚拟机之后，此过程将删除原始虚拟机。 一种可能的方法来做到这一点是 Azure 备份，可作为某些区域的预览。 有关详细信息，请参阅[备份 Azure 的虚拟机](../backup/backup-azure-vms.md)。 经认证的合作伙伴提供了其他解决方案。 若要找出当前可用，搜索 Azure 市场。

##获取虚拟机##

1. 在[Azure 的门户](http://manage.windowsazure.com)，**连接**到虚拟机。 有关说明，请参阅[如何登录到运行 Windows 服务器的虚拟机] []。

2.  以管理员身份打开命令提示符窗口。

3.  将目录更改为`%windir%\system32\sysprep`，然后运行 sysprep.exe。

4.  此时将显示**系统准备工具**对话框。 请执行以下操作︰

    - 在**系统清理操作**选择**输入系统的全新安装体验 (OOBE)**并确保已**通用化**。 有关使用 Sysprep 的详细信息，请参阅[使用 Sysprep 方法︰ 引入][]。

    - 在**关机选项**中，选择**关闭**。

    - 单击**确定**。

    ![运行 Sysprep](./media/virtual-machines-capture-image-windows-server/SysprepGeneral.png)

7.  Sysprep 关闭虚拟机，从而 Azure 门户中的虚拟机的状态更改为**已停止**。

8.  在 Azure 的门户中，单击**虚拟机**并选择您想要捕获的虚拟机。

9.  在命令栏上，单击**捕获**。

    ![获取虚拟机](./media/virtual-machines-capture-image-windows-server/CaptureVM.png)

    **捕获虚拟机**对话框出现。

10. 在**映像名称**中，键入新的映像的名称。

11. 将 Windows 服务器映像添加到您的自定义图像集之前，它必须通过运行 Sysprep，按照前面的步骤中进一步推广。 单击**我已经运行在虚拟机上的 Sysprep**来指示您这样做。

12. 单击复选标记以捕获映像。 新图像在**图像**下现。

    ![图像捕获成功](./media/virtual-machines-capture-image-windows-server/VMCapturedImageAvailable.png)

##下一步行动##
图像就可以被用来创建虚拟机。 若要执行此操作，您将通过使用**剪辑库中的**菜单项并选择您刚才创建的映像创建虚拟机。 有关说明，请参阅[创建自定义虚拟机运行 Windows] []。


[有关在 Azure 中的虚拟机映像]: http://msdn.microsoft.com/library/azure/dn790290.aspx
[创建自定义虚拟机运行 Windows]: virtual-machines-windows-create-custom.md
[如何附加到虚拟机的数据磁盘]: storage-windows-attach-disk.md
[如何登录到运行 Windows 服务器的虚拟机]: virtual-machines-log-on-windows-server.md
[如何使用 Sysprep: 简介]: http://technet.microsoft.com/library/bb457073.aspx
[运行 Sysprep.exe]: ./media/virtual-machines-capture-image-windows-server/SysprepCommand.png
[输入 Sysprep.exe 选项]: ./media/virtual-machines-capture-image-windows-server/SysprepGeneral.png
[虚拟机已停止]: ./media/virtual-machines-capture-image-windows-server/SysprepStopped.png
[捕获映像的虚拟机]: ./media/virtual-machines-capture-image-windows-server/CaptureVM.png
[请输入映像名称]: ./media/virtual-machines-capture-image-windows-server/Capture.png
[图像捕获成功]: ./media/virtual-machines-capture-image-windows-server/CaptureSuccess.png
[使用捕获的图像]: ./media/virtual-machines-capture-image-windows-server/MyImagesWindows.png
