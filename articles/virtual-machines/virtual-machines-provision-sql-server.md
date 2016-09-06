---
ms.openlocfilehash: 2d9b6e76eb27d9fcf6fb13a69752c950ba55d843
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="设置 Azure 中的 SQL Server 虚拟机" 
    description="本教程将教您如何创建并配置 SQL Server 虚拟机在 Azure 上。" 
    services="virtual-machines" 
    documentationCenter="" 
    authors="rothja" 
    manager="jeffreyg" 
    editor="monicar"/>

<tags 
    ms.service="virtual-machines" 
    ms.workload="infrastructure-services" 
    ms.tgt_pltfrm="vm-windows-sql-server" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/26/2015" 
    ms.author="jroth"/>

# 设置 Azure 中的 SQL Server 虚拟机

> [AZURE.SELECTOR]
- [门户](virtual-machines-provision-sql-server.md)
- [PowerShell](virtual-machines-sql-server-create-vm-with-powershell.md)

## 概述

Azure 虚拟机库包括包含 Microsoft SQL Server 的多个图像。 您可以从库中选择一个虚拟机映像，并几次单击即可调配到 Azure 环境虚拟机。

在本教程中，您将能够︰

* [连接到 Azure 管理门户和调配资源库中的虚拟机](#Provision)
* [打开虚拟机使用远程桌面和完成安装](#RemoteDesktop)
* [完成连接到其他计算机上使用 SQL Server 管理 Studio 虚拟机的配置步骤](#SSMS)
* [下一步行动](#Optional)

##<a id="Provision">提供 SQL Server 虚拟机从库</a>

1. 登录到[Azure 管理门户](http://manage.windowsazure.com)使用您的帐户中。 如果您没有一个 Azure 帐户，请访问[Azure 免费试用版](http://azure.microsoft.com/pricing/free-trial/)。

2. 在 Azure 管理门户网站，在左下角的网页上，单击**+ 新建****计算**单击**虚拟机**，和，然后单击**从库**。

3. 在**选择图像**页中，单击**SQL 服务器**。 然后选择一个 SQL Server 的图像。 单击页面的底部右侧的下一个箭头。 

    ![选择图像](./media/virtual-machines-provision-sql-server/choose-sql-vm.png)

在 Azure 上受支持的 SQL Server 图像上的最新信息，请参阅[SQL Server 在 Azure 虚拟机概述](virtual-machines-sql-server-infrastructure-services.md)。

>[AZURE.NOTE] 如果您有通过平台映像 SQL Server 评估版创建的虚拟机，不能将其升级到每分钟付费的版图像库中。 您可以选择以下两个选项之一︰
>
> - 可以使用每分钟支付库中的 SQL Server 版本创建新的虚拟机并将数据库文件迁移到此新的虚拟机，在[迁移到 Azure VM 上的 SQL Server 数据库](virtual-machines-migrate-onpremises-database)的步骤
> - 或者，您可以升级现有实例的 SQL Server 评估版到 SQL Server 的不同版本[许可证移动通过在 Azure 上的软件保证](http://azure.microsoft.com/pricing/license-mobility/)协议下在[升级到 SQL Server 不同版本](https://msdn.microsoft.com/library/cc707783.aspx)的步骤。 有关如何购买的 SQL Server 授权的副本的信息，请参阅[如何购买 SQL Server](http://www.microsoft.com/sqlserver/get-sql-server/how-to-buy.aspx)。

4. 在第一个**虚拟机配置**页上，提供了以下信息︰
    - **版本发布日期**。 多个图像是否可用，请选择最新的。
    - 唯一的**虚拟机的名称**。
    - 在**新用户名**中，在计算机的本地管理员帐户的唯一的用户名称。
    - 在**新密码**框中，键入一个强密码。 
    - 在**确认密码**框中，重新键入密码。
    - 从下拉列表中选择适当的**大小**。 

    ![虚拟机配置](./media/virtual-machines-provision-sql-server/4VM-Config.png)

    >[AZURE.NOTE] 在资源调配过程中指定虚拟机的大小︰
    >
    > - A2 是建议用于生产工作负载的最小大小。 
    > - 当使用 SQL Server 企业版时，虚拟机的最小建议是 A3。
    > - 选择 A3 或使用 SQL Server 企业版时更高。
    > - 选择 A4 或更高，当使用 SQL Server 2012年或 2014年企业优化事务性工作负载的图像。  
    > - 选择 A7 或使用 SQL Server 2012年或 2014年企业优化的数据仓库工作负载图像时更高。 
    > - 为了获得最佳性能与高级存储中使用 DS2 或 DS3。 有关详细信息，请参阅[SQL Server 在 Azure 的虚拟机的性能最佳](virtual-machines-sql-server-performance-best-practices.md)。
    > - 所选的大小限制可以配置的数据磁盘的数量。 有关可用的虚拟机大小和数量的数据磁盘，您可以附加到虚拟机的最新信息，请参阅[Azure 的虚拟机大小](virtual-machines-size-specs.md)。

5. 后输入 VM 配置详细信息，请单击右下方继续上的下箭头。

5. 在第二个**虚拟机配置**页上，将配置的网络、 存储和可用性的资源︰
    - 在**云服务**框中，选择**创建新的云服务**。
    - 在**云服务的 DNS 名称**框中，提供您的选择，DNS 名称的第一部分，以便完成**TESTNAME.cloudapp.net**的格式的名称 
    - 如果您有多个可供选择的订阅，请选择**订阅**。 选择确定哪些**存储帐户**可用。
    - 在**区域中的相似性组/虚拟网络**框中，选择该虚拟映像所在的区域。
    - 在**存储帐户**，自动生成一个帐户，或从列表中选择一个。 更改的**订阅**以查看更多帐户。 
    - 在**可用性设置**框中，选择**（无）**。
    - 阅读并接受法律的条款。
    

6. 单击下一步以继续。


7. 单击右下角继续中的复选标记。

8. 请等待 Azure 准备您的虚拟机。 希望继续通过虚拟机状态︰

    - **正在开始 （设置）**
    - **已停止**
    - **正在开始 （设置）**
    - **运行 （资源调配）**
    - **运行**
    

##<a id="RemoteDesktop">打开 VM 使用远程桌面以完成安装</a>

1. 在资源调配完成后，请单击虚拟机，请转至仪表板页的名称。 在页面的底部，单击**连接**。

2. 单击**打开**按钮。

    ![单击打开按钮](./media/virtual-machines-provision-sql-server/click-open-to-connect.png)

3. 在**Windows 安全**对话框中，单击**使用另一个帐户**。

    ![单击使用另一个帐户](./media/virtual-machines-provision-sql-server/credentials.png) 

4. 这台计算机的名称用作域名，跟您在此格式中的管理员名称︰ `machinename\username`。 键入您的密码并连接到计算机。

4. 有几个进程将完成第一次登录的时，，包括桌面、 Windows 更新和 Windows 初始配置任务 (sysprep) 完成安装程序。 Windows sysprep 完成后，SQL Server 安装程序将完成的配置任务。 这些任务使他们完成而导致几分钟的延迟。 `SELECT @@SERVERNAME` 直到 SQL Server 安装程序完成后，SQL Server 管理 Studio 可能不在起始页上可见，可能不会返回正确的名称。

一旦您连接到使用 Windows 远程桌面虚拟机，该虚拟机工作方式极其类似的任何其他计算机。 以正常方式连接到 SQL Server 管理 studio （虚拟机上运行） 的 SQL Server 默认实例。 

##<a id="SSMS">在另一台计算机上从 SSMS 连接到 SQL Server VM 实例</a>

[AZURE.INCLUDE [Connect to SQL Server in a VM](../../includes/virtual-machines-sql-server-connection-steps.md)]

## <a id="cdea">从您的应用程序连接到数据库引擎</a>

如果您可以连接到使用管理 Studio Azure 的虚拟机上运行的 SQL Server 实例，您应该能够通过使用类似于下面的连接字符串连接。

    connectionString = "Server=tutorialtestVM.cloudapp.net,57500;Integrated Security=false;User ID=<login_name>;Password=<your_password>"

有关详细信息，请参阅[如何解决连接到 SQL Server 数据库引擎](http://social.technet.microsoft.com/wiki/contents/articles/how-to-troubleshoot-connecting-to-the-sql-server-database-engine.aspx)。

##<a id="Optional">下一步行动</a>

您已经看到如何创建并配置 SQL Server Azure 使用平台映像的虚拟机上。 在许多情况下下, 一步是将您的数据库迁移到此新的 SQL Server 虚拟机。 数据库迁移指南，请参阅[迁移到 Azure VM 上的 SQL Server 数据库](virtual-machines-migrate-onpremises-database.md)。

下面的列表提供额外的资源为 SQL Server 在 Azure 的虚拟机中。

### 建议的用于在 Azure Vm 上的 SQL Server 资源︰
- [SQL Server 在 Azure 虚拟机概述](virtual-machines-sql-server-infrastructure-services.md)

- [连接到 SQL Server 虚拟机在 Azure 上](virtual-machines-sql-server-connectivity.md)

- [在 Azure 的虚拟机中的 SQL Server 的性能最佳做法](virtual-machines-sql-server-performance-best-practices.md)

- [SQL Server 在 Azure 的虚拟机中的安全注意事项](virtual-machines-sql-server-security-considerations.md)

### 高可用性和灾难恢复︰
- [高可用性和灾难恢复 SQL Server 在 Azure 的虚拟机中](virtual-machines-sql-server-high-availability-and-disaster-recovery-solutions.md)

- [SQL Server 在 Azure 的虚拟机的备份和恢复](virtual-machines-sql-server-backup-and-restore.md)

### 在 Azure 中的 SQL Server 工作负荷︰
- [SQL Server 在 Azure 的虚拟机中的商业智能](virtual-machines-sql-server-business-intelligence.md)

### 白皮书︰
- [了解 SQL Azure 数据库和 SQL Server 在 Azure 的虚拟机中](sql-database/data-management-azure-sql-database-and-sql-server-iaas.md)

- [应用程序模式和 SQL Server 在 Azure 的虚拟机中的开发策略](virtual-machines-sql-server-application-patterns-and-development-strategies.md)
