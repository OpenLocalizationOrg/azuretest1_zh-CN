---
ms.openlocfilehash: 979a02c6fdbac0b2107c20b61f963c5ab3e1080e
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="不同的方法创建 Windows 虚拟机"
    description="列出创建 Windows 虚拟机的不同方法，并提供说明的链接。"
    services="virtual-machines"
    documentationCenter=""
    authors="KBDAzure"
    manager="timlt"
    editor=""
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="virtual-machines"
    ms.devlang="na"
    ms.topic="index-page"
    ms.tgt_pltfrm="vm-windows"
    ms.workload="infrastructure-services"
    ms.date="07/15/2015"
    ms.author="kathydav"/>

# 不同的方法创建 Windows 虚拟机

Azure 提供不同的方法来创建一个虚拟机，因为虚拟机则适合于不同的用户和用途。 这意味着您需要做出某些选择虚拟机，以及如何创建它。 本文提供了有关这些选项和链接的摘要说明。

作为一种方法来创建和管理虚拟机和其不同资源作为一个逻辑部署单元最近引入了 azure 的资源管理器模板。 在有些地方下面，包含这种方法的说明。 若要了解有关 Azure 资源管理器以及如何作为一个单元来管理资源的详细信息，请参阅[概述][]。

## 工具选项

### GUI: Azure 的门户网站或门户网站 Azure 的预览

Azure 门户的图形用户界面是便于试验虚拟机，尤其是如果您刚开始使用 Azure。 使用 Azure 门户或 Azure 预览门户创建虚拟机︰

[创建一个运行 Windows 虚拟机][]

### 命令行解释器︰ Azure CLI 或 Azure PowerShell

如果您更喜欢使用命令行解释器中，选择用于 Mac 和 Linux 的用户或 Azure PowerShell，Azure 和自定义控制台具有 Windows PowerShell cmdlet Azure 命令行界面 (CLI)。

Azure CLI，请参见[等效资源管理器和服务管理虚拟机操作与 Mac、 Linux、 和 Windows Azure CLI 命令][]。 若要使用的模板，请参阅[部署和管理虚拟机使用 Azure 资源管理器模板和 Azure CLI][]。

Azure PowerShell，请参阅[使用 Azure PowerShell 创建和预配置 Windows 虚拟机][]使用的模板，请参阅[部署和管理虚拟机使用 Azure 资源管理器模板和 PowerShell][]。 在服务管理堆栈中创建虚拟机，请参阅[使用 Azure PowerShell 创建和预配置 Windows 虚拟机][]。

### 开发环境︰ Visual Studio

[使用 Visual Studio 创建虚拟机的网站][]

[部署使用计算、 网络和存储.NET 库的 Azure 资源][]

## 操作系统和图像中选择

选择基于您想要运行的操作系统映像。 Azure 和其合作伙伴提供了很多的图像，其中包括应用程序和工具。 或者，使用您自己的图像之一。

### Azure 的图像

这些说明将向您展示如何使用 Azure 的图像来创建自定义虚拟机与网络、 负载平衡和更多的选项。 了解[如何创建自定义虚拟机运行 Windows][]。

### 使用您自己的图像

使用图像基于 Azure 现有虚拟机通过*捕获*该虚拟机，或上载您自己的图像存储在虚拟硬盘 (VHD):

- [如何捕获 Windows 虚拟机用作图像][]
- [创建并上载到 Azure 的 Windows 服务器 VHD][]

## 下一步行动

[登录到虚拟机][]

[附加数据磁盘][]

## 其他资源
[关于 Azure 的虚拟机配置设置][]

[基本配置测试环境][]

[Azure 混合云测试环境][]

<!-- LINKS -->
[概述]: ../resource-group-overview.md

[创建一个运行 Windows 虚拟机]: virtual-machines-windows-tutorial.md

[等效资源管理器和服务管理的命令虚拟机操作与 Mac、 Linux、 和 Windows Azure CLI]:xplat-cli-azure-manage-vm-asm-arm.md
[部署和管理虚拟机使用 Azure 资源管理器模板和 Azure CLI]: virtual-machines-deploy-rmtemplates-azure-cli.md
[创建并预配置 Windows 资源管理器和 Azure PowerShell 的虚拟机]:  virtual-machines-ps-create-preconfigure-windows-resource-manager-vms.md
[部署和管理虚拟机使用 Azure 资源管理器模板和 PowerShell]: virtual-machines-deploy-rmtemplates-powershell.md
[使用 Azure PowerShell 创建和预配置 Windows 虚拟机]: virtual-machines-ps-create-preconfigure-windows-vms.md
[如何创建一个自定义的运行 Windows 的虚拟机]: virtual-machines-windows-create-custom.md

[如何捕获 Windows 虚拟机用作图像]:virtual-machines-capture-image-windows-server.md

[创建并上载到 Azure 的 Windows 服务器 VHD]: virtual-machines-create-upload-vhd-windows-server.md


[使用 Visual Studio 创建虚拟机的网站]: virtual-machines-dotnet-create-visual-studio-powershell.md
[部署使用计算、 网络和存储.NET 库的 Azure 资源]: virtual-machines-arm-deployment.md

[登录到虚拟机]: virtual-machines-log-on-windows-server.md

[附加数据磁盘]: storage-windows-attach-disk.md

[关于 Azure 的虚拟机配置设置]: http://msdn.microsoft.com/library/azure/dn763935.aspx

[基本配置测试环境]: virtual-machines-base-configuration-test-environment.md

[Azure 混合云测试环境]: virtual-machines-hybrid-cloud-test-environments.md
