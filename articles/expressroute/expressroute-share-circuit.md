---
ms.openlocfilehash: af1a42ff51939e9f20e6d0f28f3d10c5db1c78b8
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="跨多个订阅共享 ExpressRoute 电路 |Microsoft Azure"
   description="这篇文章将引导您完成跨多个 Azure 订阅共享 ExpressRoute 电路。"
   services="expressroute"
   documentationCenter="na"
   authors="cherylmc"
   manager="jdial"
   editor="tysonn" />
<tags 
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="07/20/2015"
   ms.author="cherylmc" />

# 跨多个订阅共享 ExpressRoute 电路

可以在多个订阅共享单个 ExpressRoute 电路。 **图 1**显示一个简单的跨多个订阅共享 ExpressRoute 电路工作原理示意图。

每个较小的群，大群中用来表示订阅属于组织内部的不同部门。 每个组织中的部门部署其服务可用于他们自己预订，但可以共享单个 ExpressRoute 专用电路连接到公司网络。 一个部门 (在此示例中︰ IT) 可以拥有 ExpressRoute 专用电路。 专用的电路连接和带宽费用将用在专线所有者。 在组织内的其他订阅可以使用 ExpressRoute 电路。

**图 1**

![预订共享](./media/expressroute-share-circuit/IC766124.png)

## 管理

*电路的所有者*是管理员/共同-administrator ExpressRoute 专用电路在其中创建的订阅。 电路所有者可以授权管理员/共同-administrators （称为工作流关系图中的*电路用户*） 的其他订阅使用他们所拥有的专用的电路。 被授权使用单位的专线电路用户专线向其订阅链接 VNet，一旦它们被授权。

电路所有者有权修改并随时撤销授权。 撤销授权将导致正在从其访问权限被吊销该订阅中删除所有链接。

## 工作流

1. 其他订阅的管理员可以使用指定的电路的电路所有者授予的权限。 在下面的示例中，电路 (Contoso IT) 管理员通过指定其 Microsoft ID，可将多达 2 VNETs 链接到电路的另一个订阅 （Contoso 销售额），管理员。 该 cmdlet 不发送电子邮件到指定的 Microsoft id。 需要明确通知其他预订所有者授权已完成电路所有者。

        PS C:\> New-AzureDedicatedCircuitLinkAuthorization -ServiceKey '6ed7e310-1a02-4261-915f-6ccfedc416f1' -Description 'SalesTeam' -Limit 2 -MicrosoftIds 'salesadmin@contoso.com'
        
        Description         : SalesTeam 
        Limit               : 2 
        LinkAuthorizationId : e2bc2645-6fd4-44a4-94f5-f2e43e6953ed 
        MicrosoftIds        : salesadmin@contoso.com 
        Used                : 0

1. 一旦通过电路所有者通知，授权订阅的管理员可以运行以下 cmdlet 以电路的检索服务。 在此示例中，销售 Contoso 管理员必须先登录使用 Microsoft 指定 salesadmin@contoso.com。

        PS C:\> Get-AzureAuthorizedDedicatedCircuit
        
        Bandwidth                        : 100
        CircuitName                      : ContosoIT
        Location                         : Washington DC
        MaximumAllowedLinks              : 2
        ServiceKey                       : 6ed7e310-1a02-4261-915f-6ccfedc416f1
        ServiceProviderName              : ###########
        ServiceProviderProvisioningState : Provisioned
        Status                           : Enabled
        UsedLinks                        : 0

1. 管理员的授权订阅运行以下 cmdlet 以完成链接操作。

        PS C:\> New-AzureDedicatedCircuitLink –servicekey 6ed7e310-1a02-4261-915f-6ccfedc416f1 –VnetName 'SalesVNET1' 
        
            State VnetName 
            ----- -------- 
            Provisioned SalesVNET1

就是这样。 在 Azure 上 Conto 的销售 VNet 现在已链接到创建拥有的电路，通过 ContosoIT。

## 管理授权

电路所有者可以与最多 10 个 Azure 订阅共享电路。 电路所有者可以查看已被授权到电路。 所有者可以撤消的授权，在任何时候。  当电路所有者撤消的授权，由 LinkAuthorizationId，所有链接都允许通过授权将被立即删除。 链接的 VNETs 将失去 ExpressRoute 电路通过内部网络连接。

    PS C:\> Get-AzureDedicatedCircuitLinkAuthorization -ServiceKey: 6ed7e310-1a02-4261-915f-6ccfedc416f1 
    
    Description         : EngineeringTeam 
    Limit               : 3 
    LinkAuthorizationId : cc958457-c8c1-4f16-af09-e7f099da64bf 
    MicrosoftIds        : engadmin@contoso.com 
    Used                : 1 
    
    Description         : MarketingTeam 
    Limit               : 1 
    LinkAuthorizationId : d972726f-c7b9-4658-8598-ad3208ac9348 
    MicrosoftIds        : marketingadmin@contoso.com 
    Used                : 0 
    
    Description         : SalesTeam 
    Limit               : 2 
    LinkAuthorizationId : e2bc2645-6fd4-44a4-94f5-f2e43e6953ed 
    MicrosoftIds        : salesadmin@contoso.com 
    Used                : 2 
    
    PS C:\> Remove-AzureDedicatedCircuitLinkAuthorization -ServiceKey '6ed7e310-1a02-4261-915f-6ccfedc416f1' -AuthorizationId 'e2bc2645-6fd4-44a4-94f5-f2e43e6953ed'


下图显示了授权工作流︰

![预订共享工作流](./media/expressroute-share-circuit/IC759525.png)

## 下一步行动

有关 ExpressRoute 的详细信息，请参阅[ExpressRoute 概述](expressroute-introduction.md)。


测试
