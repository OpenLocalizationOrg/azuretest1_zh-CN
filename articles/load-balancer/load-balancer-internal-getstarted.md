---
ms.openlocfilehash: d6782dd97c66ce8cea6896117f3811b5e1a712f2
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="开始使用内部负载平衡器 |Microsoft Azure "
   description="配置内部负载平衡器和如何实现虚拟机和云部署"
   services="load-balancer"
   documentationCenter="na"
   authors="joaoma"
   manager="adinah"
   editor="tysonn" />
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/01/2015"
   ms.author="joaoma" />

# 开始配置内部负载平衡器

> [AZURE.SELECTOR]
- [Azure 经典步骤](load-balancer-internal-getstarted.md)
- [资源管理器 Powershell 的步骤](load-balancer-internal-arm-powershell.md)

Azure 内部负载平衡 (ILB) 提供驻留在云服务或使用区域范围的虚拟网络的虚拟机之间的负载平衡。 有关使用和配置的区域范围的虚拟网络的信息，请参阅在 Azure 的博客中的[区域的虚拟网络](virtual-networks-migrate-to-regional-vnet.md)。 现有关系组已配置的虚拟网络不能使用 ILB。



## 为虚拟机创建内部负载平衡设置

若要创建一 Azure 负载平衡组并将其通信发送到它的服务器，必须执行以下操作︰

1. 创建一个 ILB 实例，它将会进行负载平衡服务器的负载平衡设置的传入通信的终结点。

1. 添加对应于虚拟机将接收传入的通信的终结点。

1. 要发送的通信进行负载平衡，从而将他们的通信发送到 ILB 实例的虚拟 IP 地址 (VIP) 的服务器进行配置。

### 步骤 1︰ 创建一个 ILB 实例

对于现有的云服务或部署区域的虚拟网络下的云服务，您可以创建并使用以下 Windows PowerShell 命令实例 ILB:

    $svc="<Cloud Service Name>"
    $ilb="<Name of your ILB instance>"
    $subnet="<Name of the subnet within your virtual network>"
    $IP="<The IPv4 address to use on the subnet-optional>"

    Add-AzureInternalLoadBalancer -ServiceName $svc -InternalLoadBalancerName $ilb –SubnetName $subnet –StaticVNetIPAddress $IP


若要使用这些命令，填充的值，并删除 < 和 >。 下面是一个示例︰

    $svc="WebCloud-NY"
    $ilb="SQL-BE"
    $subnet="Farm1"
    $IP="192.168.98.10"
    Add-AzureInternalLoadBalancer -ServiceName $svc -InternalLoadBalancerName $ilb –SubnetName $subnet –StaticVNetIPAddress $IP


### 步骤 2︰ 将终结点添加到 ILB 实例

对现有的虚拟机，可以添加终结点，到 ILB 实例使用以下命令︰

    $svc="<Cloud service name>"
    $vmname="<Name of the VM>"
    $epname="<Name of the endpoint>"
    $lbsetname="<Name of the load balancer set>"
    $prot="tcp" or "udp"
    $locport=<local port number>
    $pubport=<public port number>
    $ilb="<Name of your ILB instance>"
    Get-AzureVM –ServiceName $svc –Name $vmname | Add-AzureEndpoint -Name $epname -LbsetName $lbsetname -Protocol $prot -LocalPort $locport -PublicPort $pubport –DefaultProbe -InternalLoadBalancerName $ilb | Update-AzureVM

若要使用这些命令，填充的值，并删除 < 和 >。

