---
ms.openlocfilehash: 1e474c353b8ddb0b6325dc834f5afb53cdd57c6c
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
## 通过使用 PowerShell 部署 ARM 模板

若要部署使用 PowerShell 的 ARM 模板，请按照下面的步骤。

1. 如果您以前从未使用 Azure PowerShell，请参阅[如何安装和配置 Azure PowerShell](powershell-install-configure.md)并按照说明操作，一直到结尾到 Azure 签名并选择您的订购。
2. 如下所示运行**开关 AzureMode** cmdlet 以切换到资源管理器模式。

    开关 AzureMode AzureResourceManager

    警告︰ 该开关 AzureMode cmdlet 被否决，将在未来的版本中被删除。

    >[AZURE.WARNING] 开关 AzureMode cmdlet 将很快就被否决。 在这种情况下，所有的资源管理器 cmdlet 将被重命名。

3. 如果有必要，请运行**新建 AzureResourceGroup**用于创建新的资源组。 下面的命令创建一个名为*TestRG*在*美国中部*的 azure 区域中的资源组。 有关资源组的详细信息，请访问[Azure 资源管理器的概述](resource-group-overview.md/#resource-groups)。

        New-AzureResourceGroup -Name TestRG -Location centralus
        
        ResourceGroupName : TestRG
        Location          : centralus
        ProvisioningState : Succeeded
        Tags              :
        Permissions       :
                            Actions  NotActions
                            =======  ==========
                            *
        ResourceId        : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG

4. 运行**新建 AzureResourceGroupDeployment** cmdlet 使用来部署新的 VNet 下载和修改上面的模板和参数文件。

        New-AzureResourceGroupDeployment -Name TestVNetDeployment -ResourceGroupName TestRG `
            -TemplateFile C:\ARM\azuredeploy.json -TemplateParameterFile C:\ARM\azuredeploy-parameters.json
        
        
        DeploymentName    : TestVNetDeployment
        ResourceGroupName : TestRG
        ProvisioningState : Succeeded
        Timestamp         : 8/14/2015 9:40:00 PM
        Mode              : Incremental
        TemplateLink      :
        Parameters        :
                            Name             Type                       Value
                            ===============  =========================  ==========
                            location         String                     Central US
                            vnetName         String                     TestVNet
                            addressPrefix    String                     192.168.0.0/16
                            subnet1Prefix    String                     192.168.1.0/24
                            subnet1Name      String                     FrontEnd
                            subnet2Prefix    String                     192.168.2.0/24
                            subnet2Name      String                     BackEnd
        
        Outputs           :

5. 运行**Get AzureVirtualNetwork** cmdlet 以查看新的 VNet，属性，如下所示。


        Get-AzureVirtualNetwork -ResourceGroupName TestRG -Name TestVNet
        
        
        Name              : TestVNet
        ResourceGroupName : TestRG
        Location          : centralus
        Id                : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet
        Etag              : W/"2ed52eec-8c92-471f-b43b-2914d69f3f04"
        ProvisioningState : Succeeded
        Tags              :
        AddressSpace      : {
                              "AddressPrefixes": [
                                "192.168.0.0/16"
                              ]
                            }
        DhcpOptions       : {
                              "DnsServers": null
                            }
        NetworkInterfaces : null
        Subnets           : [
                              {
                                "Name": "FrontEnd",
                                "Etag": "W/\"2ed52eec-8c92-471f-b43b-2914d69f3f04\"",
                                "Id": "/subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd",
                                "AddressPrefix": "192.168.1.0/24",
                                "IpConfigurations": [],
                                "NetworkSecurityGroup": null,
                                "RouteTable": null,
                                "ProvisioningState": "Succeeded"
                              },
                              {
                                "Name": "BackEnd",
                                "Etag": "W/\"2ed52eec-8c92-471f-b43b-2914d69f3f04\"",
                                "Id": "/subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/BackEnd",
                                "AddressPrefix": "192.168.2.0/24",
                                "IpConfigurations": [],
                                "NetworkSecurityGroup": null,
                                "RouteTable": null,
                                "ProvisioningState": "Succeeded"
                              }
                            ]