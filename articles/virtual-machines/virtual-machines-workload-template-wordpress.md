---
ms.openlocfilehash: 9d26ab7250b4e8c9d9b1fb05ba6e295f5746d50a
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="在 Azure 资源管理器模板在 Ubuntu 上的 WordPress 服务器部署"
    description="轻松部署一个 WordPress 服务器运行使用资源管理器模板和 Azure 预览门户，Azure PowerShell 或 Azure CLI 的 Ubuntu。"
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
    ms.date="06/29/2015"
    ms.author="davidmu"/>

# 在 Azure 资源管理器模板在 Ubuntu 上的 WordPress 服务器部署

使用本文中的说明部署在 Ubuntu 使用资源管理器模板上运行一个 WordPress 服务器。 该模板创建新的虚拟网络中的单个虚拟机。

![](./media/virtual-machines-workload-template-wordpress/one-server-wordpress.png)

您可以使用 Azure 预览门户，Azure PowerShell 或 Azure CLI 运行该模板。

## Azure 预览门户

若要部署此工作负载使用资源管理器模板和 Azure 预览门户网站，请单击[此处](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fwordpress-single-vm-ubuntu%2Fazuredeploy.json)。

![](./media/virtual-machines-workload-template-wordpress/azure-portal-template.png)

1.  为**模板**窗格中，单击**保存**。
2.  单击**参数**。 在**参数**窗格中，输入新值选择从允许的值，或接受默认值，然后单击**确定**。
3.  如果需要请单击**订阅**，然后选择正确的 Azure 订阅。
4.  单击**资源组**，然后选择现有资源组。 或者，单击**新建或**新建一个针对此工作负载。
5.  如果需要请单击**资源组的位置**，然后选择正确的 Azure 位置。
6.  如果需要单击**法律条款**来审查条款和协议使用的模板。
7.  单击**创建**。

具体取决于模板，可能需要生成负载的 Azure 一些时间。 完成后，您将有新的 WordPress 服务器在 Ubuntu 上运行现有或新资源组中。

## Azure PowerShell

在开始之前，请确保具有的 Azure PowerShell 安装正确的版本，您已登录，并且您切换到新的资源管理器模式。 有关详细信息，请单击[此处](virtual-machines-deploy-rmtemplates-powershell.md#setting-up-powershell-for-resource-manager-templates)。

请填写 Azure 部署名称、 新的资源组名称，并在下列一组命令的 Azure 数据中心位置。 删除引号，包括一切 < 和 > 字符。

    $deployName="<deployment name>"
    $RGName="<resource group name>"
    $locName="<Azure location, such as West US>"
    $templateURI="https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/wordpress-single-vm-ubuntu/azuredeploy.json"
    New-AzureResourceGroup -Name $RGName -Location $locName
    New-AzureResourceGroupDeployment -Name $deployName -ResourceGroupName $RGName -TemplateUri $templateURI

下面是一个示例。

    $deployName="TestDeployment"
    $RGName="TestRG"
    $locname="West US"
    $templateURI="https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/wordpress-single-vm-ubuntu/azuredeploy.json"
    New-AzureResourceGroup -Name $RGName -Location $locName
    New-AzureResourceGroupDeployment -Name $deployName -ResourceGroupName $RGName -TemplateUri $templateURI

接下来，在 Azure PowerShell 提示符下运行命令块。

运行**新建 AzureResourceGroupDeployment**命令时，您将会提示您提供一系列的参数的值。 当您指定的所有参数值时，**新建 AzureResourceGroupDeployment**将创建并配置虚拟机。

模板执行完成后，您现在拥有 WordPress 在 Ubuntu 上运行新的资源组中的服务器。

## Azure CLI

在开始之前，请确保具有的 Azure CLI 安装正确的版本，您已登录，并且您切换到新的资源管理器模式。 有关详细信息，请单击[此处](virtual-machines-deploy-rmtemplates-azure-cli.md#getting-ready)。

首先，创建新的资源组。 使用下面的命令并指定组名称以及要在其中部署的 Azure 数据中心位置。

    azure group create <group name> <location>

接下来，使用下面的命令，并指定新的资源组的名称和 Azure 部署的名称。

    azure group deployment create --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/wordpress-single-vm-ubuntu/azuredeploy.json <group name> <deployment name>

下面是一个示例。

    azure group create wordpress eastus2
    azure group deployment create --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/wordpress-single-vm-ubuntu/azuredeploy.json wordpress wpdevtest

**Azure 组部署创建**命令运行时，系统会提示您提供一系列的参数的值。 当您指定的所有参数值时，Azure 创建并配置虚拟机。

模板执行完成后，您现在拥有 WordPress 在 Ubuntu 上运行新的资源组中的服务器。

## 其他资源

[部署和管理虚拟机使用 Azure 资源管理器模板和 Azure PowerShell](virtual-machines-deploy-rmtemplates-powershell.md)

[Azure 计算、 网络和存储提供商在 Azure 资源管理器](virtual-machines-azurerm-versus-azuresm.md)

[Azure 的资源管理器概述](../resource-group-overview.md)

[部署和管理虚拟机使用 Azure 资源管理器模板和 Azure CLI](virtual-machines-deploy-rmtemplates-azure-cli.md)

[虚拟机文档](http://azure.microsoft.com/documentation/services/virtual-machines/)

[如何安装和配置 Azure PowerShell](../install-configure-powershell.md)
