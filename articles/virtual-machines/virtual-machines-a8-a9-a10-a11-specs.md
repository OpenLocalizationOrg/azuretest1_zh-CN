---
ms.openlocfilehash: 9f61c9de590fd4bf5b614651adb756808b2bd95d
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
 pageTitle="有关 A8、 A9、 A10 和 A11 实例 |Microsoft Azure"
 description="获得背景信息和使用 Azure A8、 A9、 A10 和 A11 计算密集型实例的注意事项。"
 services="virtual-machines, cloud-services"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""/>
<tags
ms.service="virtual-machines"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-multiple"
 ms.workload="infrastructure-services"
 ms.date="07/22/2015"
 ms.author="danlep"/>

# 有关 A8、 A9、 A10 和 A11 计算密集型实例

本文提供了有关使用 Azure A8、 A9、 A10 和 A11 实例，也称为*计算密集型*实例背景信息和注意事项。 这种情况的主要功能包括︰

* 设计并优化的计算密集型**高性能硬件**— 运行这些实例的 Azure 数据中心硬件和网络密集型应用程序，其中包括高性能计算 (HPC) 群集的应用程序、 建模和模拟。

* **RDMA MPI 应用程序的网络连接**– 使用必需的网络驱动程序配置时，A8 和 A9 实例可以与其他 A8 和通信 A9 实例通过远程直接内存访问 (RDMA) 技术为基础的 Azure 中的低延迟、 高吞吐量的网络。 此功能可以大大提高使用受支持的 Linux 或 Windows 消息传递接口 (MPI) 实现的应用程序的性能。

* **对于 Linux 和 Windows HPC 群集支持**— 部署群集管理和作业级排产 A8、 A9、 A10 和 A11 实例在 Azure 创建独立的 HPC 群集或将容量添加到内部群集上的软件。 像其他 Azure VM 大小，A8，A9、 A10 和 A11 实例支持标准或自定义 Windows 服务器和 Linux 操作系统的图像或在 Azure Vm (IaaS) 或 Azure 来宾操作系统 Azure 资源管理器模板在云服务中 (仅适用于 Windows 服务器 PaaS) 释放。

>[AZURE.NOTE]A10 和 A11 实例作为 A8 和 A9 实例具有相同的性能优化和规范。 但是，它们不包括访问 RDMA 网络在 Azure 中。 它们旨在 HPC 应用程序不需要常数和低延迟通信节点，也称为参数化或 embarrassingly 并行应用程序之间。 和运行非 MPI 应用程序的工作负载，A8 和 A9 实例无法访问 RDMA 网络和功能上等同于 A10 和 A11 实例。


## 规格

### CPU 和内存

Azure A8、 A9、 A10 和 A11 计算密集型实例功能高速、 多核 Cpu 和大量内存，如下表所示。

大小 | CPU | Memory
------------- | ----------- | ----------------
A8 和 A10 | 英特尔至强 E5-2670<br/>2.6 g h z @ 8 核 | DDR3 1600 MHz<br/>56 GB
A9 和 A11 | 英特尔至强 E5-2670<br/>2.6 g h z @ 16 个核心 | DDR3 1600 MHz<br/>112 GB


>[AZURE.NOTE]其他处理器的详细信息，包括受支持的指令集的扩展，位于 Intel.com 网站。 对于 VM 存储容量和磁盘的详细信息，请参阅[虚拟机大小](virtual-machines-size-specs.md)。

### 网络适配器

A8 和 A9 的实例具有两个网络适配器，它连接到以下两个后端 Azure 网络。


网络 | 说明
-------- | -----------
10 Gbps 以太网 | Azure 服务 （如 Azure 存储和 Azure 虚拟网） 以及互联网相连。
32 Gbps 后端，RDMA 支持 | 启用低延迟、 高吞吐量的应用程序实例在一个云服务或可用性组之间的通信。 保留以供 MPI 通信流。


