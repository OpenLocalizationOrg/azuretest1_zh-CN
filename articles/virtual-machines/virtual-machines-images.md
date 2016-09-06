---
ms.openlocfilehash: 47ef664104ed3013b188545db5163ad161f7c128
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="关于虚拟机的图像"
    description="了解有关如何使用 Azure 中的虚拟机使用的图像。"
    services="virtual-machines"
    documentationCenter=""
    authors="KBDAzure"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/13/2015"
    ms.author="kathydav"/>

# 关于虚拟机的图像

在 Azure 使用图像来提供新的虚拟机的操作系统。 图像也可能有一个或多个数据磁盘。 从多个源的图像是可用的︰

-   Azure 提供了在[市场](http://azure.microsoft.com/gallery/virtual-machines/)中的图像。 您可以找到最新版本的 Windows 服务器和 Linux 操作系统的版本。 某些图像还包含应用程序，例如 SQL Server。 MSDN 的好处和 MSDN 即付即用订阅服务器可以访问附加图像。
-   开放源码社区提供了通过[虚拟机厂](http://vmdepot.msopentech.com/List/Index)。
-   您还可以存储和在 Azure，使用您自己的图像，通过捕获图像用作 Azure 现有虚拟机或上传图像。

## 有关 VM 映像和 OS 映像

可以在 Azure 中使用两种类型的图像︰*虚拟机映像*和*OS 映像*。 虚拟机映像包含操作系统和所有在创建映像时附加到虚拟机的磁盘。 这是图像的较新类型。 引入虚拟机映像之前，Azure 中的图像会只是一个通用的操作系统和任何其他磁盘。 包含只是图像的通用的操作系统的虚拟机映像基本上是图像的相同，OS 映像的原始类型。

您可以复制并上载创建您自己的图像，基于 Azure 中的虚拟机或其他地方运行的虚拟机。 如果您想要使用图像来创建多个虚拟机，您将需要准备使用它作为图像通过泛化它。 创建 Windows 服务器映像，请上载.vhd 文件之前对它进行一般化的服务器上运行 Sysprep 命令。 Sysprep 的详细信息，请参阅[使用 Sysprep 方法︰ 引入](http://go.microsoft.com/fwlink/p/?LinkId=392030)。 若要创建一个 Linux 映像，具体取决于分发软件，您需要运行一套特有分布情况，以及将 Azure Linux 代理运行的命令。

## 处理图像

Mac、 Linux、 Windows 和 Azure PowerShell 模块 Azure 的命令行界面 (CLI) 可用于管理 Azure 订阅该图像可用。 您还可以使用 Azure 门户的一些图像的任务，但命令行提供了更多选项。

有关资源管理器的部署与使用这些工具的信息，请参阅[使用 PowerShell 和 Azure CLI 的导航和选择 Azure 虚拟机映像](resource-groups-vm-searching.md)。

在典型部署中使用工具的示例︰

- CLI，请参阅"命令管理 Azure 虚拟机映像"中[使用 Azure CLI 的 Mac、 Linux、 Windows Azure 服务管理](virtual-machines-command-line-tools.md)
- Azure PowerShell，请参阅下面的命令列表。 查找图像上，创建一个虚拟机的示例，请参见"步骤 3:: 确定 ImageFamily"[使用 Azure PowerShell 创建和预配置的基于 Windows 的虚拟机](virtual-machines-ps-create-preconfigure-windows-vms.md)中

-   **获取所有图像**︰`Get-AzureVMImage`返回的所有您当前的订阅中提供的图像列表︰ 您的图像以及所提供的 Azure 或合作伙伴。 这意味着您可能会得到较大的列表。 下一个示例显示如何获得较短的列表。
-   **获取图像系列**︰`Get-AzureVMImage | select ImageFamily`获取的图像系列列表，通过显示**ImageFamily**属性的字符串。
-   **获取特定的家族中的所有图像**︰ `Get-AzureVMImage | Where-Object {$_.ImageFamily -eq $family}`
-   **查找 VM 映像**︰`Get-AzureVMImage | where {(gm –InputObject $_ -Name DataDiskConfigurations) -ne $null} | Select -Property Label, ImageName`这样的工作方式是筛选的 DataDiskConfiguration 属性，它仅适用于 VM 映像。 本示例还筛选为仅标签和图像名称输出。
-   **保存通用的映像**︰ `Save-AzureVMImage –ServiceName "myServiceName" –Name "MyVMtoCapture" –OSState "Generalized" –ImageName "MyVmImage" –ImageLabel "This is my generalized image"`
-   **保存专用的映像**︰ `Save-AzureVMImage –ServiceName "mySvc2" –Name "MyVMToCapture2" –ImageName "myFirstVMImageSP" –OSState "Specialized" -Verbose`
>[Azure.Tip] 如果您想要创建一个 VM 映像，其中包含数据磁盘，以及该操作系统磁盘 OSState 参数是必需的。 如果不使用该参数，则该 cmdlet 创建 OS 映像。 参数的值指示通用或专用图像，基于操作系统所在磁盘是否已准备进行重用。
-   **删除图像**︰ `Remove-AzureVMImage –ImageName "MyOldVmImage"`

## 其他资源

[不同的方法来创建一个 Linux 虚拟机](virtual-machines-linux-choices-create-vm.md)

[不同的方法创建 Windows 虚拟机](virtual-machines-windows-choices-create-vm.md)
