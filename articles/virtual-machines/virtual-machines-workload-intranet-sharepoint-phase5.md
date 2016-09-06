---
ms.openlocfilehash: 4b57f490beed481fcf2fb3d2a4a06f7e2c41935a
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="SharePoint Server 2013 场阶段 5 |Microsoft Azure"
    description="创建可用性组并向其中添加 SharePoint 数据库，SharePoint Server 2013 场 Azure 中的阶段 5 中。"
    documentationCenter=""
    services="virtual-machines"
    authors="JoeDavies-MSFT"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/21/2015"
    ms.author="josephd"/>

# SharePoint Intranet 服务器场工作负荷阶段 5︰ 创建可用性组并添加 SharePoint 数据库

在此部署仅用于内部网的 SharePoint 2013 场与 SQL Server AlwaysOn 可用性组在 Azure 的基础结构服务的最后阶段中，您创建一个新的 AlwaysOn 可用性组和添加 SharePoint 服务器场的数据库。

请参阅[使用 Azure 中的 SQL Server AlwaysOn 可用性组部署 SharePoint](virtual-machines-workload-intranet-sharepoint-overview.md)所有阶段。

## 创建可用性组并添加数据库

SharePoint 作为初始配置的一部分创建多个数据库。 通过这些步骤，必须准备好这些数据库︰

1.  在主计算机上执行完全备份和事务日志备份的数据库。
2.  还原完整和日志备份计算机上的备份。

一旦数据库已同时备份和还原，它们可以被添加到可用性组。 SQL Server 允许已 （至少一次），备份和还原到另一台计算机，可在组中的数据库。

### 共享备份文件夹

若要使备份和恢复操作，请确保备份 (.bak) 文件从辅助 SQL Server 虚拟机可以访问。 请执行以下步骤︰

1.  **[域] \sp_farm_db**登录到主 SQL Server 主机上。
2.  导航到 F:\ 磁盘。
3.  用鼠标右键单击**备份**文件夹，单击**共享**，然后单击**特定人群**。
4.  在**文件共享**对话框中，键入**[域] \sqlservice**，，然后单击**添加**。
5.  请单击**权限级别**列**sqlservice**帐户名，然后单击**读/写**。
6.  单击**共享**，然后单击**完成**。

除了向 sqlservice 帐户在步骤 5 中的 F:\Backup 文件夹的**读取**权限授予辅助 SQL Server 主机上执行上述过程。

### 备份和还原数据库

需要添加到可用性组的每个数据库都必须重复以下过程。 一些 SharePoint 2013 数据库不支持 SQL Server AlwaysOn 可用性组。 有关详细信息，请参阅[支持高可用性和灾难恢复选项的 SharePoint 数据库](http://technet.microsoft.com/library/jj841106.aspx)。

使用下列步骤来备份数据库︰

1.  在主要的 SQL Server 的计算机的开始屏幕中，键入**SQL Studio**，，然后单击**SQL Server 管理 Studio**。
2.  单击**连接**。
3.  在左窗格中，展开**数据库**节点。
4.  用鼠标右键单击要备份，指向**任务**，然后单击**备份**的数据库。
5.  在**目标**部分中，单击**删除**删除备份文件的默认文件路径。
6.  单击**添加**。 在**文件名**中，键入**\\[计算机名] \backup\[数据库名称].bak**，其中计算机名是主 SQL Server 的计算机的名称和数据库名称为数据库的名称。 单击**确定**，然后成功备份有关的消息后，再次单击**确定**。
7.  在左窗格中，用鼠标右键单击**[数据库]**，指向**任务**，然后单击**备份**。
8.  在**备份类型**选择**事务日志**，然后单击**确定**两次。
9.  使远程桌面会话保持打开状态。

使用下列步骤还原数据库︰

1.  **[域名] \sp_farm_db**登录到 SQL Server 计算机辅助。
2.  从开始屏幕中，键入**SQL Studio**，，然后单击**SQL Server 管理 Studio**。
3.  单击**连接**。
4.  在左窗格中，用鼠标右键单击**数据库**，然后单击**还原数据库**。
5.  在**源**部分中，选择**设备**，然后单击省略号 （...） 按钮。
6.  在**选择备份设备**，请单击**添加**。
7.  在**备份文件的位置**，键入**\\[计算机名] \backup**，按 enter 键，选择**[数据库].bak**，然后再单击**确定**两次。 您现在应该看到完整备份，在**要还原的备份集**部分中的日志备份。
8.  在**选择页**中，单击**选项**。 **还原选项**部分中，在**恢复状态**中，选择**还原与不恢复**，，然后单击**确定**。
9.  单击**确定**时出现提示。

### 创建一个可用性组

至少一个数据库准备好 （使用备份和恢复方法） 后，您可以创建可用性组。

1.  返回到主 SQL Server 的计算机的远程桌面会话。
2.  在**SQL Server 管理 Studio**，在左窗格中，用鼠标右键单击**AlwaysOn 高可用性**，，然后单击**新建可用性组向导**。
3.  在**简介**页上，单击**下一步**。
4.  在**指定的可用性组名称**页中，**可用性组名称**中键入的可用性组名称 (示例︰ AG1)，然后单击**下一步**。
5.  在**选择数据库**页上选择 SharePoint 服务器场已备份的数据库，然后单击**下一步**。 由于预期主要副本中已经至少一个完整备份，这些数据库满足可用性组的先决条件。
6.  在**指定的副本**页上，单击**添加副本**。
7.  在**连接到服务器**上，键入辅助 SQL Server 的计算机的名称，然后单击**连接**。
8.  在**指定副本**页中，辅助的 SQL Server 主机列入**可用性复制副本**。 对于这两个实例中，设置下列选项值︰

初始的角色 | 选项 | 值
--- | --- | ---
主要 | 自动故障切换 （最多 2) | 选择
第二 | 自动故障切换 （最多 2) | 选择
主要 | 同步 （最多 3) 提交 | 选择
第二 | 同步 （最多 3) 提交 | 选择
主要 | 读第二 | 是
第二 | 读第二 | 是

