---
ms.openlocfilehash: f845aa900099ed6771582f5ca5510b7783a2f8aa
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="创建运行 Windows Azure 中的虚拟机"
    description="在 Azure 门户创建 Windows 虚拟机。"
    services="virtual-machines"
    documentationCenter=""
    authors="KBDAzure"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/11/2015"
    ms.author="kathydav"/>

# 创建虚拟机运行 Windows Azure 门户中

> [AZURE.SELECTOR]
- [Azure 预览门户](virtual-machines-windows-tutorial.md)
- [Azure 门户](virtual-machines-windows-tutorial-classic-portal.md)
- [PowerShell︰ 资源管理器部署](virtual-machines-deploy-rmtemplates-powershell.md)
- [PowerShell︰ 经典部署](virtual-machines-ps-create-preconfigure-windows-vms.md)


本教程展示在 Azure 的门户网站中创建 Azure 的虚拟机 (VM) 是多么容易。 我们将举一个例子，使用的 Windows 服务器映像，但这是只是 Azure 提供许多图像之一。 请注意，图像的选项取决于您的订阅。 例如，桌面机映像可以到 MSDN 订阅服务器。

您还可以创建虚拟机使用[您自己的图像](virtual-machines-create-upload-vhd-windows-server.md)。 若要了解有关这和其他方法，请参阅[创建 Windows 虚拟机的不同方法](virtual-machines-windows-choices-create-vm.md)。

[AZURE.INCLUDE [free-trial-note](../../includes/free-trial-note.md)]

## 视频的演练

这是本教程的演练。

[AZURE.VIDEO creating-a-windows-vm-on-microsoft-azure-classic-portal]

## <a id="createvirtualmachine"> </a>如何创建虚拟机

本节说明如何使用 Azure 门户中的**剪辑库中的**选项来创建虚拟机。 此选项提供了更多比**快速创建**选项的配置选项。 例如，如果您想要将虚拟机加入到虚拟网络，您需要使用**剪辑库中的**选项。

> [AZURE.NOTE] 您也可以尝试更丰富、 可自定义[Azure 预览门户](https://portal.azure.com)来创建虚拟机，使用增强的监视和诊断，使用高级存储等。 配置虚拟机在两个门户网站中的可用选项明显重叠，但并不完全相同。 例如，使用预览门户配置高级存储的虚拟机。

[AZURE.INCLUDE [virtual-machines-create-WindowsVM](../../includes/virtual-machines-create-windowsvm.md)]

## 下一步行动

- 登录到虚拟机。 有关说明，请参阅[如何登录到运行 Windows 服务器的虚拟机](virtual-machines-log-on-windows-server.md)。

- 附加的磁盘来存储数据。 您可以附加空磁盘和磁盘包含数据。 有关说明，请参阅[附加的数据磁盘教程](storage-windows-attach-disk.md)。

## 其他资源

若要了解有关内容可以为虚拟机配置和您可以做到这一点，当看到[关于 Azure VM 配置设置](http://msdn.microsoft.com/library/azure/dn763935.aspx)。