>[AZURE.IMPORTANT]在 A8 和 IaaS 中运行 Linux 的 A9 Vm 上，RDMA 网络访问目前只能通过使用 Azure Linux RDMA 以及英特尔 MPI 库 5.0 SUSE Linux 企业服务器 12 (SLES 12) 的应用程序启用。 IaaS 或 PaaS 中运行 Windows Server 的 A8 和 A9 实例，目前只能通过使用 Microsoft 网络直接接口的应用程序启用 RDMA 网络访问权限。 在这篇文章的其他要求，请参阅[RDMA 网络访问](#access-the-rdma-network)。

A10 和 A11 实例具有单一的 10 Gbps 以太网网络适配器连接到 Azure 服务和互联网。

## Azure 订阅时的注意事项

* **Azure 帐户**– 如果您要部署多个少量的计算密集型实例，考虑使用付费订阅或其他购买选项。 您还可以使用 MSDN 订阅。 请参阅[Azure 为 MSDN 订阅者受益](http://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/)。 如果您正在使用[Azure 的免费试用版](http://azure.microsoft.com/pricing/free-trial/)，可以使用有限的数量的 Azure 计算内核。

* **内核配额**– 您可能需要增加 Azure 订购从默认的 20 内核，内核配额，这是不够的许多方案与 8 核或 16 核实例。 对于初始测试，则可以考虑申请的配额增加到 100 的核心。 若要执行此操作，打开免费支持票中[了解 Azure 限制，并提高了](http://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/)所示。

>[AZURE.NOTE]Azure 的配额是信用限制，不产能保证。 您收费仅为核心，使用。

* **相似性组**– 目前，相似性组不推荐大多数新的部署。 但是，请注意，是否您使用包含大小 A8 – A11 除外的实例相似性组，您将不能用于 A8 – A11 实例中，反之亦然。

* **虚拟网络**– Azure 虚拟网络不需要使用计算密集型实例。 但是，您可能需要至少基于云的 Azure 虚拟网络许多 IaaS 方案，或站点到站点连接，如果您需要访问内部资源，例如应用程序许可证服务器。 您需要在部署实例之前创建新 （区域） 的虚拟网络。 不支持将 A8、 A9、 A10 或 A11 VM 添加到组中关联的虚拟网络。 有关详细信息，请参阅[如何创建虚拟网络 (VNet)](../virtual-network/virtual-networks-create-vnet.md)和[配置站点到站点 VPN 连接使用的虚拟网络](../vpn-gateway/vpn-gateway-site-to-site-create.md)。

* **云服务或可用性组**– 通过 A8 和 A9 RDMA 网络连接实例必须部署在同一云服务 （为 IaaS 方案与基于 Linux 的 Vm 或基于 Windows Azure 服务管理中的虚拟机或与 Windows 服务器的 PaaS 方案） 或同一可用性设置 （对于基于 Linux 的 Vm 或基于 Windows 的 Vm Azure 资源管理器中）。

## 使用 HPC 包的注意事项

### HPC 包和 Linux 注意事项

[HPC 包](https://technet.microsoft.com/library/jj899572.aspx)是 Microsoft 的免费 HPC 群集和作业管理解决方案的窗口。 HPC 包 2012 R2 更新 2，HPC 包支持几个 Linux 发行版本上运行计算节点部署在 Azure Vm，开始由 Windows 服务器头节点。 与 HPC 包最新版本，您可以部署一个可以运行 MPI 应用程序访问 RDMA 网络在 Azure 中的基于 Linux 的群集。 有关详细信息，请参阅[HPC 包文档](http://go.microsoft.com/fwlink/?LinkId=617894)。

### HPC 包和窗口时的注意事项

HPC 包并不需要，您可以用 A8、 A9、 A10，A11 实例与 Windows 服务器上，但它是一种推荐的工具在 Azure 创建基于 Windows HPC 服务器的群集。 在 A8 和 A9 实例，HPC Pack 是最有效的方式来运行基于 Windows 的 MPI 应用程序访问在 Azure RDMA 网络。 HPC 包还包括用于 Microsoft 实现消息传递接口用于 Windows 的运行时环境。

有关更多信息和部署和在 Windows 服务器上的 HPC 包的 IaaS 和 PaaS 方案中使用的计算密集型实例的清单，请参阅[A8 和 A9 计算密集型实例︰ 快速入门与 HPC 包](https://msdn.microsoft.com/library/azure/dn594431.aspx)。

## RDMA 网络访问权限

### 从 Linux A8 和 A9 虚拟机访问

在一个云服务或可用性组，A8 和 A9 实例可以访问 Azure RDMA 网络运行 MPI 应用程序使用 RDMA Linux 驱动程序实例之间进行通信的。 到目前为止，Azure Linux RDMA 仅支持[英特尔 MPI 库 5.0](https://software.intel.com/en-us/intel-mpi-library/)。

>[AZURE.NOTE] 目前，Azure Linux RDMA 驱动程序不可用于通过驱动程序扩展的安装。 它们是可用只能通过从 Azure 市场使用 RDMA 启用 SLES 12 图像。

请参阅下表了解 Linux MPI 应用程序来访问 RDMA 网络 (IaaS) 的计算节点的群集中的先决条件。 请参阅[设置 Linux RDMA 群集运行 MPI 应用程序](virtual-machines-linux-cluster-rdma.md)的部署选项和配置步骤。

系统必备组件 | 虚拟机 (IaaS)
------------ | -------------
操作系统 | 从 Azure 市场 SLES 12 HPC 映像
MPI | 英特尔 MPI 库 5.0

### 从 Windows A8 和 A9 实例的访问权限

在一个云服务或可用性集中，A8 和 A9 实例可以访问 RDMA 网络在 Azure 中的运行 MPI 应用程序使用 Microsoft 网络直接接口实例之间进行通信。 A10 和 A11 实例不包含 RDMA 网络访问权限。

请参阅下表的 MPI 应用程序能够访问虚拟机 (IaaS) 中的 RDMA 网络和云服务 (PaaS) A8 或 A9 实例部署的先决条件。 典型的部署方案中，请参阅[A8 和 A9 计算密集型实例︰ 快速入门与 HPC 包](https://msdn.microsoft.com/library/azure/dn594431.aspx)。


系统必备组件 | 虚拟机 (IaaS) | 云服务 (PaaS)
---------- | ------------ | -------------
操作系统 | Windows Server 2012 R2 或 Windows Server 2012 | Windows Server 2012 R2、 Windows Server 2012 或 Windows Server 2008 R2 来宾操作系统家族
MPI | MS MPI 2012 R2 或更高版本，独立或通过 HPC 包 2012 R2 安装或更高版本<br/><br/>英特尔 MPI 库 5.0 | MS MPI 2012 R2 或更高版本，安装通过 HPC 包 2012 R2 或更高版本<br/><br/>英特尔 MPI 库 5.0


>[AZURE.NOTE]IaaS 的情况下， [HpcVmDrivers 扩展名](https://msdn.microsoft.com/library/azure/dn690126.aspx)必须添加到虚拟机安装 RDMA 连接所需的 Windows 驱动程序。


## 其他注意事项

* 仅在标准定价层 A8 – A11 VM 大小不可用。

* 不能调整到 A8、 A9、 A10 或 A11 大小现有 Azure VM。

* 当前不能使用云服务一部分的现有关系组已部署 A8、 A9、 A10 和 A11 实例。 同样，与云服务包含 A8、 A9、 A10 和 A11 实例相似性组不能用于其他实例大小的部署。 如果您尝试这些部署中，您将看到类似的错误消息 `Azure deployment failure (Compute.OverconstrainedAllocationRequest): The VM size (or combination of VM sizes) required by this deployment cannot be provisioned due to deployment request constraints.`

* RDMA 网络在 Azure 中的保留地址空间 172.16.0.0/12。 如果您计划在 A8 和 A9 Azure 的虚拟网络中所部署的实例运行 MPI 应用程序，请确保虚拟网络地址空间中不重叠 RDMA 网络。

## 下一步行动

* 有关可用性和价格 A8、 A9、 A10 和 A11 实例的详细信息，请参阅[虚拟机定价](http://azure.microsoft.com/pricing/details/virtual-machines/)和[云服务定价](http://azure.microsoft.com/pricing/details/cloud-services/)。
* 要部署和 A8 和 A9 实例访问 Azure RDMA 网络配置基于 Linux 的群集，请参阅[设置 Linux RDMA 群集运行 MPI 应用程序](virtual-machines-linux-cluster-rdma.md)。
* 若要开始部署和使用在 Windows HPC 包 A8 和 A9 实例，请参阅[A8 和 A9 计算密集型实例︰ 与 HPC 包快速入门](https://msdn.microsoft.com/library/azure/dn594431.aspx)和[A8 和 A9 实例上的运行 MPI 应用程序](https://msdn.microsoft.com/library/azure/dn592104.aspx)。
