---
ms.openlocfilehash: 5e5609eb904e48599462bd7c952d1d9ee1017d52
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="使用自定义脚本扩展 Azure 的资源管理器模板"
   description="利用 ARM 模板使用自定义脚本的 Azure 虚拟机配置任务自动化"
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
   ms.date="07/01/2015"
   ms.author="kundanap"/>

# 使用自定义脚本扩展使用 Azure 的资源管理器模板

这篇文章概括介绍编写 Azure 使用自定义脚本扩展名的资源管理器模板引导 Linux 或 Windows 虚拟机上的工作负载。

概述自定义脚本扩展请参阅文章<a href="https://azure.microsoft.com/en-us/documentation/articles/virtual-machines-extensions-customscript/" target="_blank">在此处</a>。

曾经自从，自定义脚本扩展已广泛使用在 Windows 和 Linux 虚拟机上配置工作负载。 随着 Azure 资源管理器模板，用户现在可以创建单个模板︰ 不仅提供虚拟机，而且还会在其上配置工作负载。

## Azure 的资源管理器模板概述。

Azure 的资源管理器模板允许您以声明方式指定的 Azure IaaS 基础架构在 Json 语言中通过定义资源之间的依赖关系。 Azure 资源管理器模板的详细概述，请参阅下面的文章︰

<a href="https://azure.microsoft.com/en-us/documentation/articles/resource-group-overview/" target="_blank">资源组概述</a>。
<br/>
<a href="https://azure.microsoft.com/en-us/documentation/articles/virtual-machines-deploy-rmtemplates-azure-cli/" target="_blank">部署模板使用 Azure CLI</a>。
<br/>
<a href="https://azure.microsoft.com/en-us/documentation/articles/virtual-machines-deploy-rmtemplates-powershell/" target="_blank">部署模板使用 Azure Powershell</a>。

### 运行自定义脚本扩展 Pre-Requistes

1. 安装最新的 Azure PowerShell Cmdlet 或 Azure CLI 从<a href="http://azure.microsoft.com/downloads" target="_blank">在此处</a>。
2. 如果将现有的虚拟机上运行脚本，请确保虚拟机代理启用在 VM 中，如果不按照这<a href="https://msdn.microsoft.com/library/azure/dn832621.aspx" target="_blank">文章</a>安装一个。
3. 上载要到 Azure 存储在虚拟机上运行的脚本。 这些脚本可以来自一个或多个存储容器。
4. 或者还可以到 Github 帐户上载脚本。
5. 该脚本应由扩展又启动输入脚本启动其他脚本的方式创作。

## 使用模板的自定义脚本扩展名的概述︰

与模板一起部署中，我们将使用相同版本的自定义脚本扩展名为 availale 的 Azure 服务管理 Api。 扩展支持相同的参数和方案，如将文件上载到 Azure 存储帐户或 Github 位置。 应指定的主要区别与模板一起使用时的原版本的扩展，而不是以 majorversion.* 格式指定版本。

 ## 在 Linux 虚拟机上的自定义脚本扩展名的模板代码段

在模板的资源部分中定义下面的扩展资源

      {
    "type": "Microsoft.Compute/virtualMachines/extensions",
    "name": "MyCustomScriptExtension",
    "apiVersion": "2015-05-01-preview",
    "location": "[parameters('location')]",
    "dependsOn": ["[concat('Microsoft.Compute/virtualMachines/',parameters('vmName'))]"],
    "properties":
    {
      "publisher": "Microsoft.OSTCExtensions",
      "type": "CustomScriptForLinux",
      "typeHandlerVersion": "1.2",
      "settings": {
      "fileUris": [ "https: //raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-on-ubuntu/mongo-install-ubuntu.sh                        ],
      "commandToExecute": "shmongo-install-ubuntu.sh"
      }
    }
    }

## 用于 Windows 虚拟机上的自定义脚本扩展的模板代码段

在模板的资源部分中定义下面的资源

       {
       "type": "Microsoft.Compute/virtualMachines/extensions",
       "name": "MyCustomScriptExtension",
       "apiVersion": "2015-05-01-preview",
       "location": "[parameters('location')]",
       "dependsOn": [
           "[concat('Microsoft.Compute/virtualMachines/',parameters('vmName'))]"
       ],
       "properties": {
           "publisher": "Microsoft.Compute",
           "type": "CustomScriptExtension",
           "typeHandlerVersion": "1.4",
           "settings": {
               "fileUris": [
               "http://Yourstorageaccount.blob.core.windows.net/customscriptfiles/start.ps1"
           ],
           "commandToExecute": "powershell.exe -ExecutionPolicy Unrestricted -File start.ps1"
         }
       }
     }

在上面的例子中，使用您自己的设置替换该文件的 URL 和文件名。

之后创作模板，您可以将它们部署使用 Azure CLI 或 Azure Powershell。

请参考以下示例以完整的示例在使用自定义脚本扩展虚拟机上配置的应用程序。

<a href="https://github.com/Azure/azure-quickstart-templates/blob/b1908e74259da56a92800cace97350af1f1fc32b/mongodb-on-ubuntu/azuredeploy.json/" target="_blank">在 Linux 虚拟机上的自定义脚本扩展名</a>。
</br>
<a href="https://github.com/Azure/azure-quickstart-templates/blob/b1908e74259da56a92800cace97350af1f1fc32b/201-list-storage-keys-windows-vm/azuredeploy.json/" target="_blank">在 Windows 虚拟机上的自定义脚本扩展名</a>。
