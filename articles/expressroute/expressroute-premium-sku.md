---
ms.openlocfilehash: 43e327ce311cc4468ed667c286b4277116f09888
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="如何启用或禁用加载项 ExpressRoute 特优 |Microsoft Azure"
   description="如何启用或禁用 ExpressRoute 电路的 ExpressRoute 高级加载项。 ExpressRoute 高级，可以将达 10000 个用于公钥和私钥对等和最多 10 个虚拟网络的路由添加到 ExpressRoute 电路。 您还可以链接到 ExpressRoute 电路在另一个的在一个区域中的虚拟网络。"
   services="expressroute"
   documentationCenter="na"
   authors="cherylmc"
   manager="carolz"
   editor="" />
<tags 
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/25/2015"
   ms.author="cherylmc" />

# 配置为 ExpressRoute 电路的 ExpressRoute 高级加载项

ExpressRoute 高级是下面列出的功能的集合︰

 - 提高公共对等和专用的 10000 路由到路由表限制从 4000 路由。
 - 增加的虚拟网络 (VNets) 可以连接到 ExpressRoute 电路数 （默认为 10）。 
 - 通过 Microsoft 核心网络的全球联网。 您现在能够将一个地缘政治地区与其他地区 ExpressRoute 电路 VNet 的链接。 **示例︰**您可以链接到在硅谷创建 ExpressRoute 电路在欧洲西部中创建 VNet。

