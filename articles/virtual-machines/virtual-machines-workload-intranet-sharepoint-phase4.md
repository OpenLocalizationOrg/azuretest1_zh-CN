---
ms.openlocfilehash: 613d722ad9b5f6e4174bb7d2ceccc12759c03336
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="SharePoint Server 2013 场阶段 4 |Microsoft Azure"
    description="在阶段 4 中的 SharePoint Server 2013 群 Azure 中创建 SharePoint 服务器虚拟机和新的 SharePoint 服务器场。"
    documentationCenter=""
    services="virtual-machines"
    authors="JoeDavies-MSFT"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows-sharepoint"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/22/2015"
    ms.author="josephd"/>

# SharePoint Intranet 服务器场工作负荷阶段 4︰ 配置 SharePoint 服务器

在这一阶段的部署仅用于内部网的 SharePoint 2013 场与 SQL Server AlwaysOn 可用性组在 Azure 的基础结构服务，构建应用程序和 web 层的 SharePoint 服务器场，并通过使用 SharePoint 配置向导创建服务器场。

这一阶段到[第五阶段](virtual-machines-workload-intranet-sharepoint-phase5.md)搬迁之前，必须完成。 请参阅[使用 Azure 中的 SQL Server AlwaysOn 可用性组部署 SharePoint](virtual-machines-workload-intranet-sharepoint-overview.md)所有阶段。

## 在 Azure 创建 SharePoint 服务器虚拟机

有四个 SharePoint 服务器虚拟机。 两个 SharePoint 服务器虚拟机前端 web 服务器，并且两个供管理和承载 SharePoint 应用程序。 每个层中的两个 SharePoint 服务器提供高可用性。

使用下面的块的 Azure PowerShell 命令来创建四个 SharePoint 服务器的虚拟机。 为指定的值的变量，删除 < 和 > 字符。 请注意，此 Azure PowerShell 命令设置使用下表中的值︰

- 表 M，为您的虚拟机
- 表 V，为虚拟网络设置
- 表 S，为您的子网
- 表 A，以您的可用性设置
- 表 C，云服务

记得您定义表中的 M[第 2 阶段︰ 配置域控制器](virtual-machines-workload-intranet-sharepoint-phase2.md)和表 V，S，A 和 C 中的[第 1 阶段︰ 配置 Azure](virtual-machines-workload-intranet-sharepoint-phase1.md)。

