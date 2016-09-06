---
ms.openlocfilehash: a42d268192ab81afdb7dd1e35f769d175b18a84b
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="SQL Server 在 Azure 的虚拟机中的安全注意事项"
    description="提供保护 SQL Server 运行在 Azure 虚拟机的常规指导。"
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
    ms.date="08/19/2015"
    ms.author="jroth" />

# SQL Server 在 Azure 的虚拟机中的安全注意事项

本主题包括总体安全准则，帮助在 Azure VM 中建立到 SQL Server 实例的安全访问。 但是，为了确保更好地保护您的 SQL Server 数据库实例 Azure 中，我们建议 Azure 实施传统的内部部署安全措施，除了安全最佳做法。

有关 SQL Server 安全做法的详细信息，请参阅[SQL Server 2008 R2 安全最佳做法的运营和管理任务](http://download.microsoft.com/download/1/2/A/12ABE102-4427-4335-B989-5DA579A4D29D/SQL_Server_2008_R2_Security_Best_Practice_Whitepaper.docx)

Azure 将遵循几个行业管理法规和标准，使您能够在虚拟机中运行的 SQL Server 与建立法规遵从性解决方案。 有关使用 Azure 的法规遵从性的信息，请参阅[Azure 信任中心](http://azure.microsoft.com/support/trust-center/)。

下面是配置和连接到 Azure VM 中的 SQL Server 的实例时应考虑的安全建议的列表。

## 管理帐户的注意事项︰

- 创建未命名**管理员**是唯一的本地管理员帐户。

- 对所有帐户使用复杂的强密码。 有关如何创建强密码的详细信息，请参见[创建强密码](http://go.microsoft.com/fwlink/?LinkId=293596)的安全性和安全中心中的文章。

- 默认情况下，Azure SQL Server 虚拟机安装过程中选择 Windows 身份验证。 因此，禁用**SA**登录，安装程序指定密码。 我们建议应**SA**登录不会使用或启用。 以下是如果需要 SQL 登录的备选策略︰
    - 创建 SQL 帐户具有**管理服务器**的权限。
    - 如果您必须使用**SA**登录，启用登录和重命名它并分配一个新密码。
    - 前面提到的这两个选项都需要更改为 SQL Server 身份验证模式和 Windows 身份验证模式。 有关详细信息，请参阅更改服务器身份验证模式。

- 创建 SQL 帐户具有管理服务器的权限。

- 如果您必须使用 SA 登录，启用登录和重命名它并分配一个新密码。

- 前面提到的这两个选项都需要更改为**SQL Server 和 Windows 身份验证模式**的身份验证模式。 有关详细信息，请参阅[更改服务器身份验证模式](https://msdn.microsoft.com/library/ms188670.aspx)。

## 保护连接到 Azure 的虚拟机的注意事项︰

- 请考虑使用[Azure 虚拟网络](../virtual-network/virtual-networks-overview.md)来管理而不是公用的 RDP 端口中的虚拟机。

- 删除您不使用这些虚拟机上的任何终结点。

- 启用加密的连接选项的实例的 SQL Server 数据库引擎在 Azure 的虚拟机。 将签名证书配置 SQL server 实例。 有关详细信息，请参阅[启用加密连接到数据库引擎](https://msdn.microsoft.com/library/ms191192.aspx)和[连接字符串语法](https://msdn.microsoft.com/library/ms254500.aspx)。

- 如果您的虚拟机应只能从特定的网络进行访问，使用 Windows 防火墙来限制对特定 IP 地址或网络子网的访问。 您也可以考虑添加 ACL 允许您限制仅对客户端通信的终结点上。 有关使用 Acl 具有终结点的说明，请参阅[管理某个终结点上的 ACL](virtual-machines-set-up-endpoints.md#manage-the-acl-on-an-endpoint)

## 下一步行动

如果您感还围绕性能的最佳实践，请参阅[SQL Server 在 Azure 的虚拟机的性能最佳](virtual-machines-sql-server-performance-best-practices.md)。

与在 Azure 的虚拟机中运行 SQL Server 相关的其他主题，请参阅[SQL Server 在 Azure 虚拟机概述](virtual-machines-sql-server-infrastructure-services.md)。

