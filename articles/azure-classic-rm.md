---
ms.openlocfilehash: 512f7cbd0e3ff80800026b2a719c15a4ac812371
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="资源管理器和服务管理 （经典） 部署模式 |Azure"
   description="学习资源管理器和传统部署模型之间的差异。"
   services="virtual-network"
   documentationCenter=""
   authors="telmosampaio"
   manager="carolz"
   editor=""
   tags="azure-resource-manager,azure-service-management"/>

<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/14/2015"
   ms.author="telmos"/>

# Azure 的部署模型

Azure 平台正在转换。  无论您是新手，Azure 还是一直使用它多年来，是一定要了解一些关键的变化在这一转变过程中我们要做对平台。

所有的 Azure 资源支持一个或两个以下的部署模型︰

- **资源管理器︰**这是 Azure 的资源的最新部署模型。 大多数较新的资源已经支持此部署模型中，所有资源都将最终。   
 
- **经典︰**今天的大多数现有的 Azure 资源支持此模型。 新资源添加到 Azure 将不支持此模型。

可以使用创建的服务建模它每个 Azure 资源详细信息的文档。

## 这是为什么重要的？ 

它的重要原因如下︰

- 在这两个模型，则使用 Azure 平台功能是不同的。  例如，使用资源管理器创建的资源部署模型 （或只是资源管理器） 可创建利用[Azure 资源管理器模板](resource-group-overview.md/#template-deployment)，而不能使用传统部署模型创建的资源。
- 对各个 Azure 资源功能和行为可以是不同的两种模型中，跨或仅存在于一个模型或其他。  例如，负载平衡使用传统部署模型创建的虚拟机之间的通讯是*隐式*的因为虚拟机属于 Azure 的云服务，并在云服务中的虚拟机中自动平衡负载。 使用资源管理器创建的虚拟机不是成员的云服务，并必须*明确*为通信执行负载平衡跨多个虚拟机创建的单独的 Azure 负载平衡器资源。  
- 如何创建、 配置和管理 Azure 资源是这两个模型之间的不同。
- 创建使用不同的部署模型的资源一定不能兼容使用一个部署模型创建的资源。 例如，Azure 使用一个部署模型创建的虚拟机只能连接到 Azure 创建使用相同的部署模型的虚拟网络。    

每个部署模型基础是每个资源的应用程序编程接口 (API)。  没有为资源管理器部署模型[资源管理器 API](https://msdn.microsoft.com/library/azure/dn948464.aspx)和[服务管理 API](https://msdn.microsoft.com/library/azure/ee460799.aspx) ，经典的部署模型。 开发人员可以编写代码来使用这些 Api*直接*进行交互。  

IT 专业人员使用但是，通常交互使用这些 Api*间接*通过图形化的门户 web 浏览器中通过使用 Azure PowerShell cmdlet 的 Windows 计算机上，或通过 Windows、 OS X 或 Linux 的计算机上使用 Azure 的命令行界面 (CLI)。 所有这三种由 IT 专业人员使用的间接方法直接与 Api 进行交互。 这意味着，当新的功能引入到 Azure 平台或资源，始终是通过 API 直接可用首先，API 可用后获得的新的资源和功能支持的间接方法。  

以下各节介绍如何 Azure 资源配置的三种间接方法通过使用不同的部署模型。

## 门户网站
Azure 具有两个门户网站︰

- ** [Azure 的门户网站](https://manage.windowsazure.com)︰**如果您一直一段使用 Azure，使用此门户。 它用于创建和配置旧的 Azure 资源支持的典型的部署模型。 不能使用它来创建或配置只支持资源管理器的资源。 
- ** [Azure 预览门户网站](http://azure.microsoft.com/overview/preview-portal/)︰**如果您使用较新的 Azure 资源，有可能使用此门户。 它可以用于创建和配置某些 Azure 的资源。 最终将能够创建并使用它来配置 Azure 的所有资源。 一些资源，支持这两种部署模型，可以使用此门户创建和配置使用这两种部署模型的资源。 

一些资源和功能可以只创建和配置中或另一个入口。 某些资源或功能 （尚） 不能创建或配置在任一门户中，并只能使用 PowerShell 和 / 或 CLI 配置。 Azure 的每个资源的文档详细说明了可以使用创建它的方法。 

## PowerShell
使用[PowerShell](powershell-install-configure.md)使用的命令行或脚本作者可以创建和配置 Windows 计算机从 Azure 资源。  各个 Azure 资源具有[资源管理器的 cmdlet](https://msdn.microsoft.com/library/azure/mt125356.aspx)和 / 或[服务管理 cmdlet](https://msdn.microsoft.com/library/azure/dn708504.aspx)。  一些资源和功能只能创建和 （或) 使用 PowerShell 或 CLI 配置。 使用资源管理器 PowerShell cmdlet 时根据该资源，您可能必须创建和配置 Azure 资源的两个选项︰

- **仅 PowerShell cmdlet:**您可以创建和配置每个单独为每个资源使用 cmdlet 的 Azure 资源。 从命令行或通过 PowerShell 脚本，您可以存储和版本中包含多个命令，您可以执行此操作。

- **使用 Azure 资源管理器模板 PowerShell cmdlet:**可以使用 PowerShell 创建 Azure 使用 Azure 资源管理器模板的资源。 模板可以保存和已进行版本管理。 获得详情，请阅读[了 Azure 资源管理器模板的应用程序部署](resource-group-template-deploy.md)文章。 几个[Azure 快速启动模板](http://azure.microsoft.com/documentation/templates/)存在可以下载和修改过的通用解决方案。

## CLI
您可以创建和配置从窗口、 OS X 或 Linux 计算机使用 CLI 的 Azure 资源。  阅读[安装 Azure CLI](xplat-cli-install.md)的文章选择的操作系统上安装 CLI。 PowerShell，如有不同，具体取决于您创建资源使用[资源管理器](xplat-cli-azure-resource-manager.md)或[经典 （点点）](virtual-machines-command-line-tools.md)部署模型必须使用的命令。

## 下一步行动

- 了解有关[资源管理器](/resource-group-overview.md)的详细信息。
- 了解如何[设计模板](/best-practices-resource-manager-design-templates/md)。
- 使用[最佳实践](best-practices-resource-manager-examples.md)检验
