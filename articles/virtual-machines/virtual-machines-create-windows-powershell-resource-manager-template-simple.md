---
ms.openlocfilehash: 85e551971fbaf87a4044c189736961f927c2c749
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="使用资源管理器模板和 PowerShell 创建 Windows 虚拟机"
    description="使用资源管理器模板和 Azure PowerShell 创建新的 Windows 虚拟机。"
    services="virtual-machines"
    documentationCenter=""
    authors="davidmu1"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/28/2015"
    ms.author="davidmu"/>

# 使用资源管理器模板和 PowerShell 创建 Windows 虚拟机

您可以轻松地创建新的基于 Windows Azure 的虚拟机 (VM) 资源管理器模板使用 Azure PowerShell。 该模板创建单个虚拟机中使用新的资源组中的单个子网的新虚拟网络运行 Windows。

![](./media/virtual-machines-create-windows-powershell-resource-manager-template-simple/windowsvm.png)

您深入研究之前，请确保具有 Azure 并且 PowerShell 配置并准备就绪。

[AZURE.INCLUDE [arm-getting-setup-powershell](../../includes/arm-getting-setup-powershell.md)]

## 创建 Windows 虚拟机

请按照以下步骤创建 Windows 虚拟机使用 Azure PowerShell Github 模板存储库中使用资源管理器模板。

填写 Azure 部署名称、 资源组名称和 Azure 数据中心位置，然后运行这些命令。

    $deployName="<deployment name>"
    $RGName="<resource group name>"
    $locName="<Azure location, such as West US>"
    $templateURI="https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-simple-windows-vm/azuredeploy.json"
    New-AzureResourceGroup –Name $RGName –Location $locName
    New-AzureResourceGroupDeployment -Name $deployName -ResourceGroupName $RGName -TemplateUri $templateURI

运行**新建 AzureResourceGroupDeployment**命令时，将提示您提供在"参数"部分的 JSON 文件参数的值。 当您指定的所有参数值时，命令创建资源组和虚拟机。

    $deployName="TestDeployment"
    $RGName="TestRG"
    $locname="West US"
    $templateURI="https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-simple-windows-vm/azuredeploy.json"
    New-AzureResourceGroup –Name $RGName –Location $locName
    New-AzureResourceGroupDeployment -Name $deployName -ResourceGroupName $RGName -TemplateUri $templateURI

您将看到类似下面这样︰

    cmdlet New-AzureResourceGroupDeployment at command pipeline position 1
    Supply values for the following parameters:
    (Type !? for Help.)
    newStorageAccountName: newsaacct
    adminUsername: WinAdmin1
    adminPassword: *********
    dnsNameForPublicIP: contoso
    VERBOSE: 10:56:59 AM - Template is valid.
    VERBOSE: 10:56:59 AM - Create template deployment 'TestDeployment'.
    VERBOSE: 10:57:08 AM - Resource Microsoft.Network/virtualNetworks 'MyVNET' provisioning status is succeeded
    VERBOSE: 10:57:11 AM - Resource Microsoft.Network/publicIPAddresses 'myPublicIP' provisioning status is running
    VERBOSE: 10:57:11 AM - Resource Microsoft.Storage/storageAccounts 'newsaacct' provisioning status is running
    VERBOSE: 10:57:38 AM - Resource Microsoft.Storage/storageAccounts 'newsaacct' provisioning status is succeeded
    VERBOSE: 10:57:40 AM - Resource Microsoft.Network/publicIPAddresses 'myPublicIP' provisioning status is succeeded
    VERBOSE: 10:57:45 AM - Resource Microsoft.Compute/virtualMachines 'MyWindowsVM' provisioning status is running
    VERBOSE: 10:57:45 AM - Resource Microsoft.Network/networkInterfaces 'myVMNic' provisioning status is succeeded
    VERBOSE: 11:01:59 AM - Resource Microsoft.Compute/virtualMachines 'MyWindowsVM' provisioning status is succeeded


    DeploymentName    : TestDeployment
    ResourceGroupName : TestRG
    ProvisioningState : Succeeded
    Timestamp         : 4/28/2015 6:02:13 PM
    Mode              : Incremental
    TemplateLink      :
    Parameters        :
                        Name             Type                       Value
                        ===============  =========================  ==========
                        newStorageAccountName  String                     newsaacct
                        adminUsername    String                     WinAdmin1
                        adminPassword    SecureString
                        dnsNameForPublicIP  String                     contoso
                        windowsOSVersion  String                     2012-R2-Datacenter

    Outputs           :

现在有新的资源组中名为 MyWindowsVM 的新 Windows 虚拟机。

## 其他资源

[Azure 计算、 网络和存储提供商在 Azure 资源管理器](virtual-machines-azurerm-versus-azuresm.md)

[Azure 的资源管理器概述](resource-group-overview.md)

[使用 Azure 资源管理器和 PowerShell 创建 Windows 虚拟机](virtual-machines-create-windows-powershell-resource-manager.md)

[使用 PowerShell 和 Azure 服务管理器创建 Windows 虚拟机](virtual-machines-create-windows-powershell-service-manager.md)

[虚拟机文档](http://azure.microsoft.com/documentation/services/virtual-machines/)

[如何安装和配置 Azure PowerShell](install-configure-powershell.md)