当您提供了所有的正确值时，Azure PowerShell 命令提示符处运行产生的块。

    # Create the first SharePoint application server
    $vmName="<Table M – Item 6 - Virtual machine name column>"
    $vmSize="<Table M – Item 6 - Minimum size column, specify one: Small, Medium, Large, ExtraLarge, A5, A6, A7, A8, A9>"
    $availSet="<Table A – Item 3 – Availability set name column>"
    $image= Get-AzureVMImage | where { $_.Label -eq "SharePoint Server 2013 Trial" } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1
    $vm1=New-AzureVMConfig -Name $vmName -InstanceSize $vmSize -ImageName $image -AvailabilitySetName $availSet

    $cred1=Get-Credential –Message "Type the name and password of the local administrator account for the first SharePoint application server."
    $cred2=Get-Credential –Message "Now type the name and password of an account that has permissions to add this virtual machine to the domain."
    $ADDomainName="<name of the AD domain that the server is joining (example CORP)>"
    $domainDNS="<FQDN of the AD domain that the server is joining (example corp.contoso.com)>"
    $vm1 | Add-AzureProvisioningConfig -AdminUsername $cred1.GetNetworkCredential().Username -Password $cred1.GetNetworkCredential().Password -WindowsDomain -Domain $ADDomainName -DomainUserName $cred2.GetNetworkCredential().Username -DomainPassword $cred2.GetNetworkCredential().Password -JoinDomain $domainDNS

    $subnetName="<Table 6 – Item 1 – Subnet name column>"
    $vm1 | Set-AzureSubnet -SubnetNames $subnetName

    $serviceName="<Table C – Item 3 – Cloud service name column>"
    $vnetName="<Table V – Item 1 – Value column>"
    New-AzureVM –ServiceName $serviceName -VMs $vm1 -VNetName $vnetName

    # Create the second SharePoint application server
    $vmName="<Table M – Item 7 - Virtual machine name column>"
    $vmSize="<Table M – Item 7 - Minimum size column, specify one: Small, Medium, Large, ExtraLarge, A5, A6, A7, A8, A9>"
    $vm1=New-AzureVMConfig -Name $vmName -InstanceSize $vmSize -ImageName $image -AvailabilitySetName $availSet

    $cred1=Get-Credential –Message "Type the name and password of the local administrator account for the second SharePoint application server."
    $vm1 | Add-AzureProvisioningConfig -AdminUsername $cred1.GetNetworkCredential().Username -Password $cred1.GetNetworkCredential().Password -WindowsDomain -Domain $ADDomainName -DomainUserName $cred2.GetNetworkCredential().Username -DomainPassword $cred2.GetNetworkCredential().Password -JoinDomain $domainDNS

    $vm1 | Set-AzureSubnet -SubnetNames $subnetName

    New-AzureVM –ServiceName $serviceName -VMs $vm1 -VNetName $vnetName

    # Create the first SharePoint web server
    $vmName="<Table M – Item 8 - Virtual machine name column>"
    $vmSize="<Table M – Item 8 - Minimum size column, specify one: Small, Medium, Large, ExtraLarge, A5, A6, A7, A8, A9>"
    $availSet="<Table A – Item 4 – Availability set name column>"
    $vm1=New-AzureVMConfig -Name $vmName -InstanceSize $vmSize -ImageName $image -AvailabilitySetName $availSet

    $cred1=Get-Credential –Message "Type the name and password of the local administrator account for the first SharePoint web server."
    $vm1 | Add-AzureProvisioningConfig -AdminUsername $cred1.GetNetworkCredential().Username -Password $cred1.GetNetworkCredential().Password -WindowsDomain -Domain $ADDomainName -DomainUserName $cred2.GetNetworkCredential().Username -DomainPassword $cred2.GetNetworkCredential().Password -JoinDomain $domainDNS

    $vm1 | Set-AzureSubnet -SubnetNames $subnetName

    New-AzureVM –ServiceName $serviceName -VMs $vm1 -VNetName $vnetName

    # Create the second SharePoint web server
    $vmName="<Table M – Item 9 - Virtual machine name column>"
    $vmSize="<Table M – Item 9 - Minimum size column, specify one: Small, Medium, Large, ExtraLarge, A5, A6, A7, A8, A9>"
    $vm1=New-AzureVMConfig -Name $vmName -InstanceSize $vmSize -ImageName $image -AvailabilitySetName $availSet

    $cred1=Get-Credential –Message "Type the name and password of the local administrator account for the second SharePoint web server."
    $vm1 | Add-AzureProvisioningConfig -AdminUsername $cred1.GetNetworkCredential().Username -Password $cred1.GetNetworkCredential().Password -WindowsDomain -Domain $ADDomainName -DomainUserName $cred2.GetNetworkCredential().Username -DomainPassword $cred2.GetNetworkCredential().Password -JoinDomain $domainDNS

    $vm1 | Set-AzureSubnet -SubnetNames $subnetName

    New-AzureVM –ServiceName $serviceName -VMs $vm1 -VNetName $vnetName

