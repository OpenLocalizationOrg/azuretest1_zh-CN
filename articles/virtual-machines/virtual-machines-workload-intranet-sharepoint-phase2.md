---
ms.openlocfilehash: c157556eb7b3d69c69d4adcb7508f67afd4b368b
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="SharePoint Server 2013 场第二阶段 |Microsoft Azure"
    description="创建和配置 SharePoint Server 2013 场 Azure 中的第二阶段的两个 Active Directory 复制域控制器。"
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
    ms.date="07/21/2015"
    ms.author="josephd"/>

# SharePoint Intranet 服务器场工作负荷阶段 2︰ 配置域控制器

在这一阶段的部署仅用于内部网的 SharePoint 2013 场与 SQL Server AlwaysOn 可用性组在 Azure 的基础结构服务，您可以配置在 Azure 的虚拟网络服务管理中的两个域控制器。 客户端然后可以在 Azure 的虚拟网络中，身份验证服务器场资源的 SharePoint web 请求而不是通过 VPN 或 Azure ExpressRoute 连接到您的内部网络中发送该身份验证通迅流。

您必须先完成这一阶段之前将移到[第 3 阶段](virtual-machines-workload-intranet-sharepoint-phase3.md)。 请参阅[使用 Azure 中的 SQL Server AlwaysOn 可用性组部署 SharePoint](virtual-machines-workload-intranet-sharepoint-overview.md)所有阶段。

## 在 Azure 创建域控制器虚拟机

首先，您需要填写表格 M 的**虚拟机名称**列中，根据需要在**最小大小**列中修改虚拟机大小。  

项目 | 虚拟机的名称 | 插图 | 最小大小
--- | --- | --- | ---
1。 | ___ （DC1 的示例第一个域控制器) | Windows Server 2012 R2 数据中心 | A2 （中）
2。 | ___ （第二个域控制器，DC2 的示例） | Windows Server 2012 R2 数据中心 | A2 （中）
3。 | ___ （第一台 SQL Server 的计算机的示例 SQL1） | Microsoft SQL Server 2014年企业 – Windows Server 2012 R2 |     A7
4。 | （第二个示例 SQL2 的 SQL Server 计算机） ___ | Microsoft SQL Server 2014年企业 – Windows Server 2012 R2 |    A7
5。 | ___ （多数为群集，示例 MN1 见证节点） | Windows Server 2012 R2 数据中心 | A1 （小）
6。 | ___ （第一个 SharePoint 应用程序服务器的示例 APP1） | Microsoft SharePoint 服务器 2013年试用软件 – Windows Server 2012 R2 | A4 (ExtraLarge)
7。 | （第二个 SharePoint 应用程序服务器示例 APP2） ___ | Microsoft SharePoint 服务器 2013年试用软件 – Windows Server 2012 R2 | A4 (ExtraLarge)
8。 | (第一个 SharePoint web 服务器示例 WEB1) ___ | Microsoft SharePoint 服务器 2013年试用软件 – Windows Server 2012 R2 | A4 (ExtraLarge)
9。 | （第二个 SharePoint web 服务器示例 WEB2） ___ | Microsoft SharePoint 服务器 2013年试用软件 – Windows Server 2012 R2 | A4 (ExtraLarge)

**表 M — Azure 中的 SharePoint 2013 intranet 服务器场的虚拟机**

虚拟机大小的完整列表，请参阅[虚拟机大小](virtual-machines-size-specs.md)。

使用下面的块的 Azure PowerShell 命令创建的两个域控制器虚拟机。 为指定的值的变量，删除 < 和 > 字符。 请注意，设置此 Azure PowerShell 命令使用以下值︰

- 表 M，为您的虚拟机
- 表 V，为虚拟网络设置
- 表 S，为您的子网
- 表 A，以您的可用性设置
- 表 C，云服务

撤回定义表 V，S，A，并在 C[第 1 阶段︰ 配置 Azure](virtual-machines-workload-intranet-sharepoint-phase1.md)。

