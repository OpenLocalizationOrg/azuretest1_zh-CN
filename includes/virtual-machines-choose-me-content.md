---
ms.openlocfilehash: a1f567b301fbdd7be7066efe6485045ef0627fd0
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
| 计算选项    | 受众   |
| ------------------ | --------   |
| [应用程序服务]      | 可伸缩的 Web 应用程序、 移动应用程序、 API 的应用程序和任何设备的逻辑应用程序 |
| [云服务]   | 高度可用、 可伸缩的 n 层云应用程序的操作系统的更多控制 |
| [虚拟机] | 自定义的 Windows 和 Linux，完全控制操作系统的虚拟机 |

<a name="tellmevm"></a>
## 告诉我关于虚拟机

Azure 虚拟机允许您创建和使用在云环境中的虚拟机。 提供所谓的*基础结构即服务 (IaaS)*，可以在各种不同的方式使用虚拟机技术。 一些示例包括︰

- **虚拟机 (Vm)，用于开发和测试。** 开发组通常使用虚拟机，因为它们提供了一种快速、 方便的方法，创建计算机特定配置所需的代码和测试应用程序。 Azure 的虚拟机提供了简单、 最经济的方式来创建这些虚拟机，使用它们，然后在不再需要时删除它们。
- **在云中运行的应用程序。** 所以经济应该在公共云中运行一些应用程序。 例如，应用程序具有较大的峰值需求。 虽然可以使您自己的数据中心具有足够的硬件来处理峰值需求中，硬件可能未充分利用的大部分时间。 在 Azure 允许只有当需要它们并且关闭时不需要支付额外的虚拟机上运行此应用程序。 或者，假设您需要按需计算资源，速度快、 不承诺对启动。 再次，Azure 可以是正确的选择。
- **将您自己的数据中心扩展到公有云。** 当使用 Azure 虚拟网络时，您的组织可以创建虚拟网络 (VNET)，是您自己的内部网络的扩展，并将虚拟机添加到该 VNET。 这使得在 Azure VM 上运行应用程序，如[SharePoint](virtual-machines-sharepoint-infrastructure-services.md)、 [SQL Server](virtual-machines-sql-server-infrastructure-services.md)和其他人。 这种方法可能更容易部署或比您自己的数据中心都在虚拟机中运行它们。   
- **灾难恢复。** 而不是支付连续备份数据中心，已很少使用，基于 IaaS 的灾难恢复可让您支付您需要只有在真正需要时的计算资源。  例如，如果主数据中心出现故障，可以创建对 Azure 运行重要的应用程序上运行的虚拟机，然后在不再需要时关闭它们。

像其他虚拟机，Azure 中的 VM 操作系统、 存储和网络功能都有，可以运行各种应用程序。 可以使用 Azure 或其合作伙伴之一提供的图像，也可以使用自己。 示例包括各种版本、 版本和配置︰
 
-   Windows 服务器 
-   如 Suse Ubuntu，CentOS 的 Linux 服务器
-   SQL Server
-   BizTalk Server 
-   SharePoint 服务器

虚拟机使用虚拟硬盘 (Vhd) 来存储他们的操作系统 (OS) 和数据。 您可以从要安装操作系统的图像也使用 Vhd。 下图显示了这一点，以及两个用于创建和管理虚拟机的工具。

<a name="fig_createvms"></a>
![vm_diagram](./media/virtual-machines-choose-me-content/diagram.png)

**图︰ Azure 的虚拟机作为一种服务提供基础结构。**

虚拟机可以使用基于浏览器的入口，用于编写脚本，或直接通过 REST API 支持的命令行工具来管理。 如 RightScale 和 ScaleXtreme 的 Microsoft 合作伙伴还提供依赖于 REST API 的管理服务。 

与操作系统、 虚拟机与其他配置选项包括︰

- 确定您的磁盘数量等因素，可以将附加的大小和处理能力。 Azure 提供各种大小以支持多种类型的使用。 有关详细信息，请参阅[虚拟机大小](virtual-machines-size-specs.md)。  
- 其中将承载新 VM，Azure 地区如美国、 欧洲或亚洲。 
- VM 扩展，使您的虚拟机附加功能，如运行防病毒或使用 Windows PowerShell 的所需状态配置功能。

要考虑虚拟机的其他好处包括︰

**即付即用**-Azure 费用每小时价格基于 VM 的大小和操作系统。 部分小时，Azure 的费用仅为使用分钟。 存储是定价和单独收取。 有关详细信息，请参阅[虚拟机定价](http://azure.microsoft.com/pricing/details/virtual-machines/)。

**弹性**— Azure 监视承载每个正在运行的虚拟机的物理硬件。 运行虚拟机的物理服务器出现故障，Azure 注意到这一点，将 VM 移动到新硬件并重新启动虚拟机。 此过程有时称为康复服务。 Azure 也通过在 blob 存储保存 Vhd 的冗余拷贝保护虚拟机的数据。 



