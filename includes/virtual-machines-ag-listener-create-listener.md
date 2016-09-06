---
ms.openlocfilehash: 7d668a9090a54ceb84f41b5e13adb4bc12ffde1f
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
在此步骤中，您手动创建故障转移群集管理器和 SQL Server 管理 Studio (SSMS) 可用性组侦听器。

1. 从控制的主副本的节点打开故障转移群集管理器。

1. 选择**网络**节点，并注意群集网络名称。 在 $ClusterNetworkName 变量中 PowerShell 脚本将使用此名称。

1. 展开群集名称，然后单击**角色**。

1. 在**角色**窗格中，右键单击可用性组名称，然后选择**添加资源** > **客户端访问点**。

    ![为可用性组添加客户端访问点](./media/virtual-machines-sql-server-configure-alwayson-availability-group-listener/IC678769.gif)

1. 在**名称**框中，为此新侦听器创建一个名称然后单击两次**下, 一步**，然后单击**完成**。 不要在此时使监听器或资源联机。

1. 单击**资源**选项卡，然后展开您刚刚创建的客户端访问点。 将群集中的每个群集网络看到**IP 地址**资源。 如果这是一个只有 Azure 的解决方案，您将只看到一个 IP 地址资源。

1. 如果您在配置混合方案，继续进行这一步。 如果您在配置一个 Azure 的唯一解决方案，请跳过下一步。 
     - 用鼠标右键单击对应于您的本地子网的 IP 地址资源，然后选择**属性**。 记下 IP 地址名称和网络名称。
     - 选择**静态 IP 地址**分配未使用的 IP 地址，然后单击**确定**。

1. 用鼠标右键单击对应于您 Azure 的子网的 IP 地址资源，然后选择属性。
    >[AZURE.NOTE] 如果侦听器以后无法联机因为选择 DHCP 的冲突的 IP 地址，可以在此属性窗口中配置有效的静态 IP 地址。

1. 在同一个**IP 地址**属性窗口中，更改**IP 地址名称**。 将**$IPResourceName**变量的 PowerShell 脚本中使用此 IP 地址的名称。 如果您的解决方案跨越多个 Azure VNets 为每个 IP 资源重复此步骤。