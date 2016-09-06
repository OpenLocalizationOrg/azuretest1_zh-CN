---
ms.openlocfilehash: 8ed0c102b32f61b052fc311d3fce3c5e7cbbf5cc
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="创建并上载到 Azure 的 Windows 服务器 VHD"
    description="了解如何创建并上载虚拟硬盘 (VHD) 在具有 Windows 服务器操作系统 Azure。"
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
    ms.date="04/29/2015"
    ms.author="kathydav"/>


# 创建并上载到 Azure 的 Windows 服务器 VHD#

本文介绍了如何使用操作系统的系统上载虚拟硬盘 (VHD)，因此您可以将它用作图像来创建基于该映像的虚拟机。 有关磁盘和 Microsoft Azure 中的 Vhd 的详细信息，请参阅[有关磁盘和虚拟机的 Vhd](virtual-machines-disks-vhds.md)。

> [AZURE.NOTE] 当您创建基于图像的虚拟机时，可以自定义操作系统设置为适合于您要在虚拟机上运行的应用程序。 此配置为该虚拟机保存，不会对图像的影响。

## 先决条件##

本文假定您具有以下对象︰

1. **Azure 订阅**-如果您没有，您可以[免费开设 Azure 帐户](/pricing/free-trial/?WT.mc_id=A261C142F)︰ 获取积分可用于试验 Azure 服务付费，并且即使他们习惯之后最多可以保留该帐户并使用释放 Azure 服务，例如网站。 永远不会将收取您的信用卡，除非您明确更改您的设置并问计。 您还可以[激活 MSDN 订户权益](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F)︰ 您 MSDN 订阅提供了积分可用于付费的 Azure 服务每月。

