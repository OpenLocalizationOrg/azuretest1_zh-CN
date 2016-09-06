---
ms.openlocfilehash: 48500db79a5653d5c66746f617b573363a07b796
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="创作与 Azure VM 扩展模板 |Microsoft Azure"
   description="了解更多有关创作具有扩展的模板"
   services="virtual-machines"
   documentationCenter=""
   authors="kundanap"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/01/2015"
   ms.author="kundanap"/>

# 创作与 VM 扩展的 Azure 资源管理器模板。

## Azure 的资源管理器模板概述。

Azure 的资源管理器模板允许您以声明方式指定的 Azure IaaS 基础架构在 Json 语言中通过定义资源之间的依赖关系。 Azure 资源管理器模板的详细概述，请参阅下面的文章︰

<a href="https://azure.microsoft.com/en-us/documentation/articles/resource-group-overview/" target="_blank">资源组概述</a>。
<br/>
<a href="https://azure.microsoft.com/en-us/documentation/articles/virtual-machines-deploy-rmtemplates-azure-cli/" target="_blank">部署模板使用 Azure CLI</a>。
<br/>
<a href="https://azure.microsoft.com/en-us/documentation/articles/virtual-machines-deploy-rmtemplates-powershell/" target="_blank">部署模板使用 Azure Powershell</a>。

## 对于 VM 扩展示例模板代码段。
部署扩展外观为以下模板代码段︰

      {
      "type": "Microsoft.Compute/virtualMachines/extensions",
      "name": "MyExtension",
      "apiVersion": "2015-05-01-preview",
      "location": "[parameters('location')]",
      "dependsOn": ["[concat('Microsoft.Compute/virtualMachines/',parameters('vmName'))]"],
      "properties":
      {
      "publisher": "Publisher Namespace",
      "type": "extension Name",
      "typeHandlerVersion": "extension version",
      "settings": {
      // Extension specific configuration goes in here.
      }
      }
      }

## 用于标识发布者、 类型和任何扩展名为 typeHandlerVersion。

Azure VM 扩展 microsoft 发布和受信任的发布者、 类型和 typeHandlerVersion 由唯一标识的第三方发布服务器和每个扩展。 这些就可以确定如下︰

从 Azure PowerShell，运行下面的 Azure PowerShell cmdlet:

      Get-AzureVMAvailableExtension

从运行下面的 Azure PowerShell cmdlet 的 Azure CLI:

      Azure VM Extension list

此 cmdlet 返回发布服务器名称、 扩展名称和版本如下︰

       Publisher                   : Microsoft.Azure.Extensions  
      ExtensionName               : DockerExtension
      Version                     : 1.0

这三个属性映射到"出版商"、"类型"和"typeHandlerVersion"分别在上面的模板代码段。
注意︰ 它始终建议使用最新的扩展版本，以获取最新的功能。

## 用于标识扩展配置参数的架构

与创作扩展模板的下一步是确定提供配置参数的格式。 每个扩展都支持它自己的一套参数。

若要查看 Windows 扩展的某些示例配置，请单击文档[窗口扩展示例](virtual-machines-extensions-configuration-samples-windows.md)。

要看一下 Linux 扩展的某些示例配置，请单击[Linux 扩展](virtual-machines-extensions-configuration-samples-linux.md)示例文档。

请参考以下内容能够完全完成模板与 VM 扩展虚拟机模板。

<a href="https://github.com/Azure/azure-quickstart-templates/blob/b1908e74259da56a92800cace97350af1f1fc32b/mongodb-on-ubuntu/azuredeploy.json/" target="_blank">在 Linux 虚拟机上的自定义脚本扩展名</a>。
</br>
<a href="https://github.com/Azure/azure-quickstart-templates/blob/b1908e74259da56a92800cace97350af1f1fc32b/201-list-storage-keys-windows-vm/azuredeploy.json/" target="_blank">在 Windows 虚拟机上的自定义脚本扩展名</a>。

之后创作模板，您可以将它们部署使用 Azure CLI 或 Azure Powershell。
