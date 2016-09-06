---
ms.openlocfilehash: a31c29d39f45d93cc9ba6e9fa780f8a79c2439bc
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="基本配置测试环境使用 Azure 资源管理器"
    description="了解如何创建一个简单的开发测试环境，模拟在 Microsoft Azure 使用资源管理器中的简化内部网。"
    documentationCenter=""
    services="virtual-machines"
    authors="JoeDavies-MSFT"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/23/2015"
    ms.author="josephd"/>

# 基本配置测试环境使用 Azure 资源管理器

本文提供了用来创建基本配置的分步指导您在 Microsoft Azure 虚拟网络中，测试环境使用资源管理器中创建的虚拟机。 

您可以使用生成的测试环境︰

- 应用程序开发和测试。
- 为自己设计一个长的测试环境的初始配置，包括附加的虚拟机和 Azure 服务。

基本配置测试环境由云专用虚拟网络命名测试实验室模拟简化、 专用内联网连接到 Internet 的企业网络子网组成。

![](./media/virtual-machines-base-configuration-test-environment-resource-manager/BC_TLG04.png)

它包含︰

- 运行 Windows Server 2012 R2 Azure 虚拟机命名 dc1，将其配置为一个 intranet 域控制器和域名系统 (DNS) 服务器。
- 运行 Windows Server 2012 R2 Azure 虚拟机命名 APP1 配置为常规的应用程序和 web 服务器。
- 运行 Windows Server 2012 R2 Azure 虚拟机命名用作 intranet 客户端的客户端 1。

这种配置使得 DC1、 APP1、 客户端 1 和其他 Corpnet 子网计算机是︰  

- 连接到 Internet 来安装更新程序，访问实时互联网资源，并且参与公有云技术，如 Microsoft Office 365 和其他 Azure 服务。
- 远程管理计算机连接到 Internet 或您的组织的网络中使用远程桌面连接。

有四个阶段设置 Azure 中 Windows Server 2012 R2 基本配置测试环境的企业网络子网。

1.  创建虚拟网络。
2.  配置 DC1。
3.  配置 app1 发出。
4.  将 CLIENT1 配置。

