---
ms.openlocfilehash: 0ec5d011d26e3c4fc0929cba972df84b12e7cd5f
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="Azure 的资源管理器体系结构"
   description="了解该体系结构资源管理器和计算、 网络和存储资源提供者之间的关系。"
   services="virtual-machines"
   documentationCenter=""
   authors="davidmu1"
   manager="timlt"
   editor=""
   tags="azure-resource-manager"/>

<tags
    ms.service="azure-resource-manager"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/25/2015"
    ms.author="davidmu"/>

# Azure 的资源管理器体系结构

本文概述了用于创建基于基础结构的应用程序和工作负载的服务管理和资源管理器的结构。

## 服务管理体系结构

我们讨论的 Azure 资源管理器和各种资源提供程序体系结构之前，让我们回顾一下当前存在 Azure 服务管理的体系结构。 在 Azure 服务管理、 计算、 存储或网络由提供承载虚拟机的资源︰

- 所需的云服务，它就像一个用于承载虚拟机 （计算）。 虚拟机自动提供网络接口卡 (NIC) 和通过 Azure 分配 IP 地址。 此外，云服务包含外部负载平衡器实例、 一个公用 IP 地址，并允许基于 Windows 的虚拟机的远程桌面和远程 PowerShell 通信和基于 Linux 的虚拟机的安全外壳协议 (SSH) 通信的默认终结点。
- 存储的虚拟机，包括操作系统、 临时的和其他数据磁盘 （存储） Vhd 所需的存储帐户。
- 作为一个额外的容器，在其中可以创建子网的结构，并指出虚拟机所在的子网的可选虚拟网络 （网络）。

这里是组件和 Azure 服务管理它们之间的关系。

![](./media/virtual-machines-azure-resource-manager-architecture/arm_arch1.png)

## 体系结构资源管理器的

对于 Azure 资源管理器中，资源提供程序都支持创建您需要的配置中运行虚拟机的单个资源。 为虚拟机有三个主要的资源提供程序︰

- 计算资源提供程序 (CRP): 支持虚拟机和可选的可用性设置的实例。
- 存储资源提供程序 (SRP): 支持所需的存储的虚拟机，包括它们的操作系统和其他数据磁盘 Vhd 的存储帐户。
- 网络资源提供程序 (NRP): 支持必需的 Nic、 虚拟机的 IP 地址和子网内虚拟网络和可选的负载平衡器，负载平衡器 IP 地址和网络安全组。

此外，资源提供程序中的资源之间的关系︰

- 虚拟机取决于定义 SRP （必填） 的 blob 存储在存储其磁盘中的特定的存储帐户。
- 虚拟机引用 NRP （必填） 和可用性设置定义中 CRP （可选） 中定义的特定 NIC。
- NIC 引用虚拟机分配的 IP 地址 （必填） 虚拟机 （必需的），并为网络安全组 （可选） 在虚拟网络的子网。
- 虚拟网络中的子引用网络安全组 （可选）。
- 负载平衡器实例引用后端池中的 IP 地址，包括虚拟机 （可选） 的 NIC，并引用一个负载平衡器公用或专用的 IP 地址 （可选）。

![](./media/virtual-machines-azure-resource-manager-architecture/arm_arch2.png)

组件化的资源配置在 Azure 托管 IT 工作负荷的基础结构时可以更加灵活。 Azure 的资源管理器模板利用这种灵活性可以创建的一套特定配置所需的相关资源。 当执行模板，可以确保资源管理器配置资源是以正确的顺序来保护的依赖项和引用。 例如，资源管理器将不创建虚拟机的 NIC，直到它已创建虚拟网络子网和 IP 地址 （网络安全组是可选的）。

资源组是一种逻辑容器，包含应用程序，它可以包含多个虚拟机、 Nic、 IP 地址、 负载平衡器、 子网和网络安全组的相关的资源。 例如，您可以管理所有的资源作为一个单一的管理单元的应用程序。 您可以创建、 更新和删除所有这些放在一起。 这是一个资源组中部署的示例应用程序。

![](./media/virtual-machines-azure-resource-manager-architecture/arm_arch3.png)

此应用程序包括︰

- 使用相同的存储帐户的两个虚拟机都在同一可用性组，并在同一子网上的虚拟的网络上。
- 每个虚拟机一个 NIC 和虚拟机的 IP 地址。
- 分配到两台虚拟机的 Nic 的互联网流量外部负载平衡器。

所有这些资源本应用程序的管理通过一个资源组包含它们。

您还可以查看组成并创建基于资源管理器的虚拟机使用 Azure PowerShell 或 Azure CLI 时的资源之间的依赖关系。 运行创建虚拟机的命令之前，必须创建资源组、 存储帐户、 虚拟的网络子网和 IP 地址与网卡。 有关详细信息，请参阅[创建预配置 Windows 资源管理器和 Azure PowerShell 的虚拟机和](virtual-machines-ps-create-preconfigure-windows-resource-manager-vms.md)。

## 下一步行动

[部署和管理虚拟机使用 Azure 资源管理器模板和 Azure CLI](virtual-machines-deploy-rmtemplates-azure-cli.md)

[部署和管理使用资源管理器模板和 PowerShell Azure 虚拟机](virtual-machines-deploy-rmtemplates-powershell.md)

## 其他资源

[Azure 计算、 网络和存储提供商在 Azure 资源管理器](virtual-machines-azurerm-versus-azuresm.md)

[Azure 的资源管理器概述](resource-group-overview.md)