请参阅 ExpressRoute 高级加载项的详细信息的[ExpressRoute 的常见问题解答](expressroute-faqs.md)页面。 请参阅成本的[定价详细信息](http://azure.microsoft.com/pricing/details/expressroute/)页面。

下面这些说明将帮助您执行下列操作︰

- 与已启用的高级加载项创建 ExpressRoute 电路。
- 更新现有的 ExpressRoute 电路，以启用高级加载项。
- 禁用电路的 ExpressRoute 高级加载项。


## 启用高级加载项功能创建 ExpressRoute 电路

###  在开始之前

在开始配置之前，请验证您具有以下先决条件︰

- Azure 的订阅
- 最新版本的 Azure PowerShell

###  1.为导入 PowerShell 模块 ExpressRoute

Windows PowerShell 是功能强大的脚本编写环境，可用于控制和自动化的部署和管理您的工作负载在 Azure 中。 有关详细信息，请参阅[MSDN](https://msdn.microsoft.com/library/windowsazure/jj156055.aspx)中的 PowerShell 文档。

使用下面的 cmdlet 导入 ExpressRoute PowerShell 模块︰


        Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\Azure.psd1'
        Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\ExpressRoute\ExpressRoute.psd1'


### 2.使用启用的高级加载项功能配置新的 ExpressRoute 电路

使用在创建时启用的高级加载项，您可以创建新的 ExpressRoute 电路。 按照说明如何创建使用[Nsp](expressroute-configuring-nsps.md)或[EXPs](expressroute-configuring-exps.md)ExpressRoute 电路。 我们有一个新的可选参数中，您可以指定该 SKU 新建 AzureDedicatedCircuit cmdlet。 SKU 可以是标准或津贴。 默认值为标准。 作为高级 SKU 上传递将使高级附加功能的电路。


        New-AzureDedicatedCircuit -CircuitName $CircuitName -ServiceProviderName $ServiceProvider -Bandwidth $Bandwidth -Location $Location -Sku Premium


### 3.验证启用附件 ExpressRoute 津贴
您可以检查和 ExpressRoute 高级加载项是否已启用您的电路。
在下面的示例中，ExpressRoute 电路没有 ExpressRoute 高级加载项功能已启用。 SKU 将显示为***特优***如果启用附件。

        PS C:\> Get-AzureDedicatedCircuit -ServiceKey *********************************

        Bandwidth                        : 200
        CircuitName                      : TestCircuit
        Location                         : Silicon Valley
        ServiceKey                       : *********************************
        ServiceProviderName              : equinix
        ServiceProviderProvisioningState : Provisioned
        Sku                              : Standard
        Status                           : Enabled




## 若要启用现有的 ExpressRoute 电路的 ExpressRoute 高级加载项
您可以启用 ExpressRoute 高级加载项功能为您已创建了任何 ExpressRoute 电路。


1. **获取 ExpressRoute 电路的详细信息**

    您可以使用以下 PowerShell cmdlet 您 ExpressRoute 电路的详细信息︰
        

        PS C:\> Get-AzureDedicatedCircuit
    
    此命令将返回所有已创建的订阅中的电路的列表。 您可以使用以下命令来获取特定的 ExpressRoute 电路的详细信息，如果您有与您的服务密钥

         PS C:\> Get-AzureDedicatedCircuit -ServiceKey <skey>

    更换<skey>与实际服务的键。
    
        PS C:\> Get-AzureDedicatedCircuit -ServiceKey *********************************

        Bandwidth                        : 200
        CircuitName                      : TestCircuit
        Location                         : Silicon Valley
        ServiceKey                       : *********************************
        ServiceProviderName              : equinix
        ServiceProviderProvisioningState : Provisioned
        Sku                              : Standard
        Status                           : Enabled


2. **启用为电路 ExpressRoute 高级加载项**


    您可以使用以下 PowerShell cmdlet 您现有电路启用 ExpressRoute 高级加载项︰
    
        PS C:\> Set-AzureDedicatedCircuitProperties -ServiceKey "*********************************" -Sku Premium
        
        Bandwidth                        : 1000
        CircuitName                      : TestCircuit
        Location                         : Silicon Valley
        ServiceKey                       : *********************************
        ServiceProviderName              : equinix
        ServiceProviderProvisioningState : Provisioned
        Sku                              : Premium
        Status                           : Enabled

    您电路将具有 ExpressRoute 高级加载项功能都已启用。 请注意，我们将开始计费您特优附加功能就该命令已成功运行。


## 若要禁用 ExpressRoute 电路的 ExpressRoute 高级加载项

您可以禁用已启用的高级加载项 ExpressRoute 电路的 ExpressRoute 高级加载项。

**注意**︰ 您必须确保您已经停止使用这样做之前，高级加载项特征列表中列出的功能。 我们将失败，您的请求，如果您有大于 10 VNets 链接到您电路禁用高级加载项。 您还将看到，我们将我们的 BGP 会话是否您禁用高级加载项后公布大于 5000 路由给我们。

1. **获得 ExpressRoute 电路的详细信息**

    您可以使用以下 PowerShell cmdlet 您 ExpressRoute 电路的详细信息︰
        

        PS C:\> Get-AzureDedicatedCircuit
    
    此命令将返回所有已创建的订阅中的电路的列表。 您可以使用以下命令来获取特定的 ExpressRoute 电路的详细信息，如果您有与您的服务密钥

         PS C:\> Get-AzureDedicatedCircuit -ServiceKey <skey>

    更换<skey>与实际服务的键。
    
        PS C:\> Get-AzureDedicatedCircuit -ServiceKey *********************************

        Bandwidth                        : 200
        CircuitName                      : TestCircuit
        Location                         : Silicon Valley
        ServiceKey                       : *********************************
        ServiceProviderName              : equinix
        ServiceProviderProvisioningState : Provisioned
        Sku                              : Premium
        Status                           : Enabled


3. **禁用电路的 ExpressRoute 高级加载项**


    您可以使用以下 PowerShell cmdlet 您现有电路禁用 ExpressRoute 高级加载项︰
    
        PS C:\> Set-AzureDedicatedCircuitProperties -ServiceKey "*********************************" -Sku Standard
        
        Bandwidth                        : 1000
        CircuitName                      : TestCircuit
        Location                         : Silicon Valley
        ServiceKey                       : *********************************
        ServiceProviderName              : equinix
        ServiceProviderProvisioningState : Provisioned
        Sku                              : Premium
        Status                           : Standard

    现已为您电路禁用高级加载项。


 

测试
