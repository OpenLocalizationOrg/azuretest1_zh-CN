---
ms.openlocfilehash: 641616ee96ea9654f2d94b510443794e6ec7166f
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="保留的 IP"
   description="了解保留的 IPs、 VIP、 ILPIP，以及如何对其进行管理"
   services="virtual-network"
   documentationCenter="na"
   authors="telmosampaio"
   manager="adinah"
   editor="tysonn" />
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/17/2015"
   ms.author="telmos" />

# 保留的 IP 概述
在 Azure 中的 IP 地址分为两类︰ 动态和保留。 由 Azure 的公用 IP 地址是动态的默认情况下。 这意味着，IP 地址用于一个给定的云服务 (VIP) 或者访问一个 VM 或角色实例直接 (ILPIP) 可以随时，更改资源时关闭或释放。

以防止更改 IP 地址，您可以保留一个 IP 地址。 保留的 Ip 则只可以用作 VIP，确保云服务的 IP 地址将变即使在关闭或释放资源。 此外，您可以转换现有动态的 Ip 用作 VIP 到保留的 IP 地址。

## 何时需要保留的 IP？
- **您想确保在您的订购中，预留的 IP**。 如果您想要保留 IP 地址，并不会释放从您的订阅在任何情况下，应使用保留的公共 IP。  
- **希望你的 IP，以保持与您的云服务，即使在停止或释放状态 (Vm)**。 如果希望即使在云服务中的虚拟机是否停止或释放您的服务可通过使用 IP 地址不会改变。
- **要确保从 Azure 的出站通信流使用一个可预测的 IP 地址**。 您可能必须内部防火墙配置为允许只有来自特定 IP 地址的通信。 通过保留一个 IP，您将知道的源 IP 地址，不必由于 IP 更改防火墙规则的更新。

## 常见问题
1. 我可以使用所有的 Azure 服务保留的 IP 吗？  
  - 保留的 Ip 仅可用于虚拟机和云服务实例角色。
1. 可以有多少保留的 IPs？  
  - 到目前为止，所有 Azure 订阅有权使用 20 保留的 IPs。 但是，您可以请求其他保留的 IPs。 请参阅详细信息的[订阅和服务限制](../azure-subscription-service-limits/)页。