用于[登录到虚拟机的远程桌面连接过程](virtual-machines-workload-intranet-sharepoint-phase2.md#logon)四次，一次每个 SharePoint 服务器，使用 [域] \sp_farm_db 帐户凭据登录。 创建这些凭据中的[第二阶段︰ 将域控制器配置](virtual-machines-workload-intranet-sharepoint-phase2.md)。

用于[测试连接过程](virtual-machines-workload-intranet-sharepoint-phase2.md#testconn)四次，一次每个 SharePoint 服务器上，以测试连接到组织网络上的位置。

## 配置 SharePoint 服务器场

使用下列步骤在服务器场中配置的第一个 SharePoint 服务器︰

1.  从第一个 SharePoint 应用程序服务器的桌面，双击**SharePoint 2013 产品配置向导**。 当提示您允许程序对计算机进行更改时，请单击**是**。
2.  在**欢迎使用 SharePoint 产品**页上，单击**下一步**。
3.  将出现一个**SharePoint 产品配置向导**对话框，警告将重新启动或重置服务 （如 IIS)。 单击**是**。
4.  在**连接到服务器场中**页上，选择**创建新的服务器场**，然后单击**下一步**。
5.  在**指定配置数据库设置**页中︰
 - 在**数据库服务器**中，键入主数据库服务器的名称。
 - 在**用户名**，键入 [域]**\sp_farm_db** (在中创建[第二阶段︰ 将域控制器配置](virtual-machines-workload-intranet-sharepoint-phase2.md))。 前面曾提到的 sp_farm_db 帐户数据库服务器上具有系统管理员权限。
 - 在**密码**框中，键入 sp_farm_db 帐户密码。
6.  单击**下一步**。
7.  在**指定服务器场安全设置**页面上键入密码两次。 记录此密码并将其存储在一个安全的位置，以供将来参考。 单击**下一步**。
8.  在**配置 SharePoint 管理中心 Web 应用程序**页上，单击**下一步**。
9.  将出现**完成 SharePoint 产品配置向导**页。 单击**下一步**。
10. 显示**配置 SharePoint 产品**页面。 等待，直到配置过程完成后，大约 8 分钟。
11. 服务器场配置成功后，单击**完成**。 启动新的管理网站。
12. 要开始配置 SharePoint 服务器场，请单击**启动向导**。

第二个 SharePoint 应用程序服务器和两个前端 web 服务器上执行下列步骤︰

1.  在桌面上，双击**SharePoint 2013 产品配置向导**。 当提示您允许程序对计算机进行更改时，请单击**是**。
2.  在**欢迎使用 SharePoint 产品**页上，单击**下一步**。
3.  将出现一个**SharePoint 产品配置向导**对话框，警告将重新启动或重置服务 （如 IIS)。 单击**是**。
4.  在**连接到服务器场中**页上，单击**连接到现有服务器场**，然后单击**下一步**。
5.  在**指定配置数据库设置**页中，在**数据库服务器**中键入主数据库服务器的名称，然后单击**检索数据库名称**。
6.  在数据库名称列表中，单击**SharePoint_Config** ，然后单击**下一步**。
7.  在**指定的服务器场安全设置**页上，键入前面过程中的密码。 单击**下一步**。
8.  将出现**完成 SharePoint 产品配置向导**页。 单击**下一步**。
9.  在**成功配置**页上，单击**完成**。