如果您没有一个 Azure 帐户，您可以注册在[尝试 Azure](http://azure.microsoft.com/pricing/free-trial/)的免费试用版。 如果您订阅了 MSDN，请参阅[MSDN 订阅者受益的 Azure](http://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/)。

> [AZURE.NOTE] 在 Azure 中的虚拟机运行时招来持续的货币成本。 此成本记帐针对免费试用版，MSDN 订阅，或付款订购。 运行虚拟机 Azure 的成本的详细信息，请参阅[虚拟机定价详细信息](http://azure.microsoft.com/pricing/details/virtual-machines/)和[Azure 定价计算器](http://azure.microsoft.com/pricing/calculator/)。 为了降低成本，请参阅[测试环境在 Azure 中的虚拟机的成本降至最低](#costs)。

[AZURE.INCLUDE [resource-manager-pointer-to-service-management](../../includes/resource-manager-pointer-to-service-management.md)]

- [基本配置测试环境](virtual-machines-base-configuration-test-environment.md)


## 阶段 1︰ 创建虚拟网络

首先，如果需要请按照中说明[如何安装和配置 Azure PowerShell](../install-configure-powershell.md)在您的本地计算机上安装 Azure PowerShell。 打开 Azure PowerShell 提示符。

接下来，选择正确的 Azure 订阅使用这些命令。 引号，包括的所有内容替换 < 和 > 字符，用正确的名称。

    $subscr="<Subscription name>"
    Select-AzureSubscription -SubscriptionName $subscr –Current

您可以订阅名称从**Get AzureSubscription**命令显示的 SubscriptionName 属性。

接下来，切换到资源管理器模式的 Azure PowerShell。

    Switch-AzureMode AzureResourceManager 

接下来，创建新的资源组的基本配置测试实验室。 要确定一个唯一的资源组名称，请使用以下命令列出您现有的资源组。

    Get-AzureResourceGroup | Sort ResourceGroupName | Select ResourceGroupName

要列出的 Azure 的位置，您可以在其中创建基于资源管理器的虚拟机，请使用这些命令。

    $loc=Get-AzureLocation | where { $_.Name –eq "Microsoft.Compute/virtualMachines" }
    $loc.Locations

使用这些命令创建新的资源组。 引号，包括的所有内容替换 < 和 > 字符，使用正确的名称。

    $rgName="<resource group name>"
    $locName="<location name, such as West US>"
    New-AzureResourceGroup -Name $rgName -Location $locName

基于资源管理器的虚拟机需要基于资源管理器的存储帐户。 您必须选择一个全局唯一名称为您的存储帐户中只包含小写字母和数字。 可以使用此命令可以列出现有的存储帐户。

    Get-AzureStorageAccount | Sort Name | Select Name

若要测试是否选择的存储帐户名称的全局唯一性，需要 PowerShell 的 Azure 服务管理模式下运行**测试 AzureName**命令。 使用这些命令。

    Switch-AzureMode AzureServiceManagement
    Test-AzureName -Storage <Proposed storage account name>

如果测试 AzureName 命令显示**False**，您建议的名称是唯一的。 您就能确定一个唯一的名称，Azure PowerShell 重新切换到资源管理器模式下使用此命令。

    Switch-AzureMode AzureResourceManager 

使用这些命令创建新的存储帐户为您新的测试环境。

    $rgName="<your new resource group name>"
    $locName="<the location of your new resource group>"
    $saName="<storage account name>"
    New-AzureStorageAccount -Name $saName -ResourceGroupName $rgName –Type Standard_LRS -Location $locName

接下来，您将创建测试实验室 Azure 虚拟网络将承载的基本配置的企业网络子网。

    $rgName="<name of your new resource group>"
    $locName="<Azure location name, such as West US>"
    $corpnetSubnet=New-AzureVirtualNetworkSubnetConfig -Name Corpnet -AddressPrefix 10.0.0.0/24
    New-AzurevirtualNetwork -Name TestLab -ResourceGroupName $rgName -Location $locName -AddressPrefix 10.0.0.0/8 -Subnet $corpnetSubnet –DNSServer 10.0.0.4

这是您的当前配置。

![](./media/virtual-machines-base-configuration-test-environment-resource-manager/BC_TLG01.png)

## 阶段 2︰ 配置 DC1

DC1 是 corp.contoso.com Active Directory 域服务 (AD DS) 域的域控制器和 DNS 服务器的虚拟机的测试实验室的虚拟网络。

首先，填入您的资源组、 Azure 位置和存储帐户名称和本地计算机上创建 DC1 Azure 虚拟机在 Azure PowerShell 命令提示符下运行这些命令。

    $rgName="<resource group name>"
    $locName="<Azure location, such as West US>"
    $saName="<storage account name>"
    $vnet=Get-AzurevirtualNetwork -Name TestLab -ResourceGroupName $rgName
    $pip = New-AzurePublicIpAddress -Name DC1-NIC -ResourceGroupName $rgName -Location $locName -AllocationMethod Dynamic
    $nic = New-AzureNetworkInterface -Name DC1-NIC -ResourceGroupName $rgName -Location $locName -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id -PrivateIpAddress 10.0.0.4
    $vm=New-AzureVMConfig -VMName DC1 -VMSize Standard_A1
    $storageAcc=Get-AzureStorageAccount -ResourceGroupName $rgName -Name $saName
    $vhdURI=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/DC1-TestLab-ADDSDisk.vhd"
    Add-AzureVMDataDisk -VM $vm -Name ADDS-Data -DiskSizeInGB 20 -VhdUri $vhdURI  -CreateOption empty
    $cred=Get-Credential -Message "Type the name and password of the local administrator account for DC1." 
    $vm=Set-AzureVMOperatingSystem -VM $vm -Windows -ComputerName DC1 -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    $vm=Set-AzureVMSourceImage -VM $vm -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2012-R2-Datacenter -Version "latest"
    $vm=Add-AzureVMNetworkInterface -VM $vm -Id $nic.Id
    $osDiskUri=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/DC1-TestLab-OSDisk.vhd"
    $vm=Set-AzureVMOSDisk -VM $vm -Name DC1-TestLab-OSDisk -VhdUri $osDiskUri -CreateOption fromImage
    New-AzureVM -ResourceGroupName $rgName -Location $locName -VM $vm

接下来，连接到 DC1 虚拟机。

1.  在 Azure 预览门户中，在左窗格中单击**浏览全部**，在**浏览**列表中，单击**虚拟机**，然后单击**DC1**虚拟机。  
2.  **DC1**窗格、 中单击**连接**。
3.  出现提示时，请打开下载的 DC1.rdp 文件。
4.  当系统提示您使用远程桌面连接消息框，单击**连接**。
5.  当系统提示您输入凭据时，使用以下命令︰
- 名称︰ **DC1\\**[本地管理员帐户名称]
- 密码: [本地管理员帐户密码]
6.  证书引用一个远程桌面连接消息框提示您，请单击**是**。

作为一个新卷驱动器号 f︰ 接下来，添加额外的数据磁盘。

1.  在服务器管理器的左窗格中，单击**文件和存储服务**，然后单击**磁盘**。
2.  在内容窗格中的**磁盘**组中，单击**磁盘 2** （设置为**未知****分区**）。
3.  单击**任务**，然后单击**新建卷**。
4.  之前在您开始新建卷向导页，单击**下一步**。
5.  在选择服务器和磁盘页面上，单击**磁盘 2**，然后单击**下一步**。 出现提示时，单击**确定**。
6.  在指定卷页的大小，单击**下一步**。
7.  在分配给驱动器号或文件夹页面上，单击**下一步**。
8.  在选择文件系统设置页上，单击**下一步**。
9.  在确认选项页中，单击**创建**。
10. 完成后，单击**关闭**。

接下来，将 dc1，将其配置为域控制器和 DNS 服务器的 corp.contoso.com 域。 在管理员级别的 Windows PowerShell 命令提示符运行这些命令。

    Install-WindowsFeature AD-Domain-Services -IncludeManagementTools
    Install-ADDSForest -DomainName corp.contoso.com -DatabasePath "F:\NTDS" -SysvolPath "F:\SYSVOL" -LogPath "F:\Logs"

Dc1，将其重新启动后，重新连接到 DC1 虚拟机。

1.  在 Azure 预览门户中，在左窗格中单击浏览全部，请单击浏览列表中的虚拟机，然后单击 DC1 虚拟机。
2.  DC1 的窗格中，单击连接。
3.  当系统提示您打开 DC1.rdp，请单击**打开**。
4.  当系统提示您使用远程桌面连接消息框，单击**连接**。
5.  当系统提示您输入凭据时，使用以下命令︰
- 名称︰ **CORP\\**[本地管理员帐户名称]
- 密码: [本地管理员帐户密码]
6.  当提示您的证书引用一个远程桌面连接消息框，请单击**是**。

接下来，创建一个用户帐户，登录到 CORP 域成员计算机时将使用的 Active Directory 中。 每次在管理员级别的 Windows PowerShell 命令提示符运行这些命令。

    New-ADUser -SamAccountName User1 -AccountPassword (read-host "Set user password" -assecurestring) -name "User1" -enabled $true -PasswordNeverExpires $true -ChangePasswordAtLogon $false
    Add-ADPrincipalGroupMembership -Identity "CN=User1,CN=Users,DC=corp,DC=contoso,DC=com" -MemberOf "CN=Enterprise Admins,CN=Users,DC=corp,DC=contoso,DC=com","CN=Domain Admins,CN=Users,DC=corp,DC=contoso,DC=com"

请注意第一个命令结果中提供用户 1 的提示帐户密码。 由于此帐户将用于所有 CORP 域成员计算机的远程桌面连接，请选择强密码。 要检查其强度，请查看[密码检查器︰ 使用强密码](https://www.microsoft.com/security/pc-security/password-checker.aspx)。 记录的 User1 帐户密码并将其存储在安全的位置。

关闭与 DC1 的远程桌面会话，然后重新连接使用的 CORP\User1 帐户。

接下来，要允许 Ping 工具的通讯，此命令管理员级 Windows PowerShell 命令提示符处运行。

    Set-NetFirewallRule -DisplayName "File and Printer Sharing (Echo Request - ICMPv4-In)" -enabled True

这是您的当前配置。

![](./media/virtual-machines-base-configuration-test-environment-resource-manager/BC_TLG02.png)

## 阶段 3︰ 配置 APP1

APP1 提供 web 和文件共享服务。

首先，填入您的资源组、 Azure 位置和存储帐户名称和本地计算机上创建 APP1 Azure 虚拟机在 Azure PowerShell 命令提示符下运行这些命令。

    $rgName="<resource group name>"
    $locName="<Azure location, such as West US>"
    $saName="<storage account name>"
    $vnet=Get-AzurevirtualNetwork -Name TestLab -ResourceGroupName $rgName
    $pip = New-AzurePublicIpAddress -Name APP1-NIC -ResourceGroupName $rgName -Location $locName -AllocationMethod Dynamic
    $nic = New-AzureNetworkInterface -Name APP1-NIC -ResourceGroupName $rgName -Location $locName -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id
    $vm=New-AzureVMConfig -VMName APP1 -VMSize Standard_A1
    $storageAcc=Get-AzureStorageAccount -ResourceGroupName $rgName -Name $saName
    $cred=Get-Credential -Message "Type the name and password of the local administrator account for APP1." 
    $vm=Set-AzureVMOperatingSystem -VM $vm -Windows -ComputerName APP1 -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    $vm=Set-AzureVMSourceImage -VM $vm -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2012-R2-Datacenter -Version "latest"
    $vm=Add-AzureVMNetworkInterface -VM $vm -Id $nic.Id
    $osDiskUri=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/APP1-TestLab-OSDisk.vhd"
    $vm=Set-AzureVMOSDisk -VM $vm -Name APP1-TestLab-OSDisk -VhdUri $osDiskUri -CreateOption fromImage
    New-AzureVM -ResourceGroupName $rgName -Location $locName -VM $vm

接下来，连接到 app1 发出虚拟机使用 APP1 本地管理员帐户名和密码，然后再打开管理员级别的 Windows PowerShell 命令提示符。

若要检查 APP1 和 DC1 之间的名称解析和网络通信，请运行**ping dc1.corp.contoso.com**命令，验证存在四个答复。

接下来，加入 CORP 域中使用这些命令在 Windows PowerShell 提示符 app1 发出虚拟机。

    Add-Computer -DomainName corp.contoso.com
    Restart-Computer

请注意，您必须输入添加计算机命令后提供您 CORP\User1 域帐户的凭据。

App1 发出重新启动后，连接到它的 CORP\User1 帐户名和密码，使用，然后打开管理员级别的 Windows PowerShell 命令提示符。

接下来，请 APP1 web 服务器使用此命令在 Windows PowerShell 命令提示符。

    Install-WindowsFeature Web-WebServer –IncludeManagementTools

接下来，创建一个共享的文件夹和文件夹中的文本文件 APP1 上使用这些命令。

    New-Item -path c:\files -type directory
    Write-Output "This is a shared file." | out-file c:\files\example.txt
    New-SmbShare -name files -path c:\files -changeaccess CORP\User1

这是您的当前配置。

![](./media/virtual-machines-base-configuration-test-environment-resource-manager/BC_TLG03.png)

## 阶段 4︰ 将 CLIENT1 配置

客户端 1 作为典型的便携式电脑、 平板电脑或 Contoso intranet 上的桌面计算机。

首先，填入您的资源组、 Azure 位置和存储帐户名称和本地计算机上创建的客户端 1 Azure 虚拟机在 Azure PowerShell 命令提示符下运行这些命令。

    $rgName="<resource group name>"
    $locName="<Azure location, such as West US>"
    $saName="<storage account name>"
    $vnet=Get-AzurevirtualNetwork -Name TestLab -ResourceGroupName $rgName
    $pip = New-AzurePublicIpAddress -Name CLIENT1-NIC -ResourceGroupName $rgName -Location $locName -AllocationMethod Dynamic
    $nic = New-AzureNetworkInterface -Name CLIENT1-NIC -ResourceGroupName $rgName -Location $locName -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id
    $vm=New-AzureVMConfig -VMName CLIENT1 -VMSize Standard_A1
    $storageAcc=Get-AzureStorageAccount -ResourceGroupName $rgName -Name $saName
    $cred=Get-Credential -Message "Type the name and password of the local administrator account for CLIENT1." 
    $vm=Set-AzureVMOperatingSystem -VM $vm -Windows -ComputerName CLIENT1 -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    $vm=Set-AzureVMSourceImage -VM $vm -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2012-R2-Datacenter -Version "latest"
    $vm=Add-AzureVMNetworkInterface -VM $vm -Id $nic.Id 
    $osDiskUri=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/CLIENT1-TestLab-OSDisk.vhd"
    $vm=Set-AzureVMOSDisk -VM $vm -Name CLIENT1-TestLab-OSDisk -VhdUri $osDiskUri -CreateOption fromImage
    New-AzureVM -ResourceGroupName $rgName -Location $locName -VM $vm

接下来，连接到客户端 1 虚拟机使用的客户端 1 本地管理员帐户名和密码，然后再打开管理员级 Windows PowerShell 命令提示符。

要检查 CLIENT1 DC1 之间的名称解析和网络通信，请在 Windows PowerShell 命令提示符运行**ping dc1.corp.contoso.com**命令并验证存在四个答复。

接下来，加入 CORP 域中使用这些命令在 Windows PowerShell 提示符 CLIENT1 虚拟机。

    Add-Computer -DomainName corp.contoso.com
    Restart-Computer

请注意，您必须输入添加计算机命令后提供您 CORP\User1 域帐户的凭据。

客户端 1 重新启动后，连接到它的 CORP\User1 帐户名和密码，使用，然后打开管理员级别的 Windows PowerShell 命令提示符。

接下来，验证可以从客户端 1 访问 APP1 上的网站和文件共享资源。

1.  在服务器管理器中，在树窗格中，单击**本地服务器**命令。
2.  在**客户端 1 的属性**中，单击**在**旁边**IE 增强的安全配置**。
3.  在**Internet Explorer 增强的安全配置**，**管理员**和**用户**，单击**关闭**，然后单击**确定**。
4.  从开始屏幕中，单击**Internet Explorer**，，然后单击**确定**。
5.  在地址栏中键入**http://app1.corp.contoso.com/**，，然后按 enter 键。  您应该看到 APP1 默认 Internet Information Services 网页。
6.  从桌面任务栏中，单击文件资源管理器图标。
7.  在地址栏中，键入**\\\app1\Files**，然后按 enter 键。
8.  您会看到文件的共享文件夹中的内容的文件夹窗口。
9.  在**文件**共享的文件夹窗口中，双击**Example.txt**文件。 您应该看到 Example.txt 文件的内容。
10. **Example.txt-记事本**和共享**文件**文件夹窗口关闭。

这是您最终的配置。

![](./media/virtual-machines-base-configuration-test-environment-resource-manager/BC_TLG04.png)

在 Azure 中的基本配置现在就为应用程序开发和测试或其他测试环境。

## 其他资源

[混合云测试环境](../virtual-network/virtual-networks-setup-hybrid-cloud-environment-testing.md)

[基本配置测试环境](virtual-machines-base-configuration-test-environment.md)


## <a id="costs"></a>测试环境在 Azure 中的虚拟机的成本降至最低

若要运行测试的环境的虚拟机的成本降到最低，可以执行以下任一操作︰

- 创建测试环境，并尽可能快地执行您需要测试和演示。 完成后，删除测试环境的虚拟机。
- 关闭测试环境的虚拟机，为释放状态。

若要关闭虚拟机使用 Azure PowerShell，填写的资源组名称和运行这些命令。

    $rgName="<your resource group name>"
    Stop-AzureVM -ResourceGroupName $rgName -Name "CLIENT1"
    Stop-AzureVM -ResourceGroupName $rgName -Name "APP1"
    Stop-AzureVM -ResourceGroupName $rgName -Name "DC1" -Force -StayProvisioned

若要确保您的虚拟机工作正常时从所有这些已停止 (Deallocated) 状态，应按以下顺序启动它们︰

1.  DC1
2.  APP1
3.  客户端 1

若要按顺序使用 Azure PowerShell 启动虚拟机，填写的资源组名称和运行这些命令。

    $rgName="<your resource group name>"
    Start-AzureVM -ResourceGroupName $rgName -Name "DC1"
    Start-AzureVM -ResourceGroupName $rgName -Name "APP1"
    Start-AzureVM -ResourceGroupName $rgName -Name "CLIENT1"
