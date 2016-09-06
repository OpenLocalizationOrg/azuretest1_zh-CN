---
ms.openlocfilehash: 6233d78f89c55e5e237536fe06756dc77c3ea9bb
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="创建虚拟机运行 Windows Azure 预览门户中 |Microsoft Azure"
    description="了解如何创建运行使用 Azure 市场在 Azure 预览门户的 Windows Azure 的虚拟机"
    services="virtual-machines"
    documentationCenter=""
    authors="KBDAzure"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>
<tags
    ms.service="virtual-machines"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="08/14/2015"
    ms.author="kathydav"/>

# 创建虚拟机运行 Windows Azure 预览门户中#

> [AZURE.SELECTOR]
- [Azure 预览门户](virtual-machines-windows-tutorial.md)
- [Azure 门户](virtual-machines-windows-tutorial-classic-portal.md)
- [PowerShell︰ 资源管理器部署](virtual-machines-deploy-rmtemplates-powershell.md)
- [PowerShell︰ 经典部署](virtual-machines-ps-create-preconfigure-windows-vms.md)

本教程展示多么容易它是 Azure 的虚拟机在创建预览门户中只需几分钟。 我们将举一个例子使用 Windows Server 2012 R2 数据中心图像来创建虚拟机，但这只是许多图像提供了 Azure 之一。 图像的选项取决于您的订阅。 例如，桌面机映像可以到 MSDN 订阅服务器。

您还可以创建虚拟机的资源管理器模板或自动化工具使用您自己的图像。 若要了解有关的不同方法，请参阅[创建 Windows 虚拟机的不同方法](virtual-machines-windows-choices-create-vm.md)。

本教程使用资源管理器部署模型来创建虚拟机。 这被推荐而不是经典的部署模型，它基于服务管理 Api。 有关资源管理器详细信息，请参阅[Azure 资源管理器的概述](resource-group-overview.md)。 若要了解有关使用资源管理器的虚拟机的优点，看到[Azure 计算、 网络和存储提供商在 Azure 资源管理器中](virtual-machines-azurerm-versus-azuresm.md)。

[AZURE.INCLUDE [free-trial-note](../../includes/free-trial-note.md)]

## 视频的演练

这是本教程的演练。

[AZURE.VIDEO create-a-virtual-machine-running-windows-in-the-azure-preview-portal]

## 选择图像

1. 登录到[门户网站预览](https://portal.azure.com)。

2. 在集线器菜单上，单击**新建** > **计算** > **Windows Server 2012 R2 数据中心**。

    ![市场](./media/virtual-machines-windows-tutorial/marketplace_new.png)

    >[AZURE.TIP] 若要查找其他图像，请单击**市场**，然后搜索或筛选可用项目。

3. 在**Windows Server 2012 R2 数据中心**页上，下**选择一种部署模式**，选择**资源管理器**。 单击**创建**。

    ![在市场中的搜索](./media/virtual-machines-windows-tutorial/marketplace_search_select.png)

## 创建虚拟机

选择图像后，可以对大部分配置使用 Azure 的默认设置并快速创建虚拟机。

1. 在**创建虚拟机**刀片式服务器，单击**基础知识**。 输入所需的虚拟机、 管理**用户名**和强**密码**的**名称**。 如果您有多个订阅，则指定为新的虚拟机，以及新的或现有的**资源组**和 Azure 数据中心**位置**。

    ![配置虚拟机的基础知识](./media/virtual-machines-windows-tutorial/create_vm_basics.PNG)

    >[AZURE.NOTE]**用户名**是指将用于管理服务器的管理帐户。 创建其他人难以猜到的但您可以记住的密码。 **您将需要的用户名和密码以登录到虚拟机**。

2. 单击**大小**，然后选择满足您的需要的适当的虚拟机大小。 每个大小指定数计算内核、 内存和其他功能，例如支持高级存储，将会影响价格。 Azure 建议某些自动根据您选择的图像的大小。

    ![配置虚拟机大小](./media/virtual-machines-windows-tutorial/create_vm_size.PNG)

    >[AZURE.NOTE] 高级存储空间的 DS 系列虚拟机，在某些地区。 高级存储是最适合如数据库数据密集型工作负载的存储选项。 有关详细信息，请参阅[高级存储︰ Azure 虚拟机工作负载的高性能存储](storage-premium-storage-preview-portal.md)。

3. 单击**设置**以查看存储和网络的新的虚拟机设置。 为第一个虚拟机通常可以接受默认设置。 如果选择了支持虚拟机大小，您可以通过选择**磁盘类型**下的**最优 (SSD)**尝试高级存储。

    ![配置虚拟机设置](./media/virtual-machines-windows-tutorial/create_vm_settings.PNG)

6. 单击以查看配置选择**摘要**。 查看或更新设置完成后，请单击**创建**。

    ![配置虚拟机设置](./media/virtual-machines-windows-tutorial/create_vm_summary.PNG)

8. 当 Azure 创建虚拟机时，您可以跟踪集线器菜单的进度**通知**中。 Azure 创建虚拟机后，您会发现它在您 Startboard 除非您在**创建虚拟机**刀片式服务器中清除**附到 Startboard** 。

## 登录到虚拟机

创建虚拟机后，您需要登录到它使您可以管理它的设置，并将在其运行的应用程序。

>[AZURE.NOTE] 有关要求和故障排除技巧，请参阅[连接到 RDP 或 SSH Azure 的虚拟机](https://msdn.microsoft.com/library/azure/dn535788.aspx)。

1. 如果您还没有登录到[预览门户](https://portal.azure.com)的这么做。

2. 单击您在 Startboard 上的虚拟机。 如果您需要找到它，请单击**浏览所有** > **最近**或**浏览所有** > **虚拟机**。 然后，从列表中选择您的虚拟机。

3. 在虚拟机刀片式服务器，请单击**连接**。

    ![登录到虚拟机](./media/virtual-machines-windows-tutorial/connect_vm_portal.png)

4. 单击**打开**Windows Server 虚拟机使用的远程桌面协议文件，将自动创建。

5. 单击**连接**。

6. 键入用户名和密码创建虚拟机时, 设置，然后单击**确定**。

7. 单击**是**以验证虚拟机器的身份。

    您现在可以使用虚拟机就像使用任何其他服务器一样。

## 下一步行动

* 使用 Azure PowerShell 和 Azure CLI[查找并选择虚拟机映像](resource-groups-vm-searching.md)。
* 自动执行虚拟机和工作量部署和管理使用[Azure 资源管理器](virtual-machines-how-to-automate-azure-resource-manager.md)和[Azure 资源管理器模板](http://azure.microsoft.com/en-us/documentation/templates/)。
