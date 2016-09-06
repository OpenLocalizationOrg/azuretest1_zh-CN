---
ms.openlocfilehash: a587d43a315356adbc18a2bdb4ab5c33bac74cb0
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
1. 登录到[Azure 的门户](http://manage.windowsazure.com)。 如果您没有订阅尚未，检出[免费试用](http://azure.microsoft.com/pricing/free-trial/)优惠。

2. 在窗口底部的命令栏中，单击**新建**。

3. 在**计算**，单击**虚拟机**，，然后单击**剪辑库中**。

    ![从库中命令栏导航到](./media/virtual-machines-create-WindowsVM/fromgallery.png)

4. 在这之后的第一个屏幕可以为虚拟机可用的图像中**选择图像**。 （可用图像可能不同具体取决于您正在使用的订阅。

5. 第二个屏幕，您可以选择计算机名称、 大小、 和管理权限的用户名和密码。 使用层和运行您的应用程序或工作负载所需的大小。 以下是一些提示︰

    - **新的用户名称**是指用来管理服务器的管理帐户。 创建唯一的密码，此帐户，请务必记住它。 **您将需要的用户名和密码以登录到虚拟机**。

    - 虚拟机的大小会影响的使用情况，以及如多少数据磁盘的配置选项可以将附加的成本。 有关详细信息，请参阅[虚拟机大小](../articles/virtual-machines-size-specs.md)。

6. 第三个屏幕允许您配置的网络、 存储和可用性的资源。 以下是一些提示︰

    - **云服务的 DNS 名称**是全局 DNS 名称组成部分的 URI，用于虚拟机，请与联系。 您将需要使用您自己的云服务名称提出，因为它必须在 Azure 中唯一。 云服务是非常重要的情况下使用[多个虚拟机](../articles/cloud-services-connect-virtual-machine.md)。

    - 对于**区域中的相似性组/虚拟网络**，使用适用于您所在位置的区域。 您还可以选择改为指定的虚拟网络。

    >[AZURE.NOTE] 创建虚拟机时，如果您希望虚拟机使用的虚拟网络，您**必须**指定虚拟网络。 创建虚拟机后，不能加入虚拟机与虚拟网。 有关详细信息，请参阅[Azure 虚拟网络概述](virtual-networks-overview.md)。
    >
    > 有关配置终结点的详细信息，请参阅[如何设置设置终结点的虚拟机](../articles/virtual-machines-set-up-endpoints.md)。

7. 第四个配置屏幕允许您安装虚拟机代理配置某些可用的扩展。

    >[AZURE.NOTE] 该 VM 代理提供安装扩展，可帮助您与之交互或管理虚拟机的环境。 有关详细信息，请参阅[有关 VM 代理和扩展](virtual-machines-extensions-agent-about.md)。  

8. 创建虚拟机后，门户将列出新的虚拟机**虚拟机**下。 相应的云服务和存储帐户还创建，并且在这些部分中列出。 同时自动启动虚拟机和云服务，并且其状态被列为**运行**。

    ![配置虚拟机代理和虚拟机的终结点](./media/virtual-machines-create-WindowsVM/vmcreated.png)
