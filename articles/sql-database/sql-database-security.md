---
ms.openlocfilehash: 500b354879380108de2f81a63c18a575441ffde1
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="SQL 数据库安全性概述" 
   description="了解 SQL Azure 数据库和 SQL Server 安全，包括云之间的差异和 SQL Server 内部谈到身份验证、 授权、 连接安全、 加密和法规遵从性。" 
   services="sql-database" 
   documentationCenter="" 
   authors="tmullaney" 
   manager="jeffreyg" 
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services" 
   ms.date="07/14/2015"
   ms.author="thmullan;jackr"/>


# SQL 数据库设置安全性

## 概述

本文讲解保护数据层应用程序使用 SQL Azure 数据库的基本知识。 特别是，此文章将帮助您入门与资源的限制访问，保护数据，并监控数据库上的活动在[开始使用 SQL 数据库教程](sql-database-get-started.md)中创建。 SQL 的所有特点上具有的安全性功能的完整概述，请参阅[SQL Server 数据库引擎和 SQL Azure 数据库的安全中心](https://msdn.microsoft.com/library/bb510589)。

## 连接安全

连接安全指的是如何限制和对数据库使用防火墙规则和连接加密的安全连接。

服务器和数据库使用防火墙规则来拒绝从没有被明确列入白名单的 IP 地址的连接尝试。 若要允许您的应用程序或客户端计算机的公共 IP 地址尝试连接到一个新的数据库，必须首先创建服务器级别的防火墙规则使用 Azure 管理门户、 REST API 或 PowerShell。 作为最佳实践，您应该限制允许通过服务器防火墙尽最大可能的 IP 地址范围。 有关详细信息，请参阅[Azure SQL 数据库防火墙](https://msdn.microsoft.com/library/ee621782)。

与 SQL Azure 数据库的所有连接都需要加密 (SSL/TLS) 在所有时间而与数据库数据是"在途"。 在应用程序的连接字符串中，您必须指定要加密连接参数和**信任服务器证书 （这是为您如果您复制连接字符串从 Azure 管理门户），否则该连接不会验证服务器的身份，将会遭受"--拦截"攻击。 ADO.NET 的驱动程序，例如，这些连接字符串参数是**加密 = True**和**TrustServerCertificate = False**。 有关详细信息，请参阅[Azure SQL 数据库连接加密和证书验证](https://msdn.microsoft.com/library/azure/ff394108#encryption)。


## 身份验证

身份验证是指连接到数据库时，证明您的身份的方式。 SQL 数据库当前支持 SQL 身份验证，使用用户名和密码。

为您的数据库中创建逻辑服务器，指定"服务器管理员"登录使用的用户名和密码。 使用这些凭据，您可以验证任何数据库作为数据库所有者或"dbo。"该服务器上

但是，作为一种最佳做法应用程序应使用不同的帐户进行身份验证，这种方式可以限制授予应用程序的权限并在应用程序代码容易受到 SQL 注入攻击的情况下减少恶意活动的风险。 建议的方法是创建[包含数据库用户](https://msdn.microsoft.com/library/ff929188)，它允许您的应用程序直接到单个数据库上使用的用户名和密码进行身份验证。 通过执行以下 T-SQL 连接到用户数据库与您的服务器管理员登录时，您可以创建包含的数据库用户︰

```
CREATE USER ApplicationUser WITH PASSWORD = 'strong_password';
```

应用程序的连接字符串应指定此用户名和密码，而不是服务器管理员登录，以便连接到数据库。

对 SQL 数据库进行身份验证的详细信息，请参阅[管理数据库和 SQL Azure 数据库中的登录名](https://msdn.microsoft.com/library/ee336235)。


## 授权
授权是指在 Azure SQL 数据库中，可以执行哪些操作，这由您的用户帐户的角色成员资格和权限控制。 作为最佳实践，应授予用户所必需的最低权限。 Azure 的 SQL 数据库使这更容易地管理与 T SQL 中的角色︰

```
ALTER ROLE db_datareader ADD MEMBER ApplicationUser; -- allows ApplicationUser to read data
ALTER ROLE db_datawriter ADD MEMBER ApplicationUser; -- allows ApplicationUser to write data
```

与要连接的服务器管理员帐户是 db_owner，有权进行任何操作数据库中的成员。 保存此帐户的部署架构升级和其他管理操作。 使用"ApplicationUser"更有限权限帐户连接到数据库应用程序从应用程序所需的最低特权。

有方法可以进一步限制用户可以执行与 SQL Azure 数据库︰

* 除诸如[数据库角色](https://msdn.microsoft.com/library/ms189121)可以用于创建更强大的应用程序用户帐户或不太强大的管理帐户。
* 精细[的权限](https://msdn.microsoft.com/library/ms191291)，可以控制单个列、 表、 视图、 过程和数据库中的其他对象可以执行哪些操作。
* [模拟](https://msdn.microsoft.com/library/vstudio/bb669087)和[模块签名](https://msdn.microsoft.com/library/bb669102)可以用于安全地暂时提升权限。
* [行级安全性](https://msdn.microsoft.com/library/dn765131)允许您筛选的用户可以看到哪些行。
* [数据屏蔽](sql-database-dynamic-data-masking-get-started.md)可以用于限制对敏感数据的暴露。
* 可以使用[存储过程](https://msdn.microsoft.com/library/ms190782)来限制可以在数据库执行的操作。

从 Azure 管理门户管理数据库和逻辑服务器或使用 Azure 资源管理器 API 由您的门户用户帐户角色分配控制。 有关此主题的详细信息，请参阅[在 Azure 预览门户的基于角色的访问控制](../role-based-access-control-configure.md)。


## 加密

Azure 的 SQL 数据库可以帮助您的数据通过加密来保护您的数据时，它处于"闲置"，或存储在数据库文件和备份，并使用[透明数据加密](http://go.microsoft.com/fwlink/?LinkId=526242)。 若要加密数据库，数据库所有者身份连接，然后执行︰

```
CREATE DATABASE ENCRYPTION KEY 
   WITH ALGORITHM = AES_256 
   ENCRYPTION BY SERVER CERTIFICATE ##MS_TdeCertificate##;
   
ALTER DATABASE [AdventureWorks] SET ENCRYPTION ON;
```

加密的数据机密，其他方法，请考虑︰

* [单元格级别的加密](https://msdn.microsoft.com/library/ms179331.aspx)，以使用不同的密钥加密特定列或甚至数据单元格。
* 如果需要使用硬件安全模块或加密密钥层次结构的集中管理，可以考虑使用[Azure Azure VM 中的 SQL Server 使用的密钥存储库](http://blogs.technet.com/b/kv/archive/2015/01/12/using-the-key-vault-for-sql-server-encryption.aspx)。


## 审核

审核和跟踪数据库事件可以帮助您保持法规遵从性并且识别可疑的活动。 SQL 数据库的审核允许您对审核日志数据库中的事件记录在 Azure 存储帐户。 SQL 数据库的审核还集成了 Microsoft 电源 BI，以便于向下钻取报表和分析。 有关详细信息，请参阅[开始使用 SQL 数据库的审核](sql-database-auditing-get-started.md)。

## 法规遵从性

除了上面的特性和功能，可以帮助应用程序满足各种安全法规遵从性要求，SQL Azure 数据库还参与了定期审核，并经多种法规遵从性标准与认证。 有关详细信息，请参阅[Microsoft Azure 信任中心](http://azure.microsoft.com/support/trust-center/)，在哪里可以找到[SQL 数据库法规遵从性认证](http://azure.microsoft.com/support/trust-center/services/)的最新列表。
 