9.  单击**下一步**。  
10. 在**选择初始数据同步**页面上，选择**只参加**，，然后单击**下一步**。 通过在主服务器上，执行完整和事务备份并还原该备份手动执行数据同步。 相反，可以选择**完全**让新可用性组向导为您进行数据同步。 但是，建议您不要在一些企业中找到的大型数据库的同步。  
11. 在**验证**页上，单击**下一步**。 可用性组侦听器配置不在于缺少的侦听器配置的警告。
12. 在**摘要**页上，单击**完成**。 完成该向导后，检查**结果**页后，可以验证可用性组创建成功。 如果是这样，单击**关闭**以退出向导。
13. 从开始屏幕中，键入**故障转移**，，然后单击**故障转移群集管理器**。 在左窗格中，打开您群集的名称，然后单击**角色**。 新角色的可用性组名称都不存在。  

为您的 SharePoint 数据库，您已成功配置 SQL Server AlwaysOn 可用性组。

## 配置的侦听器 AlwaysOn 可用性组

您可以选择创建侦听器配置为 AlwaysOn 可用性组。 有关的步骤，请参阅[教程︰ 侦听器配置为 AlwaysOn 可用性组](https://msdn.microsoft.com/library/dn425027.aspx)。 您应该创建一个侦听器并将其配置为使用内部负载平衡实例的虚拟 IP 地址。

一旦创建侦听器，您需要配置为使用监听器，而不是群集中的第一个 SQL Server 计算机名称的所有 SharePoint 虚拟机。 而不是使用的名称，配置 SharePoint 虚拟机使用的 SQL 别名。 有关详细步骤，请参阅[SharePoint 的 SQL 别名](http://blogs.msdn.com/b/priyo/archive/2013/09/13/sql-alias-for-sharepoint.aspx)。

SharePoint 与 SQL Server AlwaysOn 可用性组有关的其他信息，请参阅[配置 SQL Server 2012 AlwaysOn 可用性组为 SharePoint 2013](https://technet.microsoft.com/library/jj715261.aspx)。


## 其他资源

[在 Azure 中的 SQL Server AlwaysOn 可用性组部署 SharePoint](virtual-machines-workload-intranet-sharepoint-overview.md)

[在 Azure 的基础结构服务承载 SharePoint 服务器场](virtual-machines-sharepoint-infrastructure-services.md)

[SharePoint 与 SQL Server AlwaysOn infographic](http://go.microsoft.com/fwlink/?LinkId=394788)

[SharePoint 2013 的 Microsoft Azure 结构](https://technet.microsoft.com/library/dn635309.aspx)

[Azure 的基础结构服务实施指南](virtual-machines-infrastructure-services-implementation-guidelines.md)

[Azure 的基础结构服务工作负荷︰ 高可用性业务线应用程序](virtual-machines-workload-high-availability-lob-application.md)