当您提供了所有的正确值时，Azure PowerShell 命令提示符处运行产生的块。

    # Create the first domain controller
    $vmName="<Table M – Item 1 - Virtual machine name column>"
    $vmSize="<Table M – Item 1 - Minimum size column, specify one: Small, Medium, Large, ExtraLarge, A5, A6, A7, A8, A9>"
    $availSet="<Table A – Item 1 – Availability set name column>"
    $image= Get-AzureVMImage | where { $_.ImageFamily -eq "Windows Server 2012 R2 Datacenter" } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1
    $vm1=New-AzureVMConfig -Name $vmName -InstanceSize $vmSize -ImageName $image -AvailabilitySetName $availSet

    $cred=Get-Credential –Message "Type the name and password of the local administrator account for the first domain controller."
    $vm1 | Add-AzureProvisioningConfig -Windows -AdminUsername $cred.GetNetworkCredential().Username -Password $cred.GetNetworkCredential().Password

    $diskSize=<size of the additional data disk in GB>
    $diskLabel="<the label on the disk>"
    $lun=<Logical Unit Number (LUN) of the disk>
    $vm1 | Add-AzureDataDisk -CreateNew -DiskSizeInGB $diskSize -DiskLabel $diskLabel -LUN $lun -HostCaching None

    $subnetName="<Table S – Item 1 – Subnet name column>"
    $vm1 | Set-AzureSubnet -SubnetNames $subnetName

    $vm1 | Set-AzureStaticVNetIP -IPAddress <Table V – Item 6 – Value column>

    $serviceName="<Table C – Item 1 – Cloud service name column>"
    $vnetName="<Table V – Item 1 – Value column>"
    New-AzureVM –ServiceName $serviceName -VMs $vm1 -VNetName $vnetName

    # Create the second domain controller
    $vmName="<Table M – Item 2 - Virtual machine name column>"
    $vmSize="<Table M – Item 2 - Minimum size column, specify one: Small, Medium, Large, ExtraLarge, A5, A6, A7, A8, A9>"
    $image= Get-AzureVMImage | where { $_.ImageFamily -eq "Windows Server 2012 R2 Datacenter" } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

    $vm1=New-AzureVMConfig -Name $vmName -InstanceSize $vmSize -ImageName $image -AvailabilitySetName $availSet

    $cred=Get-Credential –Message "Type the name and password of the local administrator account for the second domain controller."
    $vm1 | Add-AzureProvisioningConfig -Windows -AdminUsername $cred.GetNetworkCredential().Username -Password $cred.GetNetworkCredential().Password

    $diskSize=<size of the additional data disk in GB>
    $diskLabel="<the label on the disk>"
    $lun=<Logical Unit Number (LUN) of the disk>
    $vm1 | Add-AzureDataDisk -CreateNew -DiskSizeInGB $diskSize -DiskLabel $diskLabel -LUN $lun -HostCaching None

    $vm1 | Set-AzureSubnet -SubnetNames $subnetName

    $vm1 | Set-AzureStaticVNetIP -IPAddress <Table V – Item 7 – Value column>

    New-AzureVM –ServiceName $serviceName -VMs $vm1 -VNetName $vnetName

## 将第一个域控制器配置

使用本地管理员帐户的凭据登录到第一个域控制器计算机。

### <a id="logon"></a>若要通过使用远程桌面连接登录到虚拟机

1.  在 Azure 的门户中，在左窗格中，单击**虚拟机**。
2.  要连接到虚拟机，请单击其名称旁边的**状态**列中的**运行**。
3.  在页面底部的命令栏中，单击**连接**。
4.  门户网站将通知您正在检索的.rdp 文件。 单击**确定**。
5.  浏览器对话框中会显示，询问"要打开或保存从 manage.windowsazure.com ComputerName.rdp？" 单击**打开**。
6.  在**远程桌面连接**对话框中，单击**连接**。
7.  在**Windows 安全**对话框中，单击**使用另一个帐户**。
8.  在**用户名**键入本地管理员帐户创建的虚拟机 （本地计算机帐户） 的虚拟机和用户名称的名称。 使用以下格式︰*计算机名称*\\*LocalAdministratorAccountName*
9.  在**密码**框中，键入本地管理员帐户的密码。
10. 单击**确定**。
11. 在**远程桌面连接**对话框中，单击**是**。 在远程桌面会话窗口中出现的新计算机的桌面。

接下来，您需要添加额外的数据磁盘的第一个域控制器。

### <a id="datadisk"></a>若要初始化一个空的磁盘

