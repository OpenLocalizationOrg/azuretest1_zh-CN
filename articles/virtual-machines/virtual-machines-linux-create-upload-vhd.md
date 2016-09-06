---
ms.openlocfilehash: 333befe86a8586e525593176bb7feae35ebf2036
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="创建并上载 VHD Linux 在 Azure 中"
    description="了解如何创建并上载 Azure 虚拟硬盘 (VHD) 包含 Linux 操作系统。"
    services="virtual-machines"
    documentationCenter=""
    authors="dsk-2015"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/29/2015"
    ms.author="dkshir"/>

# 创建并上载包含 Linux 操作系统虚拟硬盘

本文演示如何创建并上载虚拟硬盘 (VHD)，因此您可以将它用作您自己的图像在 Azure 中创建虚拟机。 您将学习如何准备操作系统，因此可以使用它来创建基于该图像的多个虚拟机。 请注意，这篇文章是指使用传统部署模型创建的虚拟机。

[AZURE.INCLUDE [free-trial-note](../../includes/free-trial-note.md)]

Azure 中的虚拟机运行基于的图像时创建虚拟机，您选择的操作系统。 将图像存储在 VHD 格式，以.vhd 文件的形式存储帐户中。 有关详细信息，请参阅[有关磁盘和 Azure 中的图像](https://msdn.microsoft.com/library/azure/jj672979.aspx)。

当您创建虚拟机时，您可以自定义某些操作系统设置使它们更适合于您想要运行的应用程序。 有关说明，请参阅[如何创建自定义的虚拟机](virtual-machines-create-custom.md)。

**重要提示**︰ 在[Linux 上 Azure-Endorsed 分布](virtual-machines-../linux-endorsed-distributions.md)的支持版本下指定详细介绍了 Azure 平台适用于仅在具有配置使用的已背书的分布之一时运行 Linux 操作系统的虚拟机的 SLA。 在 Azure 图像库中的所有 Linux 发行都都具有所需配置的背书的分布。


##先决条件##
本文假定您具有以下各项︰

- **一个管理证书**-您已经创建了订阅其想要上载 VHD 并导出到.cer 文件的证书管理证书。 有关创建证书的详细信息，请参阅[创建并上载 Azure 管理证书](https://msdn.microsoft.com/library/azure/gg551722.aspx)。

- **Linux 操作系统的系统上安装的.vhd 文件**到虚拟硬盘安装支持的 Linux 操作系统。 存在多个工具来创建.vhd 文件的形式，例如使用如 Hyper-V 虚拟化解决方案创建.vhd 文件并安装操作系统。 有关说明，请参阅[安装 Hyper-V 角色并配置虚拟机](http://technet.microsoft.com/library/hh846766.aspx)。

    **重要提示**︰ 在 Azure 不支持较新的 VHDX 格式。 您可以将磁盘转换为使用 Hyper-V 管理器或转换 vhd cmdlet 的 VHD 格式。

    背书的分配的列表，请参阅[Azure-Endorsed 分配上的 Linux](../linux-endorsed-distributions.md)。 另外，请参见本文末尾处的一节[Non-Endorsed 分布的信息](virtual-machines-linux-create-upload-vhd-generic.md)

- **Azure 命令行界面**-如果使用 Linux 操作系统的系统来创建您的映像，则使用[Azure 命令行界面](../virtual-machines-command-line-tools.md)来上载 VHD。

- **Azure Powershell 工具**- `Add-AzureVhd` cmdlet 还可用于上载 VHD。 请参阅[Azure 下载](http://azure.microsoft.com/downloads/)下载 Azure Powershell cmdlet。 参考信息，请参阅[添加 AzureVhd](https://msdn.microsoft.com/library/azure/dn495173.aspx)。

## <a id="prepimage"> </a>第 1 步︰ 准备要上载的图像 ##

Azure 支持多种 Linux 发行版本 （请参阅[认可分配](../linux-endorsed-distributions.md)）。 下面的文章将带您了解如何准备在 Azure 受支持的各种 Linux 发行版本︰

- **[基于 centOS 的分配](virtual-machines-linux-create-upload-vhd-centos.md)**
- **[Oracle Linux](virtual-machines-linux-create-upload-vhd-oracle.md)**
- **[SLES openSUSE &](../virtual-machines-linux-create-upload-vhd-suse)**
- **[Ubuntu](virtual-machines-linux-create-upload-vhd-ubuntu.md)**
- **[其他 – 非认可分配](virtual-machines-linux-create-upload-vhd-generic.md)**

此外请参见**[Linux 安装说明](virtual-machines-linux-create-upload-vhd-generic.md#linuxinstall)**Linux 映像准备 Azure 的更多提示。

上面这些指南中的步骤之后应该有准备到 Azure 上载 VHD 文件。


## <a id="connect"> </a>步骤 2︰ 准备到 Azure 的连接 ##

您可以上载.vhd 文件之前，您需要建立您的计算机和您的订购 Azure 之间的安全连接。


### 如果使用 Azure CLI

使用 Azure 的广告方法登录到︰

1. 打开 Azure CLI 窗口

2. 类型：

    `azure login`

    出现提示时，键入您的用户名和密码。

**或者**，使用一个 PublishSettings 文件改为︰

1. 打开 Azure CLI 窗口

2. 类型：

    `azure account download`

    此命令会打开一个浏览器窗口，并自动下载.publishsettings 文件，其中包含的信息和 Azure 订购的证书。

3. 将.publishsettings 文件保存

4. 类型：

    `azure account import <PathToFile>`

    其中`<PathToFile>`到.publishsettings 文件的完整路径。

    详细信息，请参阅[连接到 Azure CLI 从 Azure](../xplat-cli-connect.md)。


### 如果使用 Azure PowerShell

使用 Azure 的广告方法登录到︰

1. 打开 Azure PowerShell 窗口。

2. 类型：

    `Add-AzureAccount`

    出现提示时，输入您的组织的用户 id 和密码。

**或者**，改为使用 PublishSettings 文件︰

1. 打开 Azure PowerShell 窗口。

2. 类型：

    `Get-AzurePublishSettingsFile`

    此命令会打开一个浏览器窗口，并自动下载.publishsettings 文件，其中包含的信息和 Azure 订购的证书。

3. 将.publishsettings 文件保存。

4. 类型：

    `Import-AzurePublishSettingsFile <PathToFile>`

    其中`<PathToFile>`到.publishsettings 文件的完整路径。

    有关详细信息，请参阅[如何安装和配置 Azure PowerShell](powershell-install-configure.md)

> [AZURE.NOTE] 我们建议您登录到 Azure 订购，从 Azure CLI 或 Azure PowerShell 使用较新的 Azure Active Directory 方法。

## <a id="upload"> </a>步骤 3︰ 将图像传到 Azure ##

### 如果使用 Azure CLI

使用 Azure CLI 上载的图像。 您可以通过使用以下命令上载图像︰

        azure vm image create <image-name> --location <location-of-the-data-center> --os Linux <source-path-to the vhd>

### 如果使用 PowerShell

您将需要上载到 VHD 文件的存储帐户。 或者，您可以选择一个现有或新建一个。 若要创建存储帐户，请参阅创建[存储帐户](../storage-create-storage-account.md)

在上载.vhd 文件时，可以在 blob 存储内任意位置.vhd 文件。 在下面的命令示例中， **BlobStorageURL**是一个 URL，您打算使用的存储帐户， **YourImagesFolder**是 blob 存储中的容器要存储图像的位置。 **VHDName**是在[管理门户](http://manage.windowsazure.com)来标识虚拟硬盘中显示的标签。 **PathToVHDFile**是完整路径和名称的.vhd 文件。

从前面的步骤中使用 Azure PowerShell 窗口，请键入︰

        Add-AzureVhd -Destination <BlobStorageURL>/<YourImagesFolder>/<VHDName> -LocalFilePath <PathToVHDFile>

有关详细信息，请参阅 [添加-AzureVhd] ((https://msdn.microsoft.com/library/azure/dn495173.aspx)。




[第 1 步︰ 准备要上载的图像]: #prepimage
[步骤 2︰ 准备到 Azure 的连接]: #connect
[步骤 3︰ 将图像传到 Azure]: #upload
