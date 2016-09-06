---
ms.openlocfilehash: 47d5985eec7c278b281205e0583534194c3a51b2
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="如何配置虚拟机设置可用性"
    description="提供配置设置为新的或现有虚拟机在 Azure 使用 Azure 门户和 Azure PowerShell 命令可用性的步骤"
    services="virtual-machines"
    documentationCenter=""
    authors="KBDAzure"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-multiple"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/10/2015"
    ms.author="kathydav"/>

#如何配置虚拟机设置可用性#

可用性设置有助于保持虚拟机可在停机时间，如在维护期间。 可用性组中放置两个或更多同样配置的虚拟机创建维护的应用程序或运行虚拟机的服务可用性所需的冗余。 这是如何工作的详细信息，请参阅[管理虚拟机的可用性] []。

它是一种最佳做法，使用集可用性和负载平衡的端点来帮助确保您的应用程序始终可用并且正在运行高效。 有关负载平衡的端点详细信息，请参阅[负载平衡 Azure 的基础结构服务] []。

您可以将虚拟机投入使用两个选项之一来设置可用性︰

- [选项 1︰ 创建虚拟机并在同一时间设置可用性] []。 然后，到集添加新的虚拟机，当您创建这些虚拟机。
- [选项 2︰ 将现有虚拟机添加到可用性组] []。

>[AZURE.NOTE] 要放置在同一可用性组中的虚拟机必须属于同一个云服务。

## <a id="createset"> </a>选项 1︰ 创建虚拟机并在同一时间设置可用性##

可以使用 Azure 的门户网站或 Azure PowerShell 命令来执行此操作。

若要使用门户网站︰

1. 如果您还没有登录到[门户](http://manage.windowsazure.com)的这么做。

2. 在命令栏上，单击**新建**。

3. 单击**虚拟机**，然后单击**剪辑库中**。

4. 使用前两个屏幕来选择图像、 用户名和密码，等等。 有关详细信息，请参阅[创建虚拟机运行 Windows][]。

5. 在第三个屏幕上，您可以配置的网络、 存储和可用性的资源。 请执行以下操作︰

    1. 选择适当的云服务。 请将其设置为**创建新的云服务**（除非此新的虚拟机添加到现有的虚拟机云服务）。 然后，在**云服务的 DNS 名称**，键入名称。 DNS 名称将成为用于虚拟机，请与联系 URI 的一部分。 云服务作为通信和隔离组。 在同一个云服务中的所有虚拟机可以相互通信，可以设置为负载平衡，可以放在同一个可用性组。

    2. **地区/关系组/虚拟网络**下, 指定虚拟网络，如果您计划使用一个。 **重要提示**︰ 如果您希望虚拟机使用的虚拟网络，则必须连接虚拟机的虚拟网络创建虚拟机时。 创建虚拟机后，不能加入虚拟机与虚拟网。 有关详细信息，请参阅[虚拟网络概述][]。

    3. 创建可用性组。 **可用性设置**，请在下将它设置为**创建的可用性设置**。 然后，键入组的名称。

    4. 创建默认终结点，并根据需要添加更多的终结点。 也可以稍后添加终结点。

    ![创建新的虚拟机设置可用性](./media/virtual-machines-how-to-configure-availability/VMavailabilityset.png)

6. 在第四个屏幕上，单击您想要安装的扩展。 扩展提供功能，使其能够更轻松地管理虚拟机，如运行反恶意软件或重置密码。 有关详细信息，请参阅[Azure VM 代理和虚拟机的扩展](virtual-machines-extensions-agent-about.md)。

7.  单击箭头可创建虚拟机和可用性设置。

    从新的虚拟机的控制板，您可以单击**配置**虚拟机属于新的可用性设置。

若要使用 Azure PowerShell 命令创建 Azure 的虚拟机并将其添加到新的或现有的可用性，请参阅以下︰

- [使用 Azure PowerShell 创建和预配置的基于 Windows 的虚拟机](virtual-machines-ps-create-preconfigure-windows-vms.md)
- [使用 Azure PowerShell 创建和预配置的基于 Linux 的虚拟机](virtual-machines-ps-create-preconfigure-linux-vms.md)

## <a id="addmachine"> </a>选项 2︰ 将现有虚拟机添加到可用性设置##

在门户中，您可以添加现有的虚拟机，对现有的可用性设置或创建一个新的。 （请记住，相同的可用性组中的虚拟机必须属于同一个云服务）。是几乎相同的步骤。 使用 Azure PowerShell，您可以添加一组现有的可用性的虚拟机。

1. 如果您还没有，请登录到[门户网站](http://manage.windowsazure.com)。

2. 在命令栏上，单击**虚拟机**。

3. 从虚拟机的列表中，选择您想要添加到组中的虚拟机的名称。

4. 从以下虚拟机名称的选项卡，单击**配置**。

5. 在设置部分中找到**的可用性设置**。 执行下列任一操作︰

    答︰ 选择**创建可用性设置**，然后键入组的名称。

    B。 选择**选择可用性组**，然后从列表中选择一组。

    ![创建为现有的虚拟机设置可用性](./media/virtual-machines-how-to-configure-availability/VMavailabilityExistingVM.png)

6. 单击**保存**。

若要使用 Azure PowerShell 命令，打开管理员级 Azure PowerShell 会话并运行下面的命令。 占位符 (如&lt;VmCloudServiceName&gt;)，替换引号，包括一切 < 和 > 字符，使用正确的名称。

    Get-AzureVM -ServiceName "<VmCloudServiceName>" -Name "<VmName>" | Set-AzureAvailabilitySet -AvailabilitySetName "<AvSetName>" | Update-AzureVM

>[AZURE.NOTE] 虚拟机可能需要重新启动以完成将其添加到的可用性设置。

##其他资源

[关于 Azure 的虚拟机配置设置]

<!-- LINKS -->
[选项 1︰ 创建虚拟机并在同一时间设置可用性]: #createset
[选项 2︰ 将现有虚拟机添加到可用性设置]: #addmachine

[负载平衡 Azure 的基础结构服务]: virtual-machines-load-balance.md
[管理虚拟机的可用性]: virtual-machines-manage-availability.md
[创建一个运行 Windows 虚拟机]: virtual-machines-windows-tutorial.md
[虚拟网络概述]: virtual-networks-overview.md
[关于 Azure 的虚拟机配置设置]: http://msdn.microsoft.com/library/azure/dn763935.aspx
