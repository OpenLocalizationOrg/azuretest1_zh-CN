---
ms.openlocfilehash: c89bcd8130d4e15247e8f42d96f387aebca67cf2
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties title="Creating an Oracle WebLogic Server 12c and Oracle Database 12c virtual machine in Azure" pageTitle="在 Azure 创建 Oracle WebLogic 服务器 12 c 和 12 c Oracle 数据库中的虚拟机" description="通过创建 Oracle WebLogic 服务器 12 c 和 Windows Server 2012 中 Microsoft Azure 上运行 Oracle 数据库 12 c 图像的示例步骤。" services="virtual-machines" authors="bbenz" documentationCenter=""/>
<tags ms.service="virtual-machines" ms.devlang="na" ms.topic="article" ms.tgt_pltfrm="na" ms.workload="infrastructure-services" ms.date="06/22/2015" ms.author="bbenz" />

#在 Azure 创建 Oracle WebLogic 服务器 12 c 和 12 c Oracle 数据库中的虚拟机

本文演示如何创建基于 Microsoft 提供 Oracle WebLogic 服务器 12 c 和 Oracle 数据库 12 c 图像在 Windows Server 2012 在 Azure 上运行的虚拟机。

##在 Azure 创建 Oracle WebLogic 服务器 12 c 和 12 c Oracle 数据库中的虚拟机

1. 登录到[Azure 的门户](https://ms.portal.azure.com/)。

2.  单击**市场上**单击**计算**，，然后在搜索框中键入**Oracle** 。

3.  选择的**Oracle 数据库 12 c 和上 Windows Server 2012 WebLogic Server 12 c 标准版**或**Oracle 数据库 12 c 和 Windows Server 2012 在 WebLogic Server 12 c 企业版**的图像。 查看有关此图像 （例如最小推荐大小） 的信息，然后单击**下一步**。

4.  指定虚拟机的**主机名**。

5.  指定虚拟机的**用户名称**。 请注意，此用户名的远程登录到虚拟机;这不是 Oracle 数据库用户名称。

6.  指定并确认密码对于虚拟机中，或者提供安全外壳协议 (SSH) 的公钥。

7.  选择**定价层**。  请注意，默认情况下显示建议定价层。 若要查看所有的配置选项，单击**查看所有**在右上角。

8. 设置可选配置所需 （请参阅[关于 Azure 虚拟机配置设置](https://msdn.microsoft.com/library/azure/dn763935.aspx)。 请注意以下事项︰

    一。 保留**存储帐户**，为的是使用虚拟机名称创建新的存储帐户。

    b。 将**可用性设置**保留为**未配置**。

    c。 这一次，不要添加任何终结点。

9.  选择或创建资源组。 有关详细信息，请参阅[使用 Azure 预览门户管理 Azure 的资源](resource-group-portal.md)。

10. 选择**订阅**。

11. 选择一个**位置**。


##若要创建您的数据库驻留在此虚拟机中

按照[创建 Oracle 数据库 12 c 虚拟机在 Azure 中](virtual-machines-creating-oracle-database-virtual-machine.md)，开头**来创建您的数据库使用 Azure 中的 Oracle 数据库 12 c 虚拟机**部分中的说明进行操作。

##若要配置 Oracle WebLogic 服务器驻留在此虚拟机的 12 c
按照[创建 Oracle WebLogic 服务器 12 c 虚拟机在 Azure 中](virtual-machines-creating-oracle-webLogic-server-12c-virtual-machine.md)，开头**在 Azure 将 Oracle WebLogic 服务器 12 c 虚拟机配置**节中的说明进行操作。 如果您想要设置 WebLogic 服务器群集，请参阅[创建 Oracle WebLogic 服务器 12 c 群集在 Azure 中](virtual-machines-creating-oracle-webLogic-server-12c-cluster.md)。

##其他资源
[Oracle 虚拟机映像的其他注意事项](miscellaneous-considerations-for-oracle-virtual-machine-images-new-article.md)

[Oracle 的虚拟机映像的列表](virtual-machines-oracle-list-oracle-virtual-machine-images.md)

[从 Java 应用程序连接到 Oracle 数据库](http://docs.oracle.com/cd/E11882_01/appdev.112/e12137/getconn.htm#TDPJD136)

[Oracle WebLogic 服务器对 Microsoft Azure 上使用 Linux 的 12 c](http://www.oracle.com/technetwork/middleware/weblogic/learnmore/oracle-weblogic-on-azure-wp-2020930.pdf)

[Oracle 数据库 2 天 DBA 12 c 版本 1](http://docs.oracle.com/cd/E16655_01/server.121/e17643/toc.htm)
