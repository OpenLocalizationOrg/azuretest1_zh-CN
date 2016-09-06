---
ms.openlocfilehash: d068f4a20514489d5d1e9f0ca98e1efd58aae1ae
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="了解资源管理器和传统部署模型之间的差异"
   description="描述资源管理器部署模型和标准之间的差异 （或服务管理） 的部署模型。"
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="wpickett"
   editor=""/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/20/2015"
   ms.author="tomfitz"/>

# 了解资源管理器部署和传统部署

资源管理器部署模型提供了新的方法来部署和管理构成应用程序的服务。 这个新的模型包含重要的差异，从经典的部署模型中，并且两种模型不完全互相兼容。 若要简化部署和管理资源，Microsoft 建议您的新资源，可以使用资源管理器，如果可能，请重新部署现有资源通过资源管理器中。

作为服务管理模型，您可能也知道经典的部署模型。

本主题介绍两种模型，而另一些经典模型从过渡到资源管理器时可能遇到的问题之间的差异。 提供模型的概述，但不包括详述个别服务之间的区别。 有关转换计算、 存储和网络资源的更多详细信息，请参阅[Azure 计算、 网络和存储提供商在 Azure 资源管理器](./virtual-machines/virtual-machines-azurerm-versus-azuresm.md)。

许多资源不问题的经典模型和资源管理器中运行。 这些资源完全支持资源管理器，即使在经典模型中创建。 无需任何顾虑或额外的工作，可以转移到资源管理器中。

但是，少数资源提供商提供资源 （经典和为资源管理器中） 两个的版本由于模型之间的结构差异。 区分两种模型的资源提供程序包括︰

- 计算
- Storage
- 网络

对于这些资源类型，您必须知道哪一个版本的使用因为支持的操作会有所不同。 

两个模型之间的结构差异，请参阅[Azure 资源管理器体系结构](virtual-machines/virtual-machines-azure-resource-manager-architecture.md)。

## 资源管理器的特征

通过资源管理器中创建的资源都具有以下特征︰

