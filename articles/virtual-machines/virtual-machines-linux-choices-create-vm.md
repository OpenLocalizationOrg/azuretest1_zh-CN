---
ms.openlocfilehash: 58304faef36a0ee56714966344fd7e7a0eb384f0
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="不同的方法来创建一个 Linux 虚拟机"
    description="列出了不同的方式来创建一个 Linux 虚拟机并提供说明的链接。"
    services="virtual-machines"
    documentationCenter=""
    authors="dsk-2015"
    manager="timlt"
    editor=""
    tags="azure-service-management,azure-resource-manager"/>

<tags
    ms.service="virtual-machines"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-linux"
    ms.workload="infrastructure-services"
    ms.date="08/12/2015"
    ms.author="dkshir"/>

# 不同的方法来创建一个 Linux 虚拟机

Azure 提供不同的方法来创建一个虚拟机，因为 Vm 则适合于不同的用户和用途。 这意味着您将需要做出一些 VM 和如何将创建的选择。 本文提供了有关这些选项和链接的摘要说明。

作为一种方法来创建和管理虚拟机和其不同资源作为一个逻辑部署单元最近引入了 azure 的资源管理器模板。 在有些地方下面，包含这种方法的说明。 若要了解有关 Azure 资源管理器以及如何作为一个单元来管理资源的详细信息，请参阅[概述][]。

## 工具选项

### GUI: Azure 的门户网站或门户网站预览

Azure 门户的图形用户界面是便于试验虚拟机，尤其是如果您刚开始使用 Azure。 使用[Azure 门户](http://manage.windowsazure.com)或[Azure 预览门户](http://portal.azure.com)创建虚拟机。 一般的说明，请参阅[创建自定义虚拟机][]并从**库**中选择任何 Linux 映像。 请注意， [Azure 门户](http://manage.windowsazure.com)创建虚拟机使用只经典的部署模型。

### 命令行解释器︰ Azure CLI 或 Azure PowerShell

如果您更喜欢使用命令行解释器中，选择用于 Mac 和 Linux 的用户或 Azure PowerShell，Azure 和自定义控制台具有 Windows PowerShell cmdlet Azure 命令行界面 (CLI)。

Azure CLI，请参阅[创建虚拟机运行 Linux][]。 要使用的模板，请参阅[部署和管理虚拟机使用 Azure 资源管理器模板和 Azure CLI][]。

Azure PowerShell，请参阅[使用 Azure PowerShell 创建和预配置的基于 Linux 的虚拟机][]。 要使用的模板，请参阅[部署和管理虚拟机使用 Azure 资源管理器模板和 PowerShell][]。

### Visual Studio 的开发环境︰

[使用 Visual Studio 创建虚拟机的网站][]

[部署使用计算、 网络和存储.NET 库的 Azure 资源][]

## 操作系统和图像中选择

选择基于您想要运行的操作系统映像。 Azure 和其合作伙伴提供了很多的图像，其中包括应用程序和工具。 或者，使用您自己的图像之一。

### Azure 的图像

这些说明将向您展示如何使用 Azure 的图像来创建自定义虚拟机与网络、 负载平衡和更多的选项。 了解[如何创建自定义虚拟机运行在 Azure 中的 Linux][]。

### 使用您自己的图像

使用图像基于 Azure 现有虚拟机通过*捕获*该虚拟机，或上载您自己的图像存储在虚拟硬盘 (VHD):

- [如何捕获 Linux 虚拟机作为模板使用 CLI 使用][]
- [创建并上载包含 Linux 操作系统虚拟硬盘][]

## 下一步行动

[登录到虚拟机][]

[附加数据磁盘][]

## 其他资源
[关于 Azure VM 配置设置][]

[基本配置测试环境][]

[Azure 混合云测试环境][]

[等效资源管理器并为 VM 操作与 Mac、 Linux、 和 Windows Azure CLI 服务管理命令][]

<!-- LINKS -->
[概述]: ../resource-group-overview.md

[创建一个运行 Windows 虚拟机]: virtual-machines-windows-tutorial.md
[创建一个运行 Linux 虚拟机]: virtual-machines-linux-tutorial.md

[等效资源管理器并为 VM 操作与 Mac、 Linux、 和 Windows Azure CLI 服务管理命令]:xplat-cli-azure-manage-vm-asm-arm.md
[部署和管理虚拟机使用 Azure 资源管理器模板和 Azure CLI]: virtual-machines-deploy-rmtemplates-azure-cli.md
[部署和管理虚拟机使用 Azure 资源管理器模板和 PowerShell]:  virtual-machines-deploy-rmtemplates-powershell.md
[使用 Azure PowerShell 创建和预配置的基于 Linux 的虚拟机]: virtual-machines-ps-create-preconfigure-linux-vms.md

[如何创建自定义运行 Linux 在 Azure 中的虚拟机]: virtual-machines-linux-create-custom.md
[如何捕获 Linux 虚拟机作为模板使用 CLI 使用]: virtual-machines-linux-capture-image.md

[创建并上载包含 Linux 操作系统虚拟硬盘]: virtual-machines-linux-create-upload-vhd.md

[使用 Visual Studio 创建虚拟机的网站]: virtual-machines-dotnet-create-visual-studio-powershell.md
[部署使用计算、 网络和存储.NET 库的 Azure 资源]: virtual-machines-arm-deployment.md

[登录到虚拟机]: virtual-machines-linux-how-to-log-on.md

[附加数据磁盘]: virtual-machines-linux-how-to-attach-disk.md

[关于 Azure VM 配置设置]: http://msdn.microsoft.com/library/azure/dn763935.aspx
[基本配置测试环境]: virtual-machines-base-configuration-test-environment.md
[Azure 混合云测试环境]: virtual-machines-hybrid-cloud-test-environments.md

[创建一个运行 Linux 虚拟机]: virtual-machines-linux-tutorial.md
[创建自定义的虚拟机]: virtual-machines-create-custom.md
