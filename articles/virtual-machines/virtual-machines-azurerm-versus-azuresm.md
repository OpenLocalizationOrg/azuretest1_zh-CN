---
ms.openlocfilehash: 5e2ff4391d8ab9047b7c0a353dc52f05476299fe
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="Azure 计算、 网络和存储提供商在 Azure 资源管理器"
   description="计算、 网络和存储资源提供商 （CRP、 NRP 和 SRP） 的概念性概述"
   services="virtual-machines"
   documentationCenter="dev-center-name"
   authors="mahthi"
   manager="timlt"
   editor=""
    tags="azure-resource-manager,azure-service-management"/>

<tags
   ms.service="virtual-machines"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="04/29/2015"
   ms.author="mahthi"/>

# Azure 计算、 网络和存储提供商在 Azure 的资源管理器

计算、 网络和存储功能使用 Azure 资源管理器包含从根本上将简化部署和管理复杂的应用程序运行在 IaaS 上。 许多应用程序需要的资源，包括虚拟网络、 存储帐户、 虚拟机和网络接口的组合。 Azure 资源管理器提供了构造一个 JSON 模板部署和管理所有这些资源放在一起作为单个应用程序的能力。

## 将集成计算、 网络和存储在 Azure 资源管理器的优点

Azure 资源管理器提供了能够方便地利用预构建的应用程序模板或构造应用程序模板部署和管理 Azure 的计算、 网络和存储资源。 在本节中，我们将逐步介绍的部署通过 Azure 资源管理器的资源优势。

-   复杂变得简单 — 生成、 集成和协作可共享模板文件从复杂的应用程序可以包括整个色域的 Azure 资源 （例如网站、 SQL 数据库、 虚拟机或虚拟网络）
-   灵活地使用相同的模板文件时，有开发、 devOps 和系统管理员可重复的部署
-   深度集成的 VM 扩展 （自定义脚本、 DSC、 厨师的简历、 傀儡，等等） 使用 Azure 资源管理器在模板文件中允许虚拟机中安装程序配置的简单业务流程
-   定义标记和计算、 网络和存储资源的这些标记的付费的传播
-   使用 Azure Role-Based 的访问控制 (RBAC) 的简单和精确的组织资源访问管理
-   通过修改原始模板，然后重新部署简化的升级/更新文章


## 计算、 网络和存储 Api 在 Azure 资源管理器的改进

除了上面提到的优点，还有发布的 Api 中的一些显著的性能改进。