1. 是否有对保留的 Ip 费用？ 
  - 定价信息，请参阅[保留 IP 地址定价的详细信息](http://go.microsoft.com/fwlink/?LinkID=398482)。
1. 如何保留一个 IP 地址？ 
  - 您可以使用 PowerShell 或[Azure 管理 REST API，](https://msdn.microsoft.com/library/azure/dn722420.aspx)若要从特定区域请求保留的 IP。 Azure 将保留来自该区域的 IP 地址和关联订阅它。 然后可以在该区域中使用保留的 IP。 您不能通过管理门户网站上保留 IP 地址。
1. 我可以使用这组关系基于 VNets？ 
  - 在区域 VNets 只支持保留的 IPs。 它不支持与好友小组的 VNets。 将 VNet 与区域或相关性组相关联的详细信息，请参阅[有关区域 VNets 和关联组](virtual-networks-migrate-to-regional-vnet.md)。 

## 如何管理保留的 Vip

您可以使用保留的 IPs 之前，必须将其添加到您的订购。 若要创建从公共可用的 IP 地址在*美国中部*位置池中保留的 IP，运行下面的 PowerShell 命令︰

    New-AzureReservedIP –ReservedIPName MyReservedIP –Location “Central US”

但请注意，您不能指定 IP 正被保留。 要查看 IP 地址被保留在您的订阅，请运行以下 PowerShell 命令，并注意*ReservedIPName*和*地址*︰

    Get-AzureReservedIP

    ReservedIPName       : MyReservedIP
    Address              : 23.101.114.211
    Id                   : d73be9dd-db12-4b5e-98c8-bc62e7c42041
    Label                : 
    Location             : Central US
    State                : Created
    InUse                : False
    ServiceName          : 
    DeploymentName       : 
    OperationDescription : Get-AzureReservedIP
    OperationId          : 55e4f245-82e4-9c66-9bd8-273e815ce30a
    OperationStatus      : Succeeded

IP 保留的一旦将关联订阅保持直到您将其删除。 若要删除保留的 IP 如上所示，运行下面的 PowerShell 命令︰

    Remove-AzureReservedIP -ReservedIPName "MyReservedIP"

## 如何将新的云服务保留的 IP 相关联
下面的脚本创建一个新的保留的 IP，然后将其关联到名为*TestService*新的云服务。

    New-AzureReservedIP –ReservedIPName MyReservedIP –Location “Central US”
    $image = Get-AzureVMImage|?{$_.ImageName -like "*RightImage-Windows-2012R2-x64*"}
    New-AzureVMConfig -Name TestVM -InstanceSize Small -ImageName $image.ImageName `
  	| Add-AzureProvisioningConfig -Windows -AdminUsername adminuser -Password MyP@ssw0rd!! `
  	| New-AzureVM -ServiceName TestService -ReservedIPName MyReservedIP -Location "Central US"

>[AZURE.NOTE] 创建保留的 IP 使用云服务时，您仍然需要使用引用 VM *VIP:&lt;端口号 >*用于入站通信。 保留一个 IP 并不意味着可以直接连接到虚拟机。 保留的 IP 被分配给虚拟机部署到云服务。 如果想要直接连接到 ip 虚拟机，您必须配置实例级公用 IP。 实例级公用 IP 是一种公用 IP （称为 ILPIP），直接分配给 VM。 无法保留。 有关更多信息，请参见[公共 IP 实例级 (ILPIP)](../virtual-networks-instance-level-public-ip) 。

## 如何删除保留的 IP 从正在运行的部署
要删除保留的 IP 添加到在上述脚本中创建新的服务，请运行下面的 PowerShell 命令︰

    Remove-AzureReservedIPAssociation -ReservedIPName MyReservedIP -ServiceName TestService

>[AZURE.NOTE] 从运行部署删除保留的 IP 不删除预留您的订购。 它只是释放另一个资源在您的订阅中使用的 IP。

## 如何将关联到运行部署保留的 IP
下面的脚本创建一个新的云服务，名为*TestService2* ，与名为*TestVM2*，一个新的虚拟机，然后将现有保留的 IP 命名*MyReservedIP*到云服务相关联。

    $image = Get-AzureVMImage|?{$_.ImageName -like "*RightImage-Windows-2012R2-x64*"}
    New-AzureVMConfig -Name TestVM2 -InstanceSize Small -ImageName $image.ImageName `
  	| Add-AzureProvisioningConfig -Windows -AdminUsername adminuser -Password MyP@ssw0rd!! `
  	| New-AzureVM -ServiceName TestService2 -Location "Central US"
    Set-AzureReservedIPAssociation -ReservedIPName MyReservedIP -ServiceName TestService2

## 如何使用服务配置文件关联到云服务保留的 IP
您还可以通过使用服务 (CSCFG) 配置文件关联到云服务保留的 IP。 下面的 xml 的示例演示如何配置要使用名为*MyReservedIP*保留的 VIP 的云服务︰ 
    
    <?xml version="1.0" encoding="utf-8"?>
    <ServiceConfiguration serviceName="ReservedIPSample" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration" osFamily="4" osVersion="*" schemaVersion="2014-01.2.3">
      <Role name="WebRole1">
        <Instances count="1" />
        <ConfigurationSettings>
          <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="UseDevelopmentStorage=true" />
        </ConfigurationSettings>
      </Role>
      <NetworkConfiguration>
        <AddressAssignments>
          <ReservedIPs>
           <ReservedIP name="MyReservedIP"/>
          </ReservedIPs>
        </AddressAssignments>
      </NetworkConfiguration>
    </ServiceConfiguration>

## 下一步行动

- 了解有关[保留的专用 IP 地址](../virtual-networks-reserved-private-ip)。

- 了解有关[实例级别公共 IP (ILPIP) 地址](../virtual-networks-instance-level-public-ip)。

- 检查[保留的 IP REST Api](https://msdn.microsoft.com/library/azure/dn722420.aspx)。
