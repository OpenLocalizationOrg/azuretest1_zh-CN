---
ms.openlocfilehash: b9b16345319b37aea780a1df0e1cdc15942f10df
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
1. 导航回到故障转移群集管理器。  展开**角色**，然后突出显示可用性组。  在**资源**选项卡上，右键单击该侦听器名称，然后单击属性。

1. 单击**依赖项**选项卡。 如果没有列出的多个资源，请验证 IP 地址具有或不和，依赖关系。  单击**确定**。

1. 右键单击侦听器名称，然后单击**联机**。

1. 一旦该侦听器处于联机状态，从**资源**选项卡，右键单击可用性组，然后单击**属性**。

    ![配置可用性组资源](./media/virtual-machines-sql-server-configure-alwayson-availability-group-listener/IC678772.gif)

1. 创建侦听器名称资源 （不是 IP 地址资源名称） 的依赖项。 单击**确定**。

    ![侦听器名称添加依赖项](./media/virtual-machines-sql-server-configure-alwayson-availability-group-listener/IC678773.gif)

1. 启动**SQL Server 管理 Studio**并连接到的主副本。

1. 导航到**AlwaysOn 高可用性** | **可用性组** | **<AvailabilityGroupName>** | **可用性组侦听器**。 

3. 您现在应该看到创建故障转移群集管理器中的侦听器名称。 用鼠标右键单击该侦听器名称并单击**属性**。

1. 在**端口**框中指定的端口号为可用性组侦听器使用先前使用 $EndpointPort （在本教程中，1433年是默认值），然后单击**确定**。
