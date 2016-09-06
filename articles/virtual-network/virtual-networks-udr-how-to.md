---
ms.openlocfilehash: 4a449867f50aec23437a591f1d31b91568dde887
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="如何创建路由和启用 IP 转发 Azure 中"
   description="了解如何管理 UDRs 和 IP 转发"
   services="virtual-network"
   documentationCenter="na"
   authors="telmosampaio"
   manager="carolz"
   editor="tysonn" />
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/10/2015"
   ms.author="telmos" />

# 如何创建路由和启用 IP 转发 Azure 中
可以使用在 Azure 即用虚拟装置来处理在 Azure 虚拟网络中的通信量。 但是，您需要创建将允许虚拟机和云服务将数据包发送到虚拟设备，而不是为数据包所需的目标虚拟网络中的路由。 您还需要启用 IP 转发 VM，以便它可以接收和转发数据包，得不到解决到实际的虚拟设备的虚拟机的虚拟装置上。 

## 如何管理路由
您可以添加、 删除和更改使用 PowerShell Azure 中的路由。 创建工艺路线之前，您必须创建主机路由的路由表。

### 如何创建一个路由表
若要创建一个名为*FrontEndSubnetRouteTable*的路由表，运行下面的 PowerShell 命令︰

```powershell
New-AzureRouteTable -Name FrontEndSubnetRouteTable `
    -Location uscentral `
    -Label "Route table for frontend subnet"
```

上面的命令的输出应如下所示︰

    Error          :
    HttpStatusCode : OK
    Id             : 085ac8bf-26c3-9c4c-b3ae-ebe880108c70
    Status         : Succeeded
    StatusCode     : OK
    RequestId      : a8cc03ca42d39f27adeaa9c1986c14f7

### 如何将路由添加到路由表
要添加将*10.1.1.10*设置为下一个跃点上面创建的路由表中的*10.2.0.0/16*子网路由，请运行下面的 PowerShell 命令︰

```powershell
Get-AzureRouteTable FrontEndSubnetRouteTable `
  	|Set-AzureRoute -RouteName FirewallRoute -AddressPrefix 10.2.0.0/16 `
    -NextHopType VirtualAppliance `
    -NextHopIpAddress 10.1.1.10
```

上面的命令的输出应如下所示︰

    Name     : FrontEndSubnetRouteTable
    Location : Central US
    Label    : Route table for frontend subnet
    Routes   : 
               Name                 Address Prefix    Next hop type        Next hop IP address
               ----                 --------------    -------------        -------------------
               firewallroute        10.2.0.0/16       VirtualAppliance     10.1.1.10    

### 如何将关联到一个子网的路由
路由表必须与它要使用的一个或多个子网。 要关联到虚拟网络*ProductionVnet*中名为*FrontEndSubnet*的子网*FrontEndSubnetRouteTable*路由表，请运行下面的 PowerShell 命令︰

```powershell
Set-AzureSubnetRouteTable -VirtualNetworkName ProductionVnet `
    -SubnetName FrontEndSubnet `
    -RouteTableName FrontEndSubnetRouteTable
```

### 如何查看虚拟机中的应用的路由
您可以查询 Azure 若要查看适用于特定 VM 或角色实例的实际路由。 所示的路由包含 Azure 提供，以及广告的 VPN 网关的路由的默认路由。 显示路由的额度是 800。

要查看路由到主网卡上一个名为*FWAppliance1*的虚拟机相关联，请运行下面的 PowerShell 命令︰

```powershell
Get-AzureVM -Name FWAppliance1 -ServiceName ProductionVMs `
  	| Get-AzureEffectiveRouteTable
```

上面的命令的输出应如下所示︰

    Effective routes : 
                       Name            Address Prefix    Next hop type    Next hop IP address Status   Source     
                       ----            --------------    -------------    ------------------- ------   ------     
                                       {10.0.0.0/8}      VNETLocal                            Active   Default    
                                       {0.0.0.0/0}       Internet                             Active   Default    
                                       {25.0.0.0/8}      Null                                 Active   Default    
                                       {100.64.0.0/10}   Null                                 Active   Default    
                                       {172.16.0.0/12}   Null                                 Active   Default    
                                       {192.168.0.0/16}  Null                                 Active   Default    
                       firewallroute   {10.2.0.0/16}     Null             10.1.1.10           Active   User      

若要查看路由到辅助 NIC 关联名为*backendnic* ，在一个名为*FWAppliance1*的虚拟机上，运行以下 PowerShell 命令︰

```powershell
Get-AzureVM -Name FWAppliance1 -ServiceName ProductionVMs `
  	| Get-AzureEffectiveRouteTable -NetworkInterfaceName backendnic
```

若要查看与主要角色实例名为*myRole* ，属于一个名为*ProductionVMs*的云服务上 NIC 关联的路由，请运行以下 PowerShell 命令︰

```powershell
Get-AzureEffectiveRouteTable -ServiceName ProductionVMs `
    -RoleInstanceName myRole
```

## 如何管理 IP 转发
如前所述，您需要启用 IP 转发任何 VM 或角色实例，该实例将作为虚拟应用装置上。 

### 如何启用 IP 转发
要启用 IP 转发在一个名为*FWAppliance1*的虚拟机，请运行下面的 PowerShell 命令︰

```powershell
Get-AzureVM -Name FWAppliance1 -ServiceName ProductionVMs `
  	| Set-AzureIPForwarding -Enable
```

要启用 IP 转发了名为*DMZService*的云服务中名为*FWAppliance*的角色实例中，请运行下面的 PowerShell 命令︰

```powershell
Set-AzureIPForwarding -ServiceName DMZService `
    -RoleName FWAppliance -Enable
```

### 如何禁用 IP 转发
要禁用 IP 转发在一个名为*FWAppliance1*的虚拟机，请运行下面的 PowerShell 命令︰

```powershell
Get-AzureVM -Name FWAppliance1 -ServiceName ProductionVMs `
  	| Set-AzureIPForwarding -Disable
```

要禁用 IP 转发在名为*DMZService*的云服务中名为*FWAppliance*的角色实例，请运行下面的 PowerShell 命令︰

```powershell
Set-AzureIPForwarding -ServiceName DMZService `
    -RoleName FWAppliance -Disable
```

### 如何查看 IP 转发的状态
要查看 IP 转发上一个名为*FWAppliance1*的虚拟机的状态，运行下面的 PowerShell 命令︰

```powershell
Get-AzureVM -Name FWAppliance1 -ServiceName ProductionVMs `
  	| Get-AzureIPForwarding
``` 