当 SharePoint 服务器场时，它将主要的 SQL Server 虚拟机上配置一套服务器登录。 SQL Server 2012年引入了包含的数据库的密码与用户的概念。 数据库本身存储了所有的数据库元数据和用户信息，并在此数据库中定义的用户不需要有相应的登录。 此数据库中的信息复制的可用性组，可在故障转移之后。 有关详细信息，请参阅[包含的数据库](http://go.microsoft.com/fwlink/p/?LinkId=262794)。

但是，默认情况下，SharePoint 数据库不是包含的数据库。 因此，您将需要手动配置辅助数据库服务器，以使其具有主要的数据库服务器作为一组相同的 SharePoint 服务器场帐户的登录名。 通过同时连接到两台服务器，您可以从 SQL Server 管理 Studio 执行此同步。

完成初始设置后，更多能力的 SharePoint 服务器场的配置选项都可用。 有关详细信息，请参阅[SharePoint 2013 在 Azure 的基础结构服务规划](http://msdn.microsoft.com/library/dn275958.aspx)。

## 配置内部负载平衡

您配置内部负载平衡，以使客户端到 SharePoint 场通讯均匀分布到两台前端 web 服务器。 这要求您指定一个名称和其 IP 地址，从子网地址空间分配组成的内部负载平衡实例。 若要确定所选择的内部负载平衡器的 IP 地址是否可用，请使用以下的 Azure PowerShell 命令︰

    $vnet="<Table V – Item 1 – Value column>"
    $testIP="<a chosen IP address from the subnet address space, Table S - Item 1 – Subnet address space column>"
    Test-AzureStaticVNetIP –VNetName $vnet –IPAddress $testIP

如果**测试 AzureStaticVNetIP**命令显示中的**IsAvailable**字段为**True**时，您可以使用的 IP 地址。

在您的本地计算机上 Azure PowerShell 命令提示符下，运行下面的命令集︰

    $serviceName="<Table C – Item 3 – Cloud service name column>"
    $ilb="<name of your internal load balancer instance>"
    $subnet="<Table S – Item 1 – Subnet name column>"
    $IP="<an available IP address for your ILB instance>"
    Add-AzureInternalLoadBalancer –ServiceName $serviceName -InternalLoadBalancerName $ilb –SubnetName $subnet –StaticVNetIPAddress $IP

    $prot="tcp"
    $locport=80
    $pubport=80
    # This example assumes unsecured HTTP traffic to the SharePoint farm.

    $epname="SPWeb1"
    $vmname="<Table M – Item 8 – Virtual machine name column>"
    Get-AzureVM –ServiceName $serviceName –Name $vmname | Add-AzureEndpoint -Name $epname -LBSetName $ilb -Protocol $prot -LocalPort $locport -PublicPort $pubport –DefaultProbe -InternalLoadBalancerName $ilb | Update-AzureVM

    $epname="SPWeb2"
    $vmname="<Table M – Item 9 – Virtual machine name column>"
    Get-AzureVM –ServiceName $serviceName –Name $vmname | Add-AzureEndpoint -Name $epname -LBSetName $ilb -Protocol $prot -LocalPort $locport -PublicPort $pubport –DefaultProbe -InternalLoadBalancerName $ilb | Update-AzureVM

接下来，添加到您的组织的 DNS 基础结构可解决 SharePoint 服务器场 （如 sp.corp.contoso.com) 的完全限定的域名的 DNS 地址记录的 IP 地址分配给内部负载平衡器实例 （在前面的 Azure PowerShell 命令块**$IP**的值）。

这是源于这一阶段成功完成的配置︰

![](./media/virtual-machines-workload-intranet-sharepoint-phase4/workload-spsqlao_04.png)

## 下一步

要继续进行此工作负载的配置，请转到[阶段 5︰ 创建可用性组并添加 SharePoint 数据库](virtual-machines-workload-intranet-sharepoint-phase5.md)。

## 其他资源

[在 Azure 中的 SQL Server AlwaysOn 可用性组部署 SharePoint](virtual-machines-workload-intranet-sharepoint-overview.md)

[在 Azure 的基础结构服务承载 SharePoint 服务器场](virtual-machines-sharepoint-infrastructure-services.md)

[SharePoint 与 SQL Server AlwaysOn infographic](http://go.microsoft.com/fwlink/?LinkId=394788)

[SharePoint 2013 的 Microsoft Azure 结构](https://technet.microsoft.com/library/dn635309.aspx)

[Azure 的基础结构服务实施指南](virtual-machines-infrastructure-services-implementation-guidelines.md)

[Azure 的基础结构服务工作负荷︰ 高可用性业务线应用程序](virtual-machines-workload-high-availability-lob-application.md)
