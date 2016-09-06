---
ms.openlocfilehash: 64731b5ebbb50555ff752ea6e66b78023bd568af
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="DataStax 在 Ubuntu 资源管理器模板"
    description="了解如何轻松地部署新的 DataStax 群集在 Ubuntu 使用 Azure PowerShell 或 Azure CLI 和资源管理器模板的虚拟机上"
    services="virtual-machines"
    documentationCenter=""
    authors="karthmut"
    manager="timlt"
    editor="tysonn"/>

<tags
    ms.service="virtual-machines"
    ms.workload="multiple"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="04/29/2015"
    ms.author="karthmut"/>

# 在资源管理器模板与 Ubuntu 的 DataStax

DataStax 是被公认为行业的领导者中开发和交付解决方案基于 Apache Cassandra™-的商业支持、 企业的分布式 NoSQL 数据库技术已广泛认同为敏捷，间断的和可预测可缩放到任意大小。 DataStax 提供的企业 (DSE) 和社区 (DSC) 的风格。 它还提供了内存中计算、 企业级安全、 快速且功能强大的集成的分析和企业搜索等功能。

现在除了什么已经可用在 Azure 的市场中，您就可以轻松还部署使用资源管理器模板部署到[Azure PowerShell](../powershell-install-configure.md)或[Azure CLI](../xplat-cli.md)的 Ubuntu 虚拟机上的一个新 DataStax 群集。

虽然其他拓扑可以轻松实现通过自定义模板，文中基于此模板的新部署的群集将具有拓扑下图中，所述︰

![群集体系结构](media/virtual-machines-datastax-template/cluster-architecture.png)

使用的参数，可以定义新的 Apache 卡桑德拉群集中将部署中的节点数。 DataStax 操作中心服务的实例将还部署在独立的虚拟机中相同的 VNET，使您能够监视群集和所有各个节点的状态、 添加或删除节点，并执行与该群集相关的所有管理任务。

部署完成后，您可以访问使用已配置的 DNS 地址 Datastax 操作中心 VM 实例。 OpsCenter VM 都有 SSH 端口启用 HTTPS 端口 8443 22。 DNS 地址运营中心将包括*dnsName*和输入参数，为*区域*的格式生成`{dnsName}.{region}.cloudapp.azure.com`。 如果您使用*dnsName*参数设置为"datastax"中的"美国西"地区创建部署可以访问 Datastax 操作中心 VM 部署在`https://datastax.westus.cloudapp.azure.com:8443`。