1.  在服务器管理器的左窗格中，单击**文件和存储服务**，然后单击**磁盘**。
2.  在内容窗格中的**磁盘**组中，单击磁盘**2** （设置为**未知****分区**）。
3.  单击**任务**，然后单击**新建卷**。
4.  在新建卷向导**开始之前**页中，单击**下一步**。
5.  在**选择服务器和磁盘**页上，单击**磁盘 2**，然后单击**下一步**。 出现提示时，单击**确定**。
6.  在**指定卷的大小**页面上，单击**下一步**。
7.  在**分配驱动器号或文件夹**页面上，单击**下一步**。
8.  在**选择文件系统设置**页上，单击**下一步**。
9.  在**确认选择**页中，单击**创建**。
10. 初始化完成后，单击**关闭**。

接下来，测试的第一个域控制器连接到组织网络上的位置。

### <a id="testconn"></a>若要测试连接

1.  从桌面，打开 Windows PowerShell 命令提示符。
2.  使用**ping**命令来 ping 名称和 IP 地址，您的组织的网络上的资源。

此过程确保该 DNS 名称解析工作正常 （即使用内部 DNS 服务器正确配置该虚拟机），并可以跨部署虚拟网络进出时发送的数据包。

接下来，第一个域控制器的 Windows PowerShell 命令提示符下，运行下面的命令︰

    $domname="<DNS domain name of the domain for which this computer will be a domain controller>"
    Install-WindowsFeature AD-Domain-Services -IncludeManagementTools
    Install-ADDSDomainController -InstallDns –DomainName $domname  -DatabasePath "F:\NTDS" -SysvolPath "F:\SYSVOL" -LogPath "F:\Logs"

计算机将重新启动。

## 配置第二个域控制器