-   启用大规模和并行虚拟机的部署
-   3 故障域中可用性设置的支持
-   改进的自定义脚本扩展，允许从任何可公开访问的自定义 URL 指定的脚本
- 虚拟机具有高度安全存储和专用的[FIPS 验证](http://wikipedia.org/wiki/FIPS_140-2)[硬件安全模块](http://wikipedia.org/wiki/Hardware_security_module)从秘密部署 Azure 密钥存储库集成
-   提供了通过 Api 使客户能够构建复杂的应用程序，包括网络接口、 负载平衡器和虚拟网络的网络连接的基本构造块
-   网络接口，一个新的对象允许复杂的网络配置，以将持续并重复使用的虚拟机
-   作为一类资源的负载平衡器使 IP 地址分配
-   精确的虚拟网络 Api 允许您简化对单个虚拟网络的管理

## 引入了新的 Api 的概念差异

在本节中，我们将引导完成最重要的一些 XML 的概念区别 Api 可用今天和基于 JSON Api 通过 Azure 资源管理器中可用的计算、 网络和存储。

 项目 | Azure 服务管理 （基于 XML）    | 计算、 网络和存储提供程序 （基于 JSON）
 ---|---|---
| 云服务的虚拟机 |  云服务时用于保存虚拟机所需的可用性从平台和负载平衡容器。 | 云服务不再是一个对象，该对象所需的创建虚拟机使用的新模型。 |
| 可用性设置 | 通过在虚拟机上配置相同的"AvailabilitySetName"指出了对该平台的可用性。 故障域的最大计数为 2。 | 可用性组是由 Microsoft.Compute 提供程序公开的资源。 必须列入要求高可用性的虚拟机的可用性设置。 现在，故障域的最大计数为 3。 |
| 好友小组 | 好友小组所需的创建虚拟网络。 但是，随着区域虚拟网的推出，，不是必需的了。 |为了简化，Azure 资源管理器通过公开的 Api 中不存在的好友小组概念。 |
| 负载平衡    | 云服务创建的虚拟机部署提供隐式负载平衡器。 | 负载平衡器是 Microsoft.Network 提供程序所公开的资源。 负载平衡器应该引用主网络接口的虚拟机需要进行负载平衡。 负载平衡器可以是内部或外部。 [阅读更多。](resource-groups-networking.md) |
|虚拟 IP 地址 | 当一个虚拟机添加到云服务，云服务将得到默认 VIP （虚拟 IP 地址）。 虚拟 IP 地址是相关联的隐式负载平衡器的地址。   | 公共 IP 地址是 Microsoft.Network 提供程序所公开的资源。 公共 IP 地址可以是静态 （保留） 或动态。 动态的公共 Ip 可以分配给一个负载平衡器。 可以使用安全组来保护公共 IPs。 |
|保留的 IP 地址|   您可以保留在 Azure 中的 IP 地址并将其与云服务以确保 IP 地址是粘滞。   | 可以在"静态"的模式中创建公用 IP 地址，并提供同样的功能作为"保留的 IP 地址"。 静态公共 IPs 只能分配到负载平衡器稍后再试。 |
|每个虚拟机的公用 IP 地址 (PIP) | 公共 IP 地址可以也直接关联到一个虚拟机中。 | 公共 IP 地址是 Microsoft.Network 提供程序所公开的资源。 公共 IP 地址可以是静态 （保留） 或动态。 但是，只有动态的公共 Ip 可以分配给网络接口，可立即获得每个虚拟机一个公用 IP。 |
|终结点| 要打开某些端口用于连接的虚拟机上配置所需的输入终结点。 连接到虚拟机通过设置输入终结点的常见模式之一。 | 可以实现相同的功能的启用特定的端口，用于连接到虚拟机上的终结点的负载平衡器上配置入站的 NAT 规则。 |
|DNS 名称| 云服务将被隐式全局唯一的 DNS 名称。 例如︰ `mycoffeeshop.cloudapp.net`。 | DNS 名称是可选参数，可以指定在公用 IP 地址资源上。 FQDN 将以下面的格式- `<domainlabel>.<region>.cloudapp.azure.com`。 |
|网络接口 | 主要和辅助网络接口并将其属性被定义为虚拟机的网络配置。 | 网络接口是 Microsoft.Network 提供程序所公开的资源。 不依赖于虚拟机网络接口的生命周期。 |

## 开始使用 Azure 模板的虚拟机

您可以开始使用 Azure 模板通过利用各种工具，我们再为开发和部署到的平台。

### Azure 门户

Azure 门户将继续选择要部署的虚拟机和虚拟机 （预览） 同时。 Azure 门户网站还允许自定义模板部署。

### Azure PowerShell

Azure PowerShell 将具有两种模式部署 – **AzureServiceManagement**模式和**AzureResourceManager**模式。  AzureResourceManager 模式现在还包含这些 cmdlet 来管理虚拟机、 虚拟网络和存储帐户。 您可以阅读更多有关它[这里](../powershell-azure-resource-manager.md)。

### Azure CLI

Azure 命令行界面 (Azure CLI) 将有两种模式部署 – **AzureServiceManagement**模式和**AzureResourceManager**模式。 AzureResourceManager 模式现在还包含用于管理虚拟机、 虚拟网络和存储帐户的命令。 您可以阅读更多有关它[这里](xplat-cli-azure-resource-manager.md)。

### Visual Studio

Visual Studio 最新 Azure SDK 发行版，可以创作和部署虚拟机和复杂的应用程序直接从 Visual Studio。 Visual Studio 提供了预构建的模板列表中部署或从空白模板开始。

### REST Api

您可以找到详细的 REST API 文档用于计算、 网络和存储资源提供商[这里](https://msdn.microsoft.com/library/azure/dn790568.aspx)。

## 常见问题

**是否可以创建使用新的 Azure 资源管理器存储帐户使用 Azure 服务管理 Api 创建虚拟网络中部署虚拟机？**

目前不支持这样做。 您无法使用新的 Azure 资源管理器 Api 来将虚拟机部署到使用服务管理 Api 创建了一个虚拟网络。

**是否可以创建使用新的 Azure 资源管理器 Api 使用 Azure 服务管理 Api 创建的用户图像从一个虚拟机？**

目前不支持这样做。 但是，可以将 VHD 文件复制，使用服务管理 Api 创建并将其复制到新帐户创建使用使用新的 Azure 资源管理器 Api 存储帐户。

**对我的订阅配额的影响是什么？**

虚拟机、 虚拟网络，并通过新的 Azure 资源管理器 Api 创建存储帐户的配额是独立于您当前拥有的配额。 每个订阅获取新的配额，以使用新的 Api 创建资源。 您可以阅读更多关于其他配额[以下](../azure-subscription-service-limits.md)。

**可以继续使用我的自动的脚本设置虚拟机、 虚拟网络，通过新的 Azure 资源管理器 Api 存储帐户等？**

所有的自动化和所生成的脚本将继续使用现有虚拟机、 在 Azure 服务管理模式下创建虚拟网络。 但是，脚本必须更新以使用新的架构来创建新的 Azure 资源管理器模式通过相同的资源。 阅读有关如何修改您的[Azure CLI 脚本](xplat-cli-azure-manage-vm-asm-arm.md)的详细信息。

**可以使用新的 Azure 资源管理器 Api 创建虚拟网络连接到我的快速通道电路？**

目前不支持这样做。 不能连接与高速路由电路中使用新的 Azure 资源管理器 Api 创建虚拟网络。 这将在以后支持。
 