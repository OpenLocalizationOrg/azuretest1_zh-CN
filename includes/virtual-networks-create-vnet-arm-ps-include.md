---
ms.openlocfilehash: 7c408a9bc31eadc75b6590b5da90738ed41ee94b
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
## 如何创建使用 PowerShell VNet

若要使用 PowerShell 创建 VNet，请按照下面的步骤。

1. 如果您以前从未使用 Azure PowerShell，请参阅[如何安装和配置 Azure PowerShell](powershell-install-configure.md)并按照说明操作，一直到结尾到 Azure 签名并选择您的订购。
2. 在 Azure PowerShell 提示符下，运行**开关 AzureMode**用于切换到资源管理器模式，如下所示。

        Switch-AzureMode AzureResourceManager
    
        WARNING: The Switch-AzureMode cmdlet is deprecated and will be removed in a future release.

    >[AZURE.WARNING] 开关 AzureMode cmdlet 将很快就被否决。 在这种情况下，所有的资源管理器 cmdlet 将被重命名。
    
3. 如果有必要，请运行**新建 AzureResourceGroup**用于创建新的资源组，如下所示。 我们的方案中，创建一个名为*TestRG*的资源组。 有关资源组的详细信息，请访问[Azure 资源管理器的概述](resource-group-overview.md/#resource-groups)。

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

4. 运行用于创建新的 VNet，**新建 AzureVirtualNetwork** ，如下所示。

        New-AzureVirtualNetwork -ResourceGroupName TestRG -Name TestVNet `
            -AddressPrefix 192.168.0.0/16 -Location centralus   
        
        Name              : TestVNet
        ResourceGroupName : TestRG
        Location          : centralus
        Id                : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet
        Etag              : W/"5b89894f-db7f-4634-9870-f9b97e209510"
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
        Subnets           : []

5. 运行**Get AzureVirtualNetwork** cmdlet 虚拟网络对象存储在变量中，如下所示。

        $vnet = Get-AzureVirtualNetwork -ResourceGroupName TestRG -Name TestVNet
    
    >[AZURE.TIP] 您可以通过运行组合步骤 4 和 5 **$vnet = New AzureVirtualNetwork-ResourceGroupName TestRG-名称 TestVNet AddressPrefix 192.168.0.0/16-位置 centralus**。

6. 运行**添加 AzureVirtualNetworkSubnetConfig** cmdlet 将子网添加到新的 VNet，，如下所示。

        Add-AzureVirtualNetworkSubnetConfig -Name FrontEnd `
            -VirtualNetwork $vnet -AddressPrefix 192.168.1.0/24
        
        Name              : TestVNet
        ResourceGroupName : TestRG
        Location          : centralus
        Id                : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet
        Etag              : W/"5b89894f-db7f-4634-9870-f9b97e209510"
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
                                "Etag": null,
                                "Id": null,
                                "AddressPrefix": "192.168.1.0/24",
                                "IpConfigurations": null,
                                "NetworkSecurityGroup": null,
                                "RouteTable": null,
                                "ProvisioningState": null
                              }
                            ]

7. 为要创建的每个子网重复上面的步骤 6。 下面的命令创建的*后端*子网对于我们的情况。

        Add-AzureVirtualNetworkSubnetConfig -Name BackEnd `
            -VirtualNetwork $vnet -AddressPrefix 192.168.2.0/24

8. 虽然创建子网，但他们目前仅存在于中用于检索您在上面的步骤 4 中创建的 VNet 的本地变量。 若要将更改保存到 Azure，运行**设置 AzureVirtualNetwork** cmdlet，如下所示。

        Set-AzureVirtualNetwork -VirtualNetwork $vnet   
        
        Name              : TestVNet
        ResourceGroupName : TestRG
        Location          : centralus
        Id                : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet
        Etag              : W/"2d3496d8-2b85-4238-bde2-377fe660aa4a"
        ProvisioningState : Succeeded
        Tags              :
        AddressSpace      : {
                              "AddressPrefixes": [
                                "192.168.0.0/16"
                              ]
                            }
        DhcpOptions       : {
                              "DnsServers": []
                            }
        NetworkInterfaces : null
        Subnets           : [
                              {
                                "Name": "FrontEnd",
                                "Etag": "W/\"2d3496d8-2b85-4238-bde2-377fe660aa4a\"",
                                "Id": "/subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd",
                                "AddressPrefix": "192.168.1.0/24",
                                "IpConfigurations": [],
                                "NetworkSecurityGroup": null,
                                "RouteTable": null,
                                "ProvisioningState": "Succeeded"
                              },
                              {
                                "Name": "BackEnd",
                                "Etag": "W/\"2d3496d8-2b85-4238-bde2-377fe660aa4a\"",
                                "Id": "/subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/BackEnd",
                                "AddressPrefix": "192.168.2.0/24",
                                "IpConfigurations": [],
                                "NetworkSecurityGroup": null,
                                "RouteTable": null,
                                "ProvisioningState": "Succeeded"
                              }
                            ]