> [AZURE.NOTE] 在部署中使用的证书是自签名的证书将创建浏览器警告。 替换为您自己的 SSL 证书的证书的[Datastax](http://www.datastax.com/)网站上，您可以遵循的过程。

我们深入到 Azure 资源管理器和模板相关的详细信息将使用此部署之前，请确保您有 Azure PowerShell 或 Azure CLI 配置正确。

[AZURE.INCLUDE [arm-getting-setup-powershell](../../includes/arm-getting-setup-powershell.md)]

[AZURE.INCLUDE [xplat-getting-set-up-arm](../../includes/xplat-getting-set-up-arm.md)]

## 使用资源管理器模板创建一个基于 Datastax 的卡桑德拉群集

请按照以下步骤创建一个 Apache 卡桑德拉的群集，基于 DataStax，从 Github 模板库中使用资源管理器模板。 每个步骤将包括 Azure PowerShell 和 Azure CLI 的方向。

### 步骤 1-答︰ 下载使用 PowerShell 的模板文件

创建一个本地文件夹的 JSON 模板和其他相关文件 (例如，C:\Azure\Templates\DataStax)。

在您的本地文件夹的文件夹名称替换并运行以下命令︰

    $folderName="C:\Azure\Templates\DataStax"
    $webclient = New-Object System.Net.WebClient
    $url = "https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/datastax-on-ubuntu/azuredeploy.json"
    $filePath = $folderName + "\azuredeploy.json"
    $webclient.DownloadFile($url,$filePath)
    $url = "https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/datastax-on-ubuntu/azuredeploy-parameters.json"
    $filePath = $folderName + "\azuredeploy-parameters.json"
    $webclient.DownloadFile($url,$filePath)
    $url = "https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/datastax-on-ubuntu/dsenode.sh"
    $filePath = $folderName + "\dsenode.sh"
    $webclient.DownloadFile($url,$filePath)
    $url = "https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/datastax-on-ubuntu/ephemeral-nodes-resources.json"
    $filePath = $folderName + "\ephemeral-nodes-resources.json"
    $webclient.DownloadFile($url,$filePath)
    $url = "https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/datastax-on-ubuntu/metadata.json"
    $filePath = $folderName + "\metadata.json"
    $webclient.DownloadFile($url,$filePath)
    $url = "https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/datastax-on-ubuntu/opscenter-install-resources.json"
    $filePath = $folderName + "\opscenter-install-resources.json"
    $webclient.DownloadFile($url,$filePath)
    $url = "https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/datastax-on-ubuntu/opscenter-resources.json"
    $filePath = $folderName + "\opscenter-resources.json"
    $webclient.DownloadFile($url,$filePath)
    $url = "https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/datastax-on-ubuntu/opscenter.sh"
    $filePath = $folderName + "\opscenter.sh"
    $webclient.DownloadFile($url,$filePath)
    $url = "https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/datastax-on-ubuntu/shared-resources.json"
    $filePath = $folderName + "shared-resources.json"
    $webclient.DownloadFile($url,$filePath)

### 步骤 1-b︰ 下载使用 Azure CLI 的模板文件

复制整个模板存储库使用 git 中获取客户端的您的选择，例如︰

    git clone https://github.com/Azure/azure-quickstart-templates C:\Azure\Templates

当完成时，寻找在 C:\Azure\Templates 目录中的**datastax 在 ubuntu**文件夹。

### 步骤 2: （可选） 了解模板参数

部署一个基于 DataStax 的 Apache 卡桑德拉群集等非普通解决方案时，必须指定一组配置参数能够处理大量的所需的设置。 通过声明这些模板定义中的参数，就可以在部署过程中指定的值，通过外部文件或命令行。

在"参数"部分顶部的**azuredeploy.json**文件中，您可以找到所需的模板配置 DataStax 群集的参数集。 下面是此模板的 azuredeploy.json 文件中的参数部分的示例︰

    "parameters": {
        "region": {
            "type": "string",
            "defaultValue": "West US",
            "metadata": {
                "Description": "Location where resources will be provisioned"
            }
        },
        "storageAccountPrefix": {
            "type": "string",
            "defaultValue": "uniqueStorageAccountName",
            "metadata": {
                "Description": "Unique namespace for the Storage Account where the Virtual Machine's disks will be placed"
            }
        },
        "dnsName": {
            "type": "string",
            "metadata" : {
                "Description": "DNS subname for the opserations center public IP"
            }
        },
        "virtualNetworkName": {
            "type": "string",
            "defaultValue": "myvnet",
            "metadata": {
                "Description": "Name of the virtual network provisioned for the cluster"
            }
        },
        "adminUsername": {
            "type": "string",
            "metadata": {
                "Description": "Administrator user name used when provisioning virtual machines"
            }
        },
        "adminPassword": {
            "type": "securestring",
            "metadata": {
                "Description": "Administrator password used when provisioning virtual machines"
            }
        },
        "opsCenterAdminPassword": {
            "type": "securestring",
            "metadata": {
                "Description": "Sets the operations center admin user password"
            }
        },
        "clusterVmSize": {
            "type": "string",
            "defaultValue": "Standard_D3",
            "allowedValues": [
                "Standard_D1",
                "Standard_D2",
                "Standard_D3",
                "Standard_D4",
                "Standard_D11",
                "Standard_D12",
                "Standard_D13",
                "Standard_D14"
            ],
            "metadata": {
                "Description": "The size of the virtual machines used when provisioning cluster nodes"
            }
        },
        "clusterNodeCount": {
            "type": "int",
            "metadata": {
                "Description": "The number of nodes provisioned in the cluster"
            }
        },
        "clusterName": {
            "type": "string",
            "metadata": {
                "Description": "The name of the cluster provisioned"
            }
        }
    }

每个参数的详细信息，例如数据类型和允许的值。 这样可以在交互模式 （如 PowerShell 或 Azure CLI） 模板执行过程中所传递的参数的有效性以及自我发现用户界面可能会动态生成的分析所需参数的列表以及它们的说明。

### 步骤 3-答︰ 部署模板使用 PowerShell DataStax 群集

通过创建包含所有参数的运行时值的 JSON 文件准备用于部署的参数文件。 此文件将然后传递作为单个实体给部署命令。 如果不包括参数文件，PowerShell 将使用任何模板中指定的默认值，然后提示您填写其余的值。

下面是示例组**azuredeploy parameters.json**文件中的参数︰

    {
        "storageAccountPrefix": {
            "value": "scorianisa"
        },
        "dnsName": {
            "value": "scorianids"
        },
        "virtualNetworkName": {
            "value": "datastax"
        },
        "adminUsername": {
            "value": "scoriani"
        },
        "adminPassword": {
            "value": ""
        },
        "region": {
            "value": "West US"
        },
        "opsCenterAdminPassword": {
            "value": ""
        },
        "clusterVmSize": {
            "value": "Standard_D3"
        },
        "clusterNodeCount": {
            "value": 3
        },
        "clusterName": {
            "value": "cl1"
        }
    }

请填写 Azure 部署名称、 资源组的名称、 Azure 的位置和您已保存的 JSON 部署文件的文件夹。 然后运行以下命令︰

    $deployName="<deployment name>"
    $RGName="<resource group name>"
    $locName="<Azure location, such as West US>"
    $folderName="<folder name, such as C:\Azure\Templates\DataStax>"
    $templateFile= $folderName + "\azuredeploy.json"
    $templateParameterFile= $folderName + "\azuredeploy-parameters.json"

    New-AzureResourceGroup –Name $RGName –Location $locName

    New-AzureResourceGroupDeployment -Name $deployName -ResourceGroupName $RGName -TemplateParameterFile $templateParameterFile -TemplateFile $templateFile

**新建 AzureResourceGroupDeployment**命令运行时，这将 JSON 参数，从文件中提取参数值，并将启动相应地执行模板。 定义和使用多个参数文件，与您不同的环境 （如测试、 生产等） 将促进重用模板并简化复杂的多环境解决方案。

在部署时，请记住，需要使您的存储帐户参数提供的名称必须是唯一的并满足所有要求的 Azure 存储帐户 （小写字母和数字） 创建新的 Azure 存储帐户。

期间和部署之后，您可以检查期间进行资源调配，包括发生的任何错误的所有请求。  

要做到这一点，转到[Azure 门户](https://portal.azure.com)并执行下列操作︰

- 在左侧导航栏上单击"浏览"，向下滚动并单击"资源组"。  
- 单击刚才创建的资源组上后，它将显示"资源组"刀片。  
- 通过单击"事件"条形图"资源组"刀片式服务器的"监视"部分中，可以看到事件，您的部署︰
- 单击在单独的事件上可以再往下深入代表该模板所做的每个单个操作的详细信息

后测试，如果您需要删除此资源组，并且所有资源 （存储帐户、 虚拟机和虚拟网络），使用这条命令︰

    Remove-AzureResourceGroup –Name "<resource group name>" -Force

### 步骤 3-b︰ 部署模板使用 Azure CLI 使用 DataStax 群集

若要部署 Datastax 群集通过 Azure CLI，首先通过指定的名称和位置创建资源组︰

    azure group create dsc "West US"

将此资源组的名称、 JSON 模板文件的位置和参数文件的位置 （请参见上面的 PowerShell 部分了解详细信息） 到下面的命令︰

    azure group deployment create dsc -f .\azuredeploy.json -e .\azuredeploy-parameters.json

您可以检查状态的单个资源部署使用以下命令︰

    azure group deployment list dsc

## 漫游的 Datastax 模板的结构和文件组织

为了设计的可靠和可重复使用的资源管理器模板，需要其他思维组织像 DataStax 一样复杂解决方案的部署过程中所需的任务非常复杂而且相互关联的系列。 ARM**模板链接**，除了通过相关扩展脚本执行的**资源循环**利用，就可以实现几乎任何复杂的基于模板的部署与可重复使用的模块化方法。

此图描述了此部署从 GitHub 下载的所有文件之间的关系︰

![datastax 文件](media/virtual-machines-datastax-template/datastax-files.png)

本节可引导您通过 Datastax 群集的**azuredeploy.json**文件的结构。

### "参数"部分

**Azuredeploy.json**的"参数"部分指定此模板中使用的可修改参数。 前面提到的**azuredeploy parameters.json**文件用于模板执行期间将值传递给 azuredeploy.json 的"参数"部分。

### "变量"部分

"变量"部分指定可在此模板中使用的变量。 它包含多个将被设置为常数或在执行时的计算的值的字段 （JSON 数据类型或片段）。 下面是此 Datastax 模板的"变量"部分︰

    "variables": {
    "templateBaseUrl": "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/datastax-on-ubuntu/",
    "sharedTemplateUrl": "[concat(variables('templateBaseUrl'), 'shared-resources.json')]",
    "clusterNodesTemplateUrl": "[concat(variables('templateBaseUrl'), 'ephemeral-nodes-resources.json')]",
    "opsCenterTemplateUrl": "[concat(variables('templateBaseUrl'), 'opscenter-resources.json')]",
    "opsCenterInstallTemplateUrl": "[concat(variables('templateBaseUrl'), 'opscenter-install-resources.json')]",
    "opsCenterVmSize": "Standard_A1",
    "networkSettings": {
        "virtualNetworkName": "[parameters('virtualNetworkName')]",
        "addressPrefix": "10.0.0.0/16",
        "subnet": {
            "dse": {
                "name": "dse",
                "prefix": "10.0.0.0/24",
                "vnet": "[parameters('virtualNetworkName')]"
            }
        },
        "statics": {
            "clusterRange": {
                "base": "10.0.0.",
                "start": 5
            },
            "opsCenter": "10.0.0.240"
        }
    },
    "osSettings": {
        "imageReference": {
            "publisher": "Canonical",
            "offer": "UbuntuServer",
            "sku": "14.04.2-LTS",
            "version": "latest"
        },
        "scripts": [
            "[concat(variables('templateBaseUrl'), 'dsenode.sh')]",
            "[concat(variables('templateBaseUrl'), 'opscenter.sh')]",
            "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/shared_scripts/ubuntu/vm-disk-utils-0.1.sh"
        ]
    },
    "sharedStorageAccountName": "[concat(parameters('storageAccountPrefix'),'cmn')]",
    "nodeList": "[concat(variables('networkSettings').statics.clusterRange.base, variables('networkSettings').statics.clusterRange.start, '-', parameters('clusterNodeCount'))]"
    },

向下钻取到此示例中，您可以看到两种不同的方法。 在第一段中，"osSettings"变量是包含 4 个键 / 值对的嵌套的 JSON 元素︰

    "osSettings": {
          "imageReference": {
            "publisher": "Canonical",
            "offer": "UbuntuServer",
            "sku": "14.04.2-LTS",
            "version": "latest"
          },

     
此第二个片段中的"脚本"变量是一个 JSON 数组将在其中使用一个模板语言函数 （连接） 和另一个变量和字符串常量的值在运行时计算的每个元素︰

          "scripts": [
            "[concat(variables('templateBaseUrl'), 'dsenode.sh')]",
            "[concat(variables('templateBaseUrl'), 'opscenter.sh')]",
            "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/shared_scripts/ubuntu/vm-disk-utils-0.1.sh"
          ]

### "资源"部分

**"资源"**部分是在何处发生大多数操作。 在本节内仔细查看，您可以立即识别两种不同的情况下︰ 第一个是元素的类型定义`Microsoft.Resources/deployments`基本意思就是调用的嵌套在主内部署。 通过"templateLink"元素 （和相关的版本属性），就可以指定一个模板链接的文件，就会调用此片段中可以看到作为输入，通过一组参数︰

    {
          "name": "shared",
          "type": "Microsoft.Resources/deployments",
          "apiVersion": "2015-01-01",
          "properties": {
            "mode": "Incremental",
            "templateLink": {
              "uri": "[variables('sharedTemplateUrl')]",
              "contentVersion": "1.0.0.0"
            },
            "parameters": {
              "region": {
                "value": "[parameters('region')]"
              },
              "networkSettings": {
                "value": "[variables('networkSettings')]"
              },
              "storageAccountName": {
                "value": "[variables('sharedStorageAccountName')]"
              }
            }
          }
        },

从这第一个示例，很显然如何，将在此方案中的**azuredeploy.json**组织作为一种业务流程机制，调用大量的其他模板文件，每个负责所需的部署活动的一部分。

特别要说明的是，此部署将使用下面的链接的模板︰

-   **共享 resource.json**︰ 包含将整个部署共享的所有资源的定义。 例如，帐户用于存储虚拟机的操作系统磁盘和虚拟网络存储。
-   **opscenter resources.json**︰ 部署 OpsCenter VM 和所有相关的资源，包括网络接口和公用 IP 地址。
-   **opscenter-安装-resources.json**: OpsCenter VM 扩展 （linux 的自定义脚本） 将调用安装程序 OpsCenter 服务，在该虚拟机所需的特定 bash 脚本文件 (**opscenter.sh**) 部署。
-   **短暂的节点-resources.json**︰ 部署所有群集节点的虚拟机和连接的资源 (网卡，专用的 Ip，等等。)。 此模板还将部署 VM 扩展 （linux 的自定义脚本），并调用实际在每个节点上安装 Apache 卡桑德拉位 bash 脚本 (**dsenode.sh**)。

让我们深入到这最后一个模板的使用方式，因为它是一种从模板开发角度看最感兴趣。 一个重要的概念，突出显示了如何使用一个模板文件可以部署单个资源类型的多个副本，并为每个实例可以设置唯一值为所需的设置。 这一概念被称为**资源循环**。

从主**azuredeploy.json**文件中调用**resources.json-短暂的节点**时，一个名为**nodeCount**的参数作为参数列表中的一部分。 内子模板，nodeCount （部署在群集中的节点数），将使用在需要部署多个副本中下段, 的突出显示部分中每个资源的**"副本"**元素。 需要用部署资源的不同实例的唯一值的所有设置，可以使用**copyindex()**函数获取一个数值，指示在该特定资源循环创建的当前索引。 在下面的片段中，可以看到这一概念应用到正在为 Datastax 群集节点创建多个虚拟机︰

               {
                  "apiVersion": "2015-05-01-preview",
                  "type": "Microsoft.Compute/virtualMachines",
                  "name": "[concat(parameters('namespace'), 'vm', copyindex())]",
                  "location": "[parameters('region')]",
                  "copy": {
                    "name": "[concat(parameters('namespace'), 'vmLoop')]",
                    "count": "[parameters('nodeCount')]"
                  },
                  "dependsOn": [
                    "[concat('Microsoft.Network/networkInterfaces/', parameters('namespace'), 'nic', copyindex())]",
                    "[concat('Microsoft.Compute/availabilitySets/', parameters('namespace'), 'set')]",
                    "[concat('Microsoft.Storage/storageAccounts/', variables('storageAccountName'))]"
                  ],
                  "properties": {
                    "availabilitySet": {
                      "id": "[resourceId('Microsoft.Compute/availabilitySets', concat(parameters('namespace'), 'set'))]"
                    },
                    "hardwareProfile": {
                      "vmSize": "[parameters('vmSize')]"
                    },
                    "osProfile": {
                      "computername": "[concat(parameters('namespace'), 'vm', copyIndex())]",
                      "adminUsername": "[parameters('adminUsername')]",
                      "adminPassword": "[parameters('adminPassword')]"
                    },
                    "storageProfile": {
                      "imageReference": "[parameters('osSettings').imageReference]",
                      "osDisk": {
                        "name": "osdisk",
                        "vhd": {
                          "uri": "[concat('http://',variables('storageAccountName'),'.blob.core.windows.net/vhds/', variables('vmName'), copyindex(), '-osdisk.vhd')]"
                        },
                        "caching": "ReadWrite",
                        "createOption": "FromImage"
                      },
                      "dataDisks": [
                        {
                          "name": "datadisk1",
                          "diskSizeGB": 1023,
                          "lun": 0,
                          "vhd": {
                            "Uri": "[concat('http://', variables('storageAccountName'),'.blob.core.windows.net/','vhds/', variables('vmName'), copyindex(), 'DataDisk1.vhd')]"
                          },
                          "caching": "None",
                          "createOption": "Empty"
                        }
                      ]
                    },
                    "networkProfile": {
                      "networkInterfaces": [
                        {
                          "id": "[resourceId('Microsoft.Network/networkInterfaces',concat(parameters('namespace'), 'nic', copyindex()))]"
                        }
                      ]
                    }
                  }
                },

资源创建中的另一个重要概念是指定的依赖项和资源之间的 precedencies 的能力，正如您可以看到**取决于**JSON 数组中。 在此特定的模板，每个节点还必须附加 1 TB 的磁盘 （请参见"dataDisks"），可用于承载备份和快照的 Apache 卡桑德拉的实例。

连接的磁盘格式化为触发的**dsenode.sh**脚本文件的执行节点准备活动的一部分。 该脚本的第一行调用另一个脚本︰

    bash vm-disk-utils-0.1.sh

虚拟机磁盘 utils 0.1.sh azure 快速入门 tempates github repo 中， **shared_scripts\ubuntu**文件夹中的一部分，包含装载、 格式和条带化的磁盘非常有用的功能。 在 repo 所有模板中，可以使用这些函数。

另一个有趣的片段来探索是与 CustomScriptForLinux VM 扩展。 这些被安装作为单独类型的资源，每个群集节点 （和 OpsCenter 实例） 的依赖。 他们利用同一资源循环机制所描述的虚拟机︰

    {
    "type": "Microsoft.Compute/virtualMachines/extensions",
    "name": "[concat(parameters('namespace'), 'vm', copyindex(), '/installdsenode')]",
    "apiVersion": "2015-05-01-preview",
    "location": "[parameters('region')]",
    "copy": {
        "name": "[concat(parameters('namespace'), 'vmLoop')]",
        "count": "[parameters('nodeCount')]"
    },
    "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachines/', parameters('namespace'), 'vm', copyindex())]",
        "[concat('Microsoft.Network/networkInterfaces/', parameters('namespace'), 'nic', copyindex())]"
    ],
    "properties": {
        "publisher": "Microsoft.OSTCExtensions",
        "type": "CustomScriptForLinux",
        "typeHandlerVersion": "1.2",
        "settings": {
            "fileUris": "[parameters('osSettings').scripts]",
            "commandToExecute": "bash dsenode.sh"
        }
    }
    }

通过 familiarizing 自己与此部署中包含的其他文件，将能够了解所有详细信息和最佳实践来组织和协调复杂的部署策略，对于多节点的解决方案，通过任何技术，利用 Azure 资源管理器的模板。 尽管不是强制的建议的方法是通过下图突出显示部分模板文件结构︰

![datastax 模板结构](media/virtual-machines-datastax-template/datastax-template-structure.png)

从本质上讲，这种方法建议到︰

-   定义所有特定的部署活动，利用模板链接到调用 sub 模板执行的中心业务流程点作为核心模板文件
-   创建一个特定的模板文件，将部署在所有其他特定部署任务 （例如存储帐户、 vnet 配置等） 之间共享的所有资源。 这可以很大程度重用之间具有类似的要求，在公共基础设施方面的部署。
-   包括专色要求特定于给定资源的可选资源模板
-   对于相同的资源组中的成员 （节点在群集等） 创建特定模板以便部署具有唯一属性的多个实例，利用资源循环
-   部署任务 （如产品的安装、 配置等） 利用脚本部署扩展和创建脚本特定于每种技术的所有开机自检

有关详细信息，请参阅[Azure 资源管理器模板语言](../resource-group-authoring-templates.md)。
 
