---
ms.openlocfilehash: eb96c0e7124f78bfa1dedb40298df0f8f54f5ecb
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="实例级公用 IP (ILPIP)"
   description="了解 ILPIP (PIP) 以及如何对其进行管理"
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

# 实例级公用 IP 概述
实例级公用 IP (ILPIP) 是直接向您的 VM 或角色实例，而不是虚拟机或角色实例驻留在云服务可以分配一个 IP 地址。 这并不需要分配给您的云服务 VIP (虚拟 IP) 的地方。 相反，它是直接连接到您的 VM 或角色实例可以使用的其他 IP 地址。

>[AZURE.NOTE] 在过去，ILPIP 被称为骰，即公用 IP。 

![ILPIP 和 VIP 区别](./media/virtual-networks-instance-level-public-ip/Figure1.png)

如图 1 所示，使用 VIP，而通常使用 VIP 访问单个虚拟机访问云服务︰&lt;端口号&gt;。 通过将 ILPIP 分配给一个特定的虚拟机，可以直接使用该 IP 地址来访问虚拟机。

当您创建在 Azure 的云服务时，相应的 DNS A 记录自动创建允许访问该服务通过一个完全合格的域名称 (FQDN) 而不是使用实际的 VIP。 相同的过程发生的 ILPIP，允许通过而不是 ILPIP 的 FQDN 的 VM 或角色实例的访问。 例如，如果创建名为*contosoadservice*，云服务和配置 web 角色名为*contosoweb*的两个实例，Azure 将注册下面的实例 A 记录︰

- contosoweb\_IN_0.contosoadservice.cloudapp.net
- contosoweb\_IN_1.contosoadservice.cloudapp.net 

>[AZURE.NOTE] 您可以指定为每个虚拟机或角色实例只能有一个 ILPIP。 您可以使用达 5 ILPIP 的每个订阅。 在此期间，ILPIP 不支持多个 NIC 的虚拟机。

## 为什么应请求 ILPIP？
如果您想要能够通过直接为其分配的 IP 地址连接到 VM 或角色实例，而不是使用云服务 VIP:&lt;端口号&gt;，ILPIP 征求您的 VM 或角色实例。
- **被动 FTP** -通过 ILPIP 在您的 VM，您可以接收通信上的任何端口，不需要打开接收通信的终结点。 这使被动 FTP 端口选择动态等情形。
- **出站 IP** -来自 VM 的出站通信作为源 ILPIP 与熄灭，它唯一地标识外部实体到虚拟机。

>[AZURE.NOTE] 保留 ILPIPs 的使用可能导致额外的 Azure 订阅费用。 ILPIP 定价的详细信息，请参阅[IP 地址定价](http://azure.microsoft.com/pricing/details/ip-addresses/)。

## 如何在虚拟机创建期间申请 ILPIP
下面的 PowerShell 脚本创建名为*FTPService*，一个新的云服务然后从 Azure，检索的图像并创建一个名为*FTPInstance* ，使用检索到的图像的虚拟机、 设置虚拟机使用 ILPIP，并将虚拟机添加到新的服务︰

    New-AzureService -ServiceName FTPService -Location "Central US"
    $image = Get-AzureVMImage|?{$_.ImageName -like "*RightImage-Windows-2012R2-x64*"}
    New-AzureVMConfig -Name FTPInstance -InstanceSize Small -ImageName $image.ImageName `
  	| Add-AzureProvisioningConfig -Windows -AdminUsername adminuser -Password MyP@ssw0rd!! `
  	| Set-AzurePublicIP -PublicIPName ftpip | New-AzureVM -ServiceName FTPService -Location "Central US"

## 如何检索一个虚拟机的 ILPIP 信息
若要查看虚拟机使用上面的脚本创建的 ILPIP 信息，运行下面的 PowerShell 命令并观察*PublicIPAddress*和*PublicIPName*的值︰

    Get-AzureVM -Name FTPInstance -ServiceName FTPService

    DeploymentName              : FTPService
    Name                        : FTPInstance
    Label                       : 
    VM                          : Microsoft.WindowsAzure.Commands.ServiceManagement.Model.PersistentVM
    InstanceStatus              : ReadyRole
    IpAddress                   : 100.74.118.91
    InstanceStateDetails        : 
    PowerState                  : Started
    InstanceErrorCode           : 
    InstanceFaultDomain         : 0
    InstanceName                : FTPInstance
    InstanceUpgradeDomain       : 0
    InstanceSize                : Small
    HostName                    : FTPInstance
    AvailabilitySetName         : 
    DNSName                     : http://ftpservice888.cloudapp.net/
    Status                      : ReadyRole
    GuestAgentStatus            : Microsoft.WindowsAzure.Commands.ServiceManagement.Model.GuestAgentStatus
    ResourceExtensionStatusList : {Microsoft.Compute.BGInfo}
    PublicIPAddress             : 104.43.142.188
    PublicIPName                : ftpip
    NetworkInterfaces           : {}
    ServiceName                 : FTPService
    OperationDescription        : Get-AzureVM
    OperationId                 : 568d88d2be7c98f4bbb875e4d823718e
    OperationStatus             : OK

## 如何从虚拟机中删除 ILPIP
若要删除添加到在上述脚本中 VM ILPIP，运行下面的 PowerShell 命令︰
    
    Get-AzureVM -ServiceName FTPService -Name FTPInstance `
  	| Remove-AzurePublicIP `
  	| Update-AzureVM

## 如何将 ILPIP 添加到现有的虚拟机
要将 ILPIP 添加到以下命令使用上面，runt 脚本创建虚拟机︰

    Get-AzureVM -ServiceName FTPService -Name FTPInstance `
  	| Set-AzurePublicIP -PublicIPName ftpip2 `
  	| Update-AzureVM

## 如何使用服务配置文件关联到一个虚拟机 ILPIP
您还可以通过使用服务 (CSCFG) 配置文件关联到一个虚拟机 ILPIP。 下面的示例 xml 演示如何配置一个云服务使用名为 ILPIP 的*MyReservedIP*为角色实例保留的 IP: 
    
    <?xml version="1.0" encoding="utf-8"?>
    <ServiceConfiguration serviceName="ReservedIPSample" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration" osFamily="4" osVersion="*" schemaVersion="2014-01.2.3">
      <Role name="WebRole1">
        <Instances count="1" />
        <ConfigurationSettings>
          <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="UseDevelopmentStorage=true" />
        </ConfigurationSettings>
      </Role>
      <NetworkConfiguration>
        <VirtualNetworkSite name="VNet"/>
        <AddressAssignments>
          <InstanceAddress roleName="VMRolePersisted">
            <Subnets>
              <Subnet name="Subnet1"/>
              <Subnet name="Subnet2"/>
            </Subnets>
            <PublicIPs>
              <PublicIP name="MyReservedIP" domainNameLabel="MyReservedIP" />
            </PublicIPs>
          </InstanceAddress>
        </AddressAssignments>
      </NetworkConfiguration>
    </ServiceConfiguration>

## 下一步行动

[保留的 IP](../virtual-networks-reserved-public-ip)

[保留的 IP REST Api](https://msdn.microsoft.com/library/azure/dn722420.aspx)
 