2. **Microsoft Azure PowerShell** -有 Microsoft Azure PowerShell 模块安装和配置为使用您的订购。 若要下载模块，请参阅[Microsoft Azure 下载](http://azure.microsoft.com/downloads/)。 教程，说明如何安装和配置模块位于[此处](../powershell-install-configure.md)。 [添加 AzureVHD](http://msdn.microsoft.com/library/azure/dn495173.aspx) cmdlet 将用于上载 VHD。

3. **A 支持的 Windows 操作系统的.vhd 文件中存储**的受支持的 Windows Server 操作系统安装到虚拟硬盘。 创建.vhd 文件的形式而存在的多个工具。 如 Hyper-V 虚拟化解决方案可用于创建虚拟机并安装操作系统。 有关说明，请参阅[安装 Hyper-V 角色并配置虚拟机](http://technet.microsoft.com/library/hh846766.aspx)。

> [AZURE.NOTE] 在 Microsoft Azure 中不支持 VHDX 格式。 您可以将磁盘转换为使用 Hyper-V 管理器或[转换 VHD cmdlet](http://technet.microsoft.com/library/hh848454.aspx)的 VHD 格式。 对此教程可以找到[这里](http://blogs.msdn.com/b/virtual_pc_guy/archive/2012/10/03/using-powershell-to-convert-a-vhd-to-a-vhdx.aspx)。

 支持以下 Windows 服务器版本︰

操作系统|SKU|服务包|体系结构
---|---|---|---
Windows Server 2012 R2|所有版本|N/A|x64
Windows Server 2012|所有版本|N/A|x64
Windows Server 2008 R2|所有版本|SP1|x64

## 第 1 步︰ 准备要上载的图像 ##

可以将图像上载到 Azure 之前，您需要使用 Sysprep 命令将其一般化。 Sysprep 有关的详细信息，请参阅[使用 Sysprep 方法︰ 引入](http://technet.microsoft.com/library/bb457073.aspx)。

从操作系统的安装到虚拟机，请完成以下过程︰

1. 登录到操作系统。

2. 以管理员身份打开命令提示符窗口。 将目录更改为**%windir%\system32\sysprep**，然后再运行`sysprep.exe`。

    ![打开一个命令提示符窗口](./media/virtual-machines-create-upload-vhd-windows-server/sysprep_commandprompt.png)

3.  此时将显示**系统准备工具**对话框。

    ![Sysprep 开始](./media/virtual-machines-create-upload-vhd-windows-server/sysprepgeneral.png)

4.  在**系统准备工具**中，选择**输入系统全新体验 (OOBE)**并确保已**通用化**。

5.  在**关机选项**中，选择**关闭**。

6.  单击**确定**。

## 步骤 2︰ 创建在 Azure 存储帐户。 ##

您需要在 Azure 上载.vhd 文件，以便它可用于在 Azure 创建虚拟机的存储帐户。 您可以使用 Azure 门户创建存储帐户。

1. 登录到[门户网站](http://manage.windowsazure.com)。

2. 在命令栏上，单击**新建**。

3. 单击**数据服务** > **存储** > **快速创建**。

    ![快速创建存储帐户。](./media/virtual-machines-create-upload-vhd-windows-server/Storage-quick-create.png)

4. 请填写字段，如下所示︰

 - 在**URL**下键入用于在 URL 中使用的存储帐户。 该条目可以包含从 3 日至 24 日小写字母和数字。 此名称将成为使用为解决斑点、 队列或订阅表资源的 URL 中的主机名称。
 - 选择**位置或相关性组**存储帐户。 相似性组允许您将您的云服务和存储放在同一个数据中心。
 - 决定是否使用**地区复制**的存储帐户。 默认情况下，各地区复制打开。 此选项将数据复制到辅助位置，免费给您，以便您的存储故障转移到该位置在主站点发生重大故障时。 第二的位置，自动分配，不能更改。 如果您需要更好地控制您的云存储由于法律要求或其组织的策略位置，您可以关闭地区复制。 但是，请注意，如果您以后打开地区复制，将收取一次性数据传输费用将您现有的数据复制到辅助位置。 而地理复制存储服务均提供折扣。 管理地区复制存储帐户的详细信息可以在以下位置找到︰[创建、 管理或删除存储帐户](../storage-create-storage-account/#replication-options)。

      ![输入存储帐户详细信息](./media/virtual-machines-create-upload-vhd-windows-server/Storage-create-account.png)

5. 单击**创建存储帐户**。 该帐户现在显示在**存储**下。

    ![已成功创建存储帐户](./media/virtual-machines-create-upload-vhd-windows-server/Storagenewaccount.png)

6. 对于您上载 Vhd，接下来，创建一个容器。 单击存储帐户的名称，然后单击**容器**。

    ![存储帐户详细信息](./media/virtual-machines-create-upload-vhd-windows-server/storageaccount_detail.png)

7. 单击**创建一个容器**。

    ![存储帐户详细信息](./media/virtual-machines-create-upload-vhd-windows-server/storageaccount_container.png)

8. 键入您的容器的**名称**，然后选择**访问**策略。

    ![容器名称](./media/virtual-machines-create-upload-vhd-windows-server/storageaccount_containervalues.png)

    > [AZURE.NOTE] 默认情况下，容器是私有的并可以仅通过帐户所有者。 要允许公共的 blob 容器，但不是容器属性和元数据的读取访问权限，请使用**公钥 Blob**选项。 要允许完整公钥容器和 blob 的读取访问权限，请使用**公共容器**选项。

## 第 3 步︰ 准备到 Microsoft Azure 的连接 ##

您可以上载.vhd 文件之前，您需要建立您的计算机和您的订购 Azure 之间的安全连接。 您可以使用 Microsoft Azure Active Directory 方法或证书方法要这样做。

> [AZURE.TIP] 若要开始使用 Azure PowerShell，请参阅[如何安装和配置 Microsoft Azure PowerShell](../install-configure-powershell.md)。 有关的一般信息，请参阅[开始使用 Microsoft Azure Cmdlet。](https://msdn.microsoft.com/library/azure/jj554332.aspx)

### 使用 Microsoft Azure 的广告方法

1. 打开 Azure PowerShell 控制台。

2. 类型：  
       `Add-AzureAccount`

    此命令会打开登录窗口，以便您可以与您的工作或学校帐户签名。

    ![PowerShell 窗口](./media/virtual-machines-create-upload-vhd-windows-server/add_azureaccount.png)

3. Azure 进行身份验证并保存的凭据信息，然后关闭该窗口。

### 使用证书的方法

1. 打开 Azure PowerShell 控制台。

2.  类型︰       `Get-AzurePublishSettingsFile`。

3. 打开一个浏览器窗口，并提示您下载的.publishsettings 文件。 它包含信息以及 Microsoft Azure 订阅的证书。

    ![浏览器下载页面](./media/virtual-machines-create-upload-vhd-windows-server/Browser_download_GetPublishSettingsFile.png)

3. 将.publishsettings 文件保存。

4. 类型︰    `Import-AzurePublishSettingsFile <PathToFile>`

    其中`<PathToFile>`到.publishsettings 文件的完整路径。

## 步骤 4︰ 将.vhd 文件上载

在上载.vhd 文件时，可以在 blob 存储内任意位置.vhd 文件。 在下面的命令示例中， **BlobStorageURL**是一个 URL，您在步骤 2 中创建的存储帐户， **YourImagesFolder**是 blob 存储中的容器要存储图像的位置。 **VHDName**是在门户来标识虚拟硬盘中显示的标签。 **PathToVHDFile**是完整路径和名称的.vhd 文件。

1. 从前面的步骤中使用 Azure PowerShell 窗口，请键入︰

    `Add-AzureVhd -Destination "<BlobStorageURL>/<YourImagesFolder>/<VHDName>.vhd" -LocalFilePath <PathToVHDFile>`

    ![PowerShell 添加 AzureVHD](./media/virtual-machines-create-upload-vhd-windows-server/powershell_upload_vhd.png)

    有关添加 AzureVhd cmdlet 的详细信息，请参阅[添加 AzureVhd](http://msdn.microsoft.com/library/dn495173.aspx)。

## 步骤 5︰ 将图像添加到您的自定义图像列表 ##

.Vhd 上载后，您将其添加为图像到与订阅相关的自定义映像的列表。

1. 从门户下**的所有项目**，, 单击**虚拟机**。

2. 在虚拟机下单击**图像**。

3. 单击**创建的图像**。

    ![PowerShell 添加 AzureVHD](./media/virtual-machines-create-upload-vhd-windows-server/Create_Image.png)

4. 在**创建从一个 VHD 映像**，请执行以下操作︰

    - 指定的**名称**。

    - 指定**说明**。

    - 若要指定**您的 VHD 的 URL**，请单击文件夹按钮以打开以下窗口︰

    ![选择 VHD](./media/virtual-machines-create-upload-vhd-windows-server/Select_VHD.png)

    - 选择您的 VHD 位于存储帐户并单击**打开**。 此操作将返回到在**从 VHD 映像创建**窗口。

    - 返回到**创建从一个 VHD 映像**窗口后操作系统家族中选择您的操作系统。

    - 检查**已运行 Sysprep 虚拟机与此 VHD 上的**承认，通用第 1 步中的操作系统，然后单击**确定**。

    ![添加图像](./media/virtual-machines-create-upload-vhd-windows-server/Create_Image_From_VHD.png)

5. **可选︰**您可以**添加 AzureVMImage** cmdlet 而不是门户网站，为图像添加您的 VHD。 在 Azure PowerShell 控制台中，请键入︰

    `Add-AzureVMImage -ImageName <Your Image's Name> -MediaLocation <location of the VHD> -OS <Type of the OS on the VHD>`

    ![PowerShell 添加 AzureVMImage](./media/virtual-machines-create-upload-vhd-windows-server/add_azureimage_powershell.png)

6. 完成上述步骤后，当选择**图像**选项卡列出新的映像。

    ![自定义图像](./media/virtual-machines-create-upload-vhd-windows-server/vm_custom_image.png)

    创建虚拟机时，此新图像现下**我的图像**。 有关说明，请参阅[如何创建自定义虚拟机运行 Windows](virtual-machines-windows-create-custom.md)。

    ![从自定义映像创建虚拟机](./media/virtual-machines-create-upload-vhd-windows-server/create_vm_custom_image.png)

    > [AZURE.TIP] 如果当您尝试创建一个虚拟机，与此错误消息，可能会出错"VHD https://XXXXX...有不受支持的虚拟 YYYY 字节大小。 大小必须是一个整数 （以 Mb)，"这意味着您的 VHD 的 MBs 的整数并不需要是固定的大小 VHD。 请尝试使用而不门户网站**添加 AzureVMImage** PowerShell cmdlet 添加图像 （请参阅上面的第 5 步）。 Azure 的 cmdlet 确保 VHD 符合 Azure 的要求。

## 下一步行动 ##

在创建虚拟机之后, 尝试创建 SQL Server 虚拟机。 有关说明，请参阅[Microsoft Azure 上的 SQL Server 虚拟机的资源调配](virtual-machines-provision-sql-server.md)。

[第 1 步︰ 准备要上载的图像]: #prepimage
[步骤 2︰ 创建在 Azure 存储帐户。]: #createstorage
[第 3 步︰ 准备到 Azure 的连接]: #prepAzure
[步骤 4︰ 将.vhd 文件上载]: #upload
