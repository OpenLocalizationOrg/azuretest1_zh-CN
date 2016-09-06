---
ms.openlocfilehash: 02c27ea4e72dc140b39e6268b640be5d391bc296
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="连接到 SQL Server 虚拟机在 Azure 上"
   description="本主题介绍如何连接到 Azure 中的虚拟机上运行的 SQL Server。 方案有所不同，具体取决于网络的配置和客户端的位置。"
   services="virtual-machines"
   documentationCenter="na"
   authors="rothja"
   manager="jeffreyg"
   editor="monicar" />
<tags 
   ms.service="virtual-machines"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows-sql-server"
   ms.workload="infrastructure-services"
   ms.date="08/18/2015"
   ms.author="jroth" />

# 连接到 SQL Server 虚拟机在 Azure 上
 
## 概述

配置连接到 Azure 虚拟机上运行的 SQL Server 内部 SQL Server 实例所需的步骤与不显著不同。 您仍需要处理涉及防火墙、 身份验证和登录数据库的配置步骤。

但也有一些特定于 Azure Vm 的 SQL Server 连接方面。 本文介绍一些[一般的连接方案](#connection-scenarios)，然后提供[有关配置在 Azure VM 中的 SQL Server 连接的详细的步骤](#steps-for-configuring-sql-server-connectivity-in-an-azure-vm)。

>[AZURE.NOTE] 本文重点介绍连接。 资源调配和连接的完整的预排，请参阅[配置 SQL Server 虚拟机在 Azure 上](virtual-machines-provision-sql-server.md)。

## 连接方案

客户端连接到虚拟机上运行 SQL Server 的方式的客户端和计算机/网络连接配置的位置而异。 这些方案包括︰

- [连接到 SQL Server 中的同一个云服务](#connect-to-sql-server-in-the-same-cloud-service)
- [通过 internet 连接到 SQL Server](#connect-to-sql-server-over-the-internet)
- [连接到 SQL Server 中的相同的虚拟网络](#connect-to-sql-server-in-the-same-virtual-network)

### 连接到 SQL Server 中的同一个云服务

可以在同一个云服务中创建多个虚拟机。 若要了解此方案中的虚拟机，请参阅[如何连接虚拟机与虚拟的网络或云服务](cloud-services-connect-virtual-machine.md)。

首先，按照[本文配置连接的步骤](#steps-for-configuring-sql-server-connectivity-in-an-azure-vm)。 请注意，您不需要安装公共端点，如果要在同一个云服务中的计算机之间连接。 

您可以在客户端连接字符串中使用虚拟机**的主机名**。 主机名是您在创建过程中赋予您的虚拟机的名称。 例如，如果 SQL 虚拟机命名为**mysqlvm** **mycloudservice.cloudapp.net**的云服务 DNS 名称，同一个云服务中的客户端虚拟机可以使用下面的连接字符串连接︰

    "Server=mysqlvm;Integrated Security=false;User ID=<login_name>;Password=<your_password>"

### 通过 Internet 连接到 SQL Server

如果您要从 Internet 连接到您的 SQL Server 数据库引擎，您必须创建传入的 TCP 通信的虚拟机终结点。 此 Azure 的配置步骤中，将定向到虚拟机可以访问 TCP 端口的入站 TCP 端口通讯。

首先，按照[本文配置连接的步骤](#steps-for-configuring-sql-server-connectivity-in-an-azure-vm)。 具有 internet 访问权限的任何客户端无法连接到 SQL Server 实例通过指定的云服务 DNS 名称 （例如**mycloudservice.cloudapp.net**) 和虚拟机端点 （例如**57500**)。

    "Server=mycloudservice.cloudapp.net,57500;Integrated Security=false;User ID=<login_name>;Password=<your_password>"

尽管这样，客户端通过互联网连接，但这并不意味着任何人都可以连接到 SQL Server。 外部客户端具有到正确的用户名和密码。 为额外的安全性，请不要使用已知端口 1433年公用虚拟机终结点。 如果可能，请考虑添加 ACL，并允许您限制仅对客户端通信的终结点上。 有关使用 Acl 具有终结点的说明，请参阅[管理某个终结点上的 ACL](virtual-machines-set-up-endpoints.md#manage-the-acl-on-an-endpoint)。 

>[AZURE.NOTE] 请务必注意在使用这种技术与 SQL Server 进行通信时，所返回的所有数据被都视为数据中心的传出通信。 它是容易正常[价格上的出站数据传输](http://azure.microsoft.com/pricing/details/data-transfers)。 这是真即使使用来自相同的 Azure 数据中心内的另一个计算机或云服务的这种方法因为流量仍会通过 Azure 的公用负载平衡器。

### 连接到 SQL Server 中的相同的虚拟网络

[虚拟网络](..\virtual-network\virtual-networks-overview.md)支持其他方案。 即使不同的云服务中存在的这些虚拟机，您可以在同一个虚拟的网络中，连接虚拟机。 并通过[站点到站点 VPN](../vpn-gateway/vpn-gateway-site-to-site-create.md)，您可以创建一个与内部网络和计算机连接的虚拟机的混合体系结构。

虚拟网络还使您能够将 Azure 虚拟机加入到域。 这是使用 Windows 身份验证对 SQL Server 的唯一办法。 其他连接方案要求使用用户名和密码的 SQL 身份验证。

首先，按照[本文配置连接的步骤](#steps-for-configuring-sql-server-connectivity-in-an-azure-vm)。 如果您要配置的域环境和 Windows 身份验证，您不需要使用本文中的步骤配置 SQL 身份验证和登录名。 此外，在这种情况下不需要公用的终结点。

假定您已配置了 DNS，可以通过在连接字符串中指定 SQL Server 虚拟机的主机名连接到 SQL Server 实例。 下面的示例假定 Windows 身份验证也已配置和用户已被授予访问 SQL Server 实例。

    "Server=mysqlvm;Integrated Security=true" 

请注意，在这种情况下，您还可以指定虚拟机的 IP 地址。

## 在 Azure VM 配置 SQL Server 连接的步骤

[AZURE.INCLUDE [Connect to SQL Server in a VM](../../includes/virtual-machines-sql-server-connection-steps.md)]

## 下一步行动

若要查看资源调配以及这些连接的步骤的说明，请参阅[配置 SQL Server 虚拟机在 Azure 上](virtual-machines-provision-sql-server.md)。

如果您打算使用 AlwaysOn 可用性组的高可用性和灾难恢复，则应考虑实现一个侦听器。 数据库客户端连接到侦听程序，而不是直接到一个 SQL Server 实例。 侦听器会将客户端路由到可用性组中的主副本。 有关详细信息，请参阅[配置 AlwaysOn 可用性组在 Azure 中的 ILB 侦听器](virtual-machines-sql-server-configure-ilb-alwayson-availability-group-listener.md)。

请务必检查所有的安全最佳做法的 Azure 的虚拟机上运行的 SQL Server。 有关详细信息，请参阅[SQL Server 在 Azure 的虚拟机的安全注意事项](virtual-machines-sql-server-security-considerations.md)。

在 Azure 的虚拟机中运行 SQL Server 相关的其他主题，请参阅[SQL Server 在 Azure 的虚拟机](virtual-machines-sql-server-infrastructure-services.md)。 
