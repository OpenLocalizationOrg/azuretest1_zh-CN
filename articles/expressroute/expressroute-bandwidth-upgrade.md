---
ms.openlocfilehash: 51bc29541048402eb9e3721f10bb18a57a5879c3
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="升级 ExpressRoute 带宽动态 |Microsoft Azure"
   description="如何动态地增加而不需要停机 ExpressRoute 电路的带宽大小。 "
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

# 升级 ExpressRoute 电路带宽动态而不需要停机

您可以增大 ExpressRoute 电路不停机。 以下说明将帮助您更新使用 PowerShell ExpressRoute 电路的带宽。 请参阅限制和限制的详细信息的[ExpressRoute 的常见问题解答](expressroute-faqs.md)页面。 

##  配置系统必备组件

在开始配置之前，请验证您具有以下先决条件︰

- Azure 的订阅
- 最新版本的 Azure PowerShell
- 在操作中已配置且活动 ExpressRoute 电路


##  使用 PowerShell 的配置设置

Windows PowerShell 是功能强大的脚本编写环境，可用于控制和自动化的部署和管理您的工作负载在 Azure 中。 有关详细信息请参阅[MSDN](https://msdn.microsoft.com/library/windowsazure/jj156055.aspx)中的 PowerShell 文档。

1. **导入 ExpressRoute PowerShell 模块。**

        Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\Azure.psd1'
        Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\ExpressRoute\ExpressRoute.psd1'

2. **获得 ExpressRoute 电路的详细信息**

    您可以使用下面的 PowerShell Commandlet 您 ExpressRoute 电路的详细信息︰
        

        PS C:\> Get-AzureDedicatedCircuit
    
    此命令将返回所有已创建的订阅中的电路的列表。 可以使用以下命令来获取特定的 ExpressRoute 电路的详细信息，如果您有与您的服务密钥︰

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


3. **增加的 Microsoft 一侧的 ExpressRoute 电路的带宽**
    
    请检查您的提供程序支持的带宽选项的[ExpressRoute FAQ](expressroute-faqs.md)页面。 您可以选择任意大小大于您现有的电路。 一旦您决定您需要什么尺寸，可以使用下面的命令来调整您的电路。

        PS C:\> Set-AzureDedicatedCircuitProperties -ServiceKey ********************************* -Bandwidth 1000
        
        Bandwidth                        : 1000
        CircuitName                      : TestCircuit
        Location                         : Silicon Valley
        ServiceKey                       : *********************************
        ServiceProviderName              : equinix
        ServiceProviderProvisioningState : Provisioned
        Sku                              : Standard
        Status                           : Enabled

    您电路将有已调整 Microsoft 一侧。 请注意，我们将开始计费您更新的带宽选项，从这一点上。

4. **增加的服务提供程序端的 ExpressRoute 电路的带宽**

    联系您的连接提供商 (NSP / EXP) 并为他们提供了有关更新的带宽信息。 按照规定的服务提供商以完成任务的顺序更新过程。

 

测试