请注意，[添加 AzureEndpoint](https://msdn.microsoft.com/library/dn495300.aspx) Windows PowerShell cmdlet 的这种用法使用 DefaultProbe 参数集。 更多的参数集的详细信息，请参阅[添加 AzureEndpoint](https://msdn.microsoft.com/library/dn495300.aspx)。

下面是一个示例︰

    $svc="AZ-LOB1"
    $vmname="SQL-LOBAZ1"
    $epname="SQL1"
    $lbsetname="SQL-LB"
    $prot="tcp"
    $locport=1433
    $pubport=1433
    $ilb="SQL ILB"
    Get-AzureVM –ServiceName $svc –Name $vmname | Add-AzureEndpoint -Name $epname -Lbset $lbsetname -Protocol $prot -LocalPort $locport -PublicPort $pubport –DefaultProbe -InternalLoadBalancerName $ilb | Update-AzureVM


### 步骤 3︰ 配置您的服务器将其通信发送到新的 ILB 终结点

必须将其通信进行负载平衡以使用新的 IP 地址 (VIP) 的 ILB 实例的服务器配置。 这是 ILB 实例正在侦听地址。 在大多数情况下，您只需添加或修改的 ILB 实例的 VIP 的 DNS 记录。

ILB 实例的创建过程中指定的 IP 地址，如果您已经有 VIP。 否则，您会看到以下命令从 VIP:

    $svc="<Cloud Service Name>"
    Get-AzureService -ServiceName $svc | Get-AzureInternalLoadBalancer



若要使用这些命令，填充的值，并删除 < 和 >。 下面是一个示例︰

    $svc="WebCloud-NY"
    Get-AzureService -ServiceName $svc | Get-AzureInternalLoadBalancer


从获取 AzureInternalLoadBalancer 命令显示，记下 IP 地址并进行必要的更改，您的服务器或 DNS 记录，以确保通信，获取发送到 VIP。

>[AZURE.NOTE] Microsoft Azure 平台用于管理方案的各种静态、 公开可穿绕的 IPv4 地址。 IP 地址是 168.63.129.16。 因为这可能导致意外的行为，不应被任何防火墙，阻止此 IP 地址。
>Azure ILB 中，对于监视探测从负载平衡器使用此 IP 地址，以确定虚拟机运行状况在负载平衡组。 如果网络安全组用于在内部负载平衡集中，限制通信到 Azure 的虚拟机，或应用于虚拟的网络子网，请确保网络安全规则将添加以允许来自 168.63.129.16 的通信。



## 内部的端到端示例负载平衡

为您的端到端过程创建两个示例配置负载平衡设置，请参阅以下各节。

### 面向 Internet 的、 多层应用程序

Contoso 公司想要提供一套面向 Internet 的 web 服务器和数据库服务器的一组之间的负载平衡。 这两组服务器承载于单个 Azure 的云服务。 Web 服务器通信的 TCP 端口 1433年必须分布在数据库层中的三个虚拟机。 图 1 显示了配置。

![数据库层的的内部负载平衡设置](./media/load-balancer-internal-getstarted/IC736321.png)

图 1︰ 面向 Internet 的、 多层应用程序的示例

配置由以下内容组成︰

- 现有的云服务承载虚拟机名为 Contoso PartnerSite。

- 三个现有的数据库服务器名为合作伙伴-SQL-1、 合作伙伴-SQL-2 和合作伙伴-SQL-3。

- Web 层中的 web 服务器连接到数据库服务器在数据库层中使用 DNS 名称合作伙伴 sql.external.contoso.com。

下面的命令配置新 ILB 实例名为合作伙伴 DBTIER 并添加终结点到对应的三个数据库服务器的虚拟机︰

    $svc="Contoso-PartnerSite"
    $ilb="PARTNER-DBTIER"
    Add-AzureInternalLoadBalancer -ServiceName $svc -InternalLoadBalancerName $ilb

    $prot="tcp"
    $locport=1433
    $pubport=1433
    $epname="DBTIER1"
    $lbsetname="SQL-LB"
    $vmname="PARTNER-SQL-1"
    Get-AzureVM –ServiceName $svc –Name $vmname | Add-AzureEndpoint -Name $epname -LbSetName $lbsetname -Protocol $prot -LocalPort $locport -PublicPort $pubport –DefaultProbe -InternalLoadBalancerName $ilb | Update-AzureVM

    $epname="DBTIER2"
    $vmname="PARTNER-SQL-2"
    Get-AzureVM –ServiceName $svc –Name $vmname | Add-AzureEndpoint -Name $epname -LbSetName $lbsetname -Protocol $prot -LocalPort $locport -PublicPort $pubport –DefaultProbe -InternalLoadBalancerName $ilb | Update-AzureVM

    $epname="DBTIER3"
    $vmname="PARTNER-SQL-3"
    Get-AzureVM –ServiceName $svc –Name $vmname | Add-AzureEndpoint -Name $epname -LbSetName $lbsetname -Protocol $prot -LocalPort $locport -PublicPort $pubport –DefaultProbe -InternalLoadBalancerName $ilb | Update-AzureVM

接下来，Contoso 决定 VIP 的合作伙伴 DBTIER ILB 实例，使用以下命令︰

    Get-AzureService -ServiceName $svc | Get-AzureInternalLoadBalancer

通过此命令显示，Contoso 提到 100.64.65.211 的 VIP 地址和配置名称合作伙伴-sql.external.contoso.com 使用新地址的 DNS 地址 (A) 记录。

### 在 Azure 中承载的 LOB 应用程序

Contoso 公司想要承载一套 Azure 中的 web 服务器上的业务线 (LOB) 应用程序。 对 TCP 端口 80 的客户端通信必须在跨部署虚拟网络中运行的三个虚拟机之间的负载平衡。 图 2 显示了配置。

![内部负载平衡的 LOB 应用程序](./media/load-balancer-internal-getstarted/IC744148.png)

图 2︰ 在 Azure 中承载的 LOB 应用程序的示例

配置由以下内容组成︰

- 现有的云服务承载虚拟机名为 Contoso 法律。

- LOB 服务器位于子网命名为 LOB 法律，Contoso 内部负载平衡器的 VIP 地址选择地址 198.168.99.145。

- 三个现有的 LOB 服务器称为法律 1、 法律-2 和法律-3。

- 企业内部网 web 客户端连接到使用 DNS 名称 legalnet.corp.contoso.com。

下面的命令创建名为法律 ILB ILB 实例并添加终结点到对应三个 LOB 服务器的虚拟机︰


    $svc="Contoso-Legal"
    $ilb="LEGAL-ILB"
    $subnet="LOB-LEGAL"
    $IP="198.168.99.145"
    Add-AzureInternalLoadBalancer –ServiceName $svc -InternalLoadBalancerName $ilb –SubnetName $subnet –StaticVNetIPAddress $IP

    $prot="tcp"
    $locport=80
    $pubport=80
    $epname="LOB1"
    $lbsetname="LOB-LB"
    $vmname="LEGAL-1"
    Get-AzureVM –ServiceName $svc –Name $vmname | Add-AzureEndpoint -Name $epname-LbSetName $lbsetname -Protocol $prot -LocalPort $locport -PublicPort $pubport –DefaultProbe -InternalLoadBalancerName $ilb | Update-AzureVM

    $epname="LOB2"
    $vmname="LEGAL2"
    Get-AzureVM –ServiceName $svc –Name $vmname | Add-AzureEndpoint -Name $epname -LbSetName $lbsetname -Protocol $prot -LocalPort $locport -PublicPort $pubport –DefaultProbe -InternalLoadBalancerName $ilb | Update-AzureVM

    $epname="LOB3"
    $vmname="LEGAL3"
    Get-AzureVM –ServiceName $svc –Name $vmname | Add-AzureEndpoint -Name $epname -LbSetName $lbsetname -Protocol $prot -LocalPort $locport -PublicPort $pubport –DefaultProbe -InternalLoadBalancerName $ilb | Update-AzureVM


接下来，Contoso 配置要使用 198.168.99.145 的 legalnet.corp.contoso.com 名称的 DNS A 记录。

## 将虚拟机添加到 ILB

要添加到 ILB 实例的虚拟机创建后，可以使用新建 AzureInternalLoadBalancerConfig 和新建 AzureVMConfig cmdlet。

下面是一个示例︰

    $svc="AZ-LOB1"
    $ilb="LOB-ILB"
    $vnet="LOBNet_Azure"
    $subnet="LOBServers"
    $vmname="LOB-WEB1"
    $adminuser="Lando"
    $adminpw="Platform327"
    $regionname="North Central US"

    $myilbconfig=New-AzureInternalLoadBalancerConfig -InternalLoadBalancerName $ilb -SubnetName $subnet
    $images = Get-AzureVMImage
    New-AzureVMConfig -Name $vmname -InstanceSize Small -ImageName $images[50].ImageName | Add-AzureProvisioningConfig -Windows -AdminUsername $adminuser -Password $adminpw | New-AzureVM -ServiceName $svc -InternalLoadBalancerConfig $myilbconfig -Location $regionname –VNetName $vnet

## 为云服务配置 ILB


ILB 支持的两个虚拟机并在云服务区域的虚拟网络之外创建的云服务 ILB 终结点都可以只在云服务中。

ILB 配置具有设置期间创建的第一个部署在云服务中，如下面的示例中所示。

>[AZURE.IMPORTANT] 运行下面的步骤的先决条件就是要有云部署已创建一个虚拟网络。 您将需要的虚拟网络名称和要创建 ILB 的子网名称。

### 第 1 步

对于您的云部署在 visual studio 中打开服务配置文件 (.cscfg) 并添加下一节创建最后一个下的 ILB"`</Role>`"网络配置信息的项。 




    <NetworkConfiguration>
      <LoadBalancers>
        <LoadBalancer name="name of the load balancer">
          <FrontendIPConfiguration type="private" subnet="subnet-name" staticVirtualNetworkIPAddress="static-IP-address"/>
        </LoadBalancer>
      </LoadBalancers>
    </NetworkConfiguration>
 

让我们添加要显示它的外观类似的网络配置文件的值。 在示例中，假定您创建了子网使用名为 test_subnet 和 10.0.0.4 静态 IP 的子网 10.0.0.0/24 调用"test_vnet"。 负载平衡器将被命名为 testLB。

    <NetworkConfiguration>
      <LoadBalancers>
        <LoadBalancer name="testLB">
          <FrontendIPConfiguration type="private" subnet="test_subnet" staticVirtualNetworkIPAddress="10.0.0.4"/>
        </LoadBalancer>
      </LoadBalancers>
    </NetworkConfiguration>

有关负载平衡器架构的详细信息，请参阅[添加负载平衡器](https://msdn.microsoft.com/library/azure/dn722411.aspx)

### 第 2 步


更改将终结点添加到 ILB 的服务定义 (.csdef) 文件。 创建角色实例，时刻服务定义文件将添加到 ILB 的角色实例。


    <WorkerRole name="worker-role-name" vmsize="worker-role-size" enableNativeCodeExecution="[true|false]">
      <Endpoints>
        <InputEndpoint name="input-endpoint-name" protocol="[http|https|tcp|udp]" localPort="local-port-number" port="port-number" certificate="certificate-name" loadBalancerProbe="load-balancer-probe-name" loadBalancer="load-balancer-name" />
      </Endpoints>
    </WorkerRole>

从上面的示例中相同的值之后, 我们将值添加到服务定义文件 

    <WorkerRole name=WorkerRole1" vmsize="A7" enableNativeCodeExecution="[true|false]">
      <Endpoints>
        <InputEndpoint name="endpoint1" protocol="http" localPort="80" port="80" loadBalancer="testLB" />
      </Endpoints>
    </WorkerRole>

网络通信进行负载平衡使用 testLB 负载平衡器使用传入的请求，发送到辅助角色实例也在端口 80 上的端口 80。 


## 正在删除 ILB 配置

要从 ILB 实例作为终结点中删除虚拟机，请使用以下命令︰

    $svc="<Cloud service name>"
    $vmname="<Name of the VM>"
    $epname="<Name of the endpoint>"
    Get-AzureVM -ServiceName $svc -Name $vmname | Remove-AzureEndpoint -Name $epname | Update-AzureVM

若要使用这些命令，填充值，移除 < 和 >。

下面是一个示例︰

    $svc="AZ-LOB1"
    $vmname="SQL-LOBAZ1"
    $epname="SQL1"
    Get-AzureVM -ServiceName $svc -Name $vmname | Remove-AzureEndpoint -Name $epname | Update-AzureVM

若要删除从云服务的 ILB 实例，请使用以下命令︰

    $svc="<Cloud service name>"
    Remove-AzureInternalLoadBalancer -ServiceName $svc

若要使用这些命令，请填写值，并删除 < 和 >。

下面是一个示例︰

    $svc="AZ-LOB1"
    Remove-AzureInternalLoadBalancer -ServiceName $svc



## 有关 ILB cmdlet 的其他信息


若要获得有关 ILB cmdlet 的其他信息，请在 Azure Windows PowerShell 提示符下运行以下命令︰

- 获取帮助新 AzureInternalLoadBalancerConfig-完整

- 获取帮助添加 AzureInternalLoadBalancer-完整

- 获取帮助获取 AzureInternalLoadbalancer-完整

- 获取帮助删除 AzureInternalLoadBalancer-完整

## 请参见

[配置负载平衡器分布模式](load-balancer-distribution-mode.md)

[配置负载平衡器的空闲 TCP 超时设置](load-balancer-tcp-idle-timeout.md)
 

测试