通过使用其本地管理员帐户的凭据登录到第二个域控制器计算机。 有关说明，请参阅[登录到虚拟机的远程桌面连接过程](#logon)。

接下来，您需要添加额外的数据磁盘到另一个域控制器。 [初始化一个空的磁盘过程](#datadisk)，请参阅。

接下来，测试您的连接到组织网络上的位置从第二个域控制器。 [若要测试连接过程](#testconn)，请参阅。 使用此过程可以确保该 DNS 名称解析工作正常 （即使用内部 DNS 服务器正确配置该虚拟机），并可以跨部署虚拟网络进出时发送的数据包。

接下来，第二个域控制器上的 Windows PowerShell 命令提示符下，运行下面的命令︰

    $domname="<DNS domain name of the domain for which this computer will be a domain controller>"
    Install-WindowsFeature AD-Domain-Services -IncludeManagementTools
    Install-ADDSDomainController -InstallDns –DomainName $domname  -DatabasePath "F:\NTDS" -SysvolPath "F:\SYSVOL" -LogPath "F:\Logs"

计算机将重新启动。

## 配置 SharePoint 服务器场帐户和权限

SharePoint 服务器场将需要以下用户帐户︰

- sp_farm︰ 用户帐户来管理 SharePoint 服务器场。
- sp_farm_db︰ 在 SQL Server 实例具有系统管理员权限的用户帐户。
- sp_install︰ 已安装的角色和功能所需的域管理权限的用户帐户。
- sqlservice︰ 可以作为运行 SQL Server 实例的用户帐户。

接下来，登录到域管理员帐户的域的域控制器是成员的任何计算机上，打开管理员级别的 Windows PowerShell 命令提示符，并运行这些命令*一次*︰

    New-ADUser -SamAccountName sp_farm -AccountPassword (read-host "Set user password" -assecurestring) -name "sp_farm" -enabled $true -PasswordNeverExpires $true -ChangePasswordAtLogon $false

    New-ADUser -SamAccountName sp_farm_db -AccountPassword (read-host "Set user password" -assecurestring) -name "sp_farm_db" -enabled $true -PasswordNeverExpires $true -ChangePasswordAtLogon $false

    New-ADUser -SamAccountName sp_install -AccountPassword (read-host "Set user password" -assecurestring) -name "sp_install" -enabled $true -PasswordNeverExpires $true -ChangePasswordAtLogon $false

    New-    ADUser -SamAccountName sqlservice -AccountPassword (read-host "Set user password" -assecurestring) -name "sqlservice" -enabled $true -PasswordNeverExpires $true -ChangePasswordAtLogon $false

为每个命令，您将被提示输入密码。 记录这些帐户名和密码并将它们存储在安全的位置。

接下来，请执行以下步骤以将多个帐户的属性添加到新的用户帐户。

1.  从开始屏幕中，键入**活动目录用户**，然后单击**Active Directory 用户和计算机**。
2.  在树窗格中，打开您的域，然后单击**用户**。
3.  在内容窗格中，用鼠标右键单击**sp_install**，，然后单击**添加到组**。
4.  在**选择组**对话框中，键入**域管理员**，然后单击**确定**两次。
5.  在对话框中，单击**视图并单击高级功能**。 该选项允许您查看所有隐藏的容器和 Active Directory 对象的属性窗口中的隐藏选项卡。
6.  右键单击域名，然后单击**属性**。
7.  在**属性**对话框中，单击**安全**选项卡，然后单击**高级**按钮。
8.  在**的高级安全设置<YourDomain>**窗口中，单击**添加**。
9.  在**的权限条目<YourDomain>**窗口中，单击**选择主体**。
10. 在文本框中，键入**<YourDomain>\sp_install**，然后单击**确定**。
11. 对于**创建计算机对象**，选择**允许**，然后单击**确定**三次。

下一步，更新的 DNS 服务器的虚拟网络，以便该 Azure 将虚拟机分配了两个新的域控制器用作其 DNS 服务器的 IP 地址。 请注意，此过程使用来自表 V 值 （对于虚拟网络设置）。

1.  在 Azure 门户的左窗格中，单击**网络**，，然后单击虚拟网络 （表 – 1 项 – V 值列） 的名称。
2.  单击**配置**。
3.  在**DNS 服务器**中删除位于内部网络的 DNS 服务器所对应的条目。
4.  在**DNS 服务器**中，将添加两个条目与友好名称和这两个表项的 IP 地址︰
 - – 项目 6 – V 值表列
 - – 项目 7 – V 值表列
5.  在底部的命令栏中，单击**保存**。
6.  在 Azure 门户的左窗格中，单击**虚拟机**，然后单击**状态**列旁的第一个域控制器的名称。
7.  在命令栏中，单击**重新启动**。
8.  启动后的第一个域控制器，请单击**状态**列旁的第二个域控制器的名称。
9.  在命令栏中，单击**重新启动**。 等待，直到第二个域控制器已启动。

请注意，以便它们不已配置为 DNS 服务器的内部 DNS 服务器重新启动两个域控制器。 因为它们本身的两个 DNS 服务器，它们会自动配置使用内部 DNS 服务器作为 DNS 转发器时它们都将提升为域控制器。

接下来，您需要创建一个 Active Directory 复制站点以确保 Azure 的虚拟网络中的服务器使用本地域控制器。 登录到该域的主域控制器的 sp_install 帐户和管理员级 Windows PowerShell 命令提示符下运行以下命令︰

    $vnet="<Table V – Item 1 – Value column>"
    $vnetSpace="<Table V – Item 5 – Value column>"
    New-ADReplicationSite -Name $vnet
    New-ADReplicationSubnet –Name $vnetSpace –Site $vnet

此关系图显示因成功完成这一阶段，与占位符计算机名称的配置。

![](./media/virtual-machines-workload-intranet-sharepoint-phase2/workload-spsqlao_02.png)

## 下一步

要继续进行此工作负载的配置，请转到[阶段 3︰ 配置 SQL Server 基础架构](virtual-machines-workload-intranet-sharepoint-phase3.md)。

## 其他资源

[在 Azure 中的 SQL Server AlwaysOn 可用性组部署 SharePoint](virtual-machines-workload-intranet-sharepoint-overview.md)

[在 Azure 的基础结构服务承载 SharePoint 服务器场](virtual-machines-sharepoint-infrastructure-services.md)

[SharePoint 与 SQL Server AlwaysOn infographic](http://go.microsoft.com/fwlink/?LinkId=394788)

[SharePoint 2013 的 Microsoft Azure 结构](https://technet.microsoft.com/library/dn635309.aspx)

[Azure 的基础结构服务实施指南](virtual-machines-infrastructure-services-implementation-guidelines.md)

[Azure 的基础结构服务工作负荷︰ 高可用性业务线应用程序](virtual-machines-workload-high-availability-lob-application.md)
