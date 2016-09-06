---
ms.openlocfilehash: 342b4d086382be7d74bcfb3aa8e79c250472a80b
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="使用 Azure 资源管理器模板部署 SQL Server AlwaysOn |Microsoft Azure"
    description="轻松部署五个服务器资源管理器模板和 Azure 预览门户，Azure PowerShell 或 Azure CLI 支持 SQL Server AlwaysOn。"
    services="virtual-machines"
    documentationCenter=""
    authors="davidmu1"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="06/29/2015"
    ms.author="davidmu"/>

# 将 SQL Server AlwaysOn 部署使用 Azure 资源管理器模板

使用本文中的说明部署 SQL Server AlwaysOn 使用 Azure 资源管理器模板。 该模板创建新的虚拟网络上两个不同的子网中的 5 个虚拟机。

![](./media/virtual-machines-workload-template-sql-alwayson/five-server-sqlao.png)

您可以使用 Azure 预览门户，Azure PowerShell 或 Azure CLI 运行该模板。

## Azure 预览门户

若要部署此工作负载使用 Azure 资源管理器模板和 Azure 预览门户网站，请单击[此处](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsql-server-2014-alwayson-dsc%2Fazuredeploy.json)。

![](./media/virtual-machines-workload-template-sql-alwayson/azure-portal-template.png)

1.  为**模板**窗格中，单击**保存**。
2.  单击**参数**。 在**参数**窗格中，输入新值选择从允许的值，或接受默认值，然后单击**确定**。
3.  如果需要请单击**订阅**，然后选择正确的 Azure 订阅。
4.  单击**资源组**，然后选择现有资源组。 或者，单击**新建或**新建一个针对此工作负载。
5.  如果需要请单击**资源组的位置**，然后选择正确的 Azure 位置。
6.  如果需要单击**法律条款**来审查条款和协议使用的模板。
7.  单击**创建**。

具体取决于模板，可能需要生成负载的 Azure 一些时间。 模板执行完成后，您现有的或新的资源组中有新的五个服务器 SQL Server AlwaysOn 配置。

## Azure PowerShell

在开始之前，请确保具有的 Azure PowerShell 安装正确的版本，您已登录，并且您切换到新的资源管理器模式。 有关详细信息，请单击[此处](virtual-machines-deploy-rmtemplates-powershell.md#setting-up-powershell-for-resource-manager-templates)。

请填写 Azure 部署名称、 新的资源组名称，并在下列一组命令的 Azure 数据中心位置。 删除引号，包括一切 < 和 > 字符。

    $deployName="<deployment name>"
    $RGName="<resource group name>"
    $locName="<Azure location, such as West US>"
    $templateURI="https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/sql-server-2014-alwayson-dsc/azuredeploy.json"
    New-AzureResourceGroup -Name $RGName -Location $locName
    New-AzureResourceGroupDeployment -Name $deployName -ResourceGroupName $RGName -TemplateUri $templateURI

下面是一个示例。

    $deployName="TestDeployment"
    $RGName="TestRG"
    $locname="West US"
    $templateURI="https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/sql-server-2014-alwayson-dsc/azuredeploy.json"
    New-AzureResourceGroup -Name $RGName -Location $locName
    New-AzureResourceGroupDeployment -Name $deployName -ResourceGroupName $RGName -TemplateUri $templateURI

接下来，在 Azure PowerShell 提示符下运行命令块。

运行**新建 AzureResourceGroupDeployment**命令时，您将会提示您提供一系列的参数的值。 当您指定的所有参数值时，**新建 AzureResourceGroupDeployment**将创建并配置虚拟机。

模板执行完成后，新的资源组中有新的五个服务器 SQL Server AlwaysOn 配置。

## Azure CLI

在开始之前，请确保具有的 Azure CLI 安装正确的版本，您已登录，并且您切换到新的资源管理器模式。 有关详细信息，请单击[此处](virtual-machines-deploy-rmtemplates-azure-cli.md#getting-ready)。

首先，创建新的资源组。 使用下面的命令并指定组名称以及要在其中部署的 Azure 数据中心位置。

    azure group create <group name> <location>

接下来，使用下面的命令，并指定新的资源组的名称和 Azure 部署的名称。

    azure group deployment create --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/sql-server-2014-alwayson-dsc/azuredeploy.json <group name> <deployment name>

下面是一个示例。

    azure group create sqlao eastus2
    azure group deployment create --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/sql-server-2014-alwayson-dsc/azuredeploy.json sqlao sqldevtest

**Azure 组部署创建**命令运行时，系统会提示您提供一系列的参数的值。 当您指定的所有参数值时，Azure 创建并配置虚拟机。

模板执行完成后，新的资源组中有新的五个服务器 SQL Server AlwaysOn 配置。


## 其他资源

[部署和管理虚拟机使用 Azure 资源管理器模板和 Azure PowerShell](virtual-machines-deploy-rmtemplates-powershell.md)

[Azure 计算、 网络和存储提供商在 Azure 资源管理器](virtual-machines-azurerm-versus-azuresm.md)

[Azure 的资源管理器概述](../resource-group-overview.md)

[部署和管理虚拟机使用 Azure 资源管理器模板和 Azure CLI](virtual-machines-deploy-rmtemplates-azure-cli.md)

[虚拟机文档](http://azure.microsoft.com/documentation/services/virtual-machines/)

[如何安装和配置 Azure PowerShell](../install-configure-powershell.md)