- 通过以下方法之一创建︰

  - [预览的门户](https://portal.azure.com/)。

        ![preview portal](./media/resource-manager-deployment-model/preview-portal.png)

        For Compute, Storage, and Networking resources, you have the option of using either Resourece Manager or Classic deployment. Select **Resource Manager**.

        ![Resource Manager deployment](./media/resource-manager-deployment-model/select-resource-manager.png)

  - PowerShell 命令在**AzureResourceManager**模式下运行。

            PS C:\> Switch-AzureMode -Name AzureResourceManager

  - [REST API，azure 资源管理器](https://msdn.microsoft.com/library/azure/dn790568.aspx)的其他操作。
  - Azure 的 CLI 命令运行在**arm**模式。

            azure config mode arm

- 资源类型不包括**（经典）**的名称。 下面的图像显示为**存储帐户**的类型。

    ![web 应用程序](./media/resource-manager-deployment-model/resource-manager-type.png)

## 传统部署的特征

在经典的部署模型中创建的资源都具有以下特征︰

- 通过以下方法之一创建︰

  - [Azure 门户](https://manage.windowsazure.com)

        ![Azure portal](./media/resource-manager-deployment-model/azure-portal.png)

        Or, the preview portal and you specify **Classic** deployment (for Compute, Storage, and Networking).

        ![Classic deployment](./media/resource-manager-deployment-model/select-classic.png)

  - PowerShell 命令在**AzureServiceManagement**模式下运行 (这是默认模式，因此，如果不这样做尤其是切换到 AzureResourceManager，在 AzureServiceManagement 模式下运行)。

            PS C:\> Switch-AzureMode -Name AzureServiceManagement

  - [REST API，服务管理](https://msdn.microsoft.com/library/azure/ee460799.aspx)的其他操作。
  - 在**asm**或默认模式中运行 azure 的 CLI 命令。
- 资源类型包括在名称**（经典）** 。 下面的图像显示为**存储帐户 （经典）**的类型。

    ![经典的类型](./media/resource-manager-deployment-model/classic-type.png)

仍可以使用预览门户管理通过经典的部署创建的资源。

## 使用资源管理器和资源组的好处

资源管理器添加该资源组的概念。 通过资源管理器中创建的每个资源的资源组中存在。 资源管理器部署模型提供了以下几个优点︰

- 您可以部署、 管理和监视所有的服务为您的解决方案作为一个组，而不是单独处理这些服务。
- 重复可以部署整个应用程序生命周期的应用程序并可以的相信您的资源部署在一致的状态。
- 您可以使用声明性的模板来定义您的部署。 
- 您可以定义以便按正确的顺序来部署资源之间的依赖关系。
- 因为基于角色的访问控制 (RBAC) 本机集成管理平台，可以适用于所有服务在资源组中的访问控制。
- 可以将标记应用到逻辑方式组织您的订阅中的资源的所有资源。


在资源管理器中，通过经典的部署创建的每个资源在资源组中不存在。 当已添加资源管理器中时，所有资源对抗都添加到默认资源组。 如果您创建到现在经典部署资源，该资源即使未指定在部署该资源组自动创建空资源组中。 然而，就现有的资源组中并不意味着该资源已被转换为 Resourece 管理器模型。 如果该资源通过经典的部署创建的您必须继续通过经典的操作对其进行操作。 

您可以将资源移到不同的资源组，并将新资源添加到现有资源组。 因此，资源组可以包含通过资源管理器和经典的部署创建的资源的组合。 这种资源的组合可以创建意外的结果，因为资源不支持相同的操作。

通过使用声明性的模板，您可以简化您的部署的脚本。 而不是试图将从服务管理现有脚本转换为资源管理器时，请考虑重新使用您的部署策略，以利用在模板中定义您的基础架构和配置。

## 使用标记

标记使您可以用逻辑方式组织您的资源。 只有通过资源管理器支持标记创建的资源。 不能将标记应用于经典的资源。

有关使用资源管理器中的标记的详细信息，请参阅[使用标签来组织您的 Azure 资源](resource-group-using-tags.md)。

## 支持的操作的部署模型

在经典的部署模型中创建的资源不支持资源管理器的操作。 在某些情况下，资源管理器命令可以检索有关典型部署中，通过创建一个资源的信息或可以执行管理任务，如将传统的资源移动到另一个资源组，但这种情况下不应该将该类型支持资源管理器操作的印象。 例如，假设您有包含虚拟机使用资源管理器和经典创建的资源组。 如果运行下面的 PowerShell 命令时，您将看到所有虚拟机︰

    PS C:\> Get-AzureResourceGroup -Name ExampleGroup
    ...
    Resources :
     Name                 Type                                          Location
     ================     ============================================  ========
     ExampleClassicVM     Microsoft.ClassicCompute/domainNames          eastus
     ExampleClassicVM     Microsoft.ClassicCompute/virtualMachines      eastus
     ExampleResourceVM    Microsoft.Compute/virtualMachines             eastus
    ...

但是，如果运行 Get AzureVM 命令时，才会使用资源管理器中创建的虚拟机。

    PS C:\> Get-AzureVM -ResourceGroupName ExampleGroup
    ...
    Id       : /subscriptions/xxxx/resourceGroups/ExampleGroup/providers/Microsoft.Compute/virtualMachines/ExampleResourceVM
    Name     : ExampleResourceVM
    ...

一般情况下，您不应期望通过经典的部署要使用资源管理器命令创建的资源。

在使用通过资源管理器中创建的资源，应使用资源管理器的操作并不切换到服务管理操作。

## 虚拟机的注意事项

虚拟机工作时，有一些重要的注意事项。

- 与传统的部署模型部署的虚拟机不能包含在虚拟网络中部署的资源管理器中。
- 必须在虚拟网络中包括使用资源管理器部署模型部署的虚拟机。
- 与经典的部署模型部署的虚拟机无需包含在虚拟的网络。

从经典的部署代码转换到资源管理器时的等效 Azure CLI 命令的列表，请参见[等效资源管理器和服务管理的 VM 操作的命令](./virtual-machines/xplat-cli-azure-manage-vm-asm-arm.md)。

有关转换计算、 存储和网络资源的更多详细信息，请参阅[Azure 计算、 网络和存储提供商在 Azure 资源管理器](./virtual-machines/virtual-machines-azurerm-versus-azuresm.md)。

若要了解有关连接不同的部署模型中的虚拟网络，请参阅[连接到新的 VNets 的经典 VNets](./virtual-network/virtual-networks-arm-asm-s2s.md)。

## 下一步行动

- 若要了解有关创建声明性部署模板，请参阅[创作 Azure 资源管理器模板](resource-group-authoring-templates.md)。
- 若要查看模板的部署命令，请参阅[部署了 Azure 资源管理器模板的应用程序](resource-group-template-deploy.md)。


测试
