---
ms.openlocfilehash: 2dac8fbfc41cfad0920a788752a614fe81ef4c4a
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="保护在 SQL 数据仓库数据库 |Microsoft Azure"
   description="开发解决方案保护在 Azure SQL 数据仓库数据库的提示。"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sahaj08"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="06/22/2015"
   ms.author="sahajs"/>

# SQL 数据仓库数据库安全

本文讲解的 Azure SQL 数据仓库数据库设置安全性基础知识。 特别是，该文章将帮助您初步了解资源限制的访问，保护数据，并监控数据库上的活动。

## 连接安全

连接安全指的是如何限制和对数据库使用防火墙规则和连接加密的安全连接。

服务器和数据库使用防火墙规则来拒绝从没有被明确列入白名单的 IP 地址的连接尝试。 若要允许您的应用程序或客户端计算机的公共 IP 地址尝试连接到一个新的数据库，必须首先创建服务器级别的防火墙规则使用 Azure 管理门户、 REST API 或 PowerShell。 作为最佳实践，您应该限制允许通过服务器防火墙尽最大可能的 IP 地址范围。 有关详细信息，请参阅[SQL Azure 数据库防火墙][]。


## 身份验证

身份验证是指连接到数据库时，证明您的身份的方式。 SQL 数据仓库目前支持 SQL 身份验证，使用用户名和密码。

为您的数据库中创建逻辑服务器，指定"服务器管理员"登录使用的用户名和密码。 使用这些凭据，您可以验证任何数据库作为数据库所有者或"dbo。"该服务器上

但是，作为一种最佳做法组织的用户应该使用不同的帐户进行身份验证，这种方式可以限制授予应用程序的权限并在应用程序代码容易受到 SQL 注入攻击的情况下减少恶意活动的风险。 建议的方法是创建一个包含的数据库用户，它允许您的应用程序直接到单个数据库上使用的用户名和密码进行身份验证。 通过执行以下 T-SQL 连接到用户数据库与您的服务器管理员登录时，您可以创建包含的数据库用户︰

```
CREATE USER ApplicationUser WITH PASSWORD = 'strong_password';
```

对 SQL 数据库进行身份验证的详细信息，请参阅[管理数据库和 SQL Azure 数据库中的登录名][]。


## 授权

授权是指在 Azure SQL 数据仓库数据库中，可以执行哪些操作，这由您的用户帐户的角色成员资格和权限控制。 作为最佳实践，应授予用户所必需的最低权限。 Azure SQL 数据仓库使这更容易地管理与 T SQL 中的角色︰

```
ALTER ROLE db_datareader ADD MEMBER ApplicationUser; -- allows ApplicationUser to read data
ALTER ROLE db_datawriter ADD MEMBER ApplicationUser; -- allows ApplicationUser to write data
```

与要连接的服务器管理员帐户是 db_owner，有权进行任何操作数据库中的成员。 保存此帐户的部署架构升级和其他管理操作。 使用"ApplicationUser"更有限权限帐户连接到数据库应用程序从应用程序所需的最低特权。

有方法可以进一步限制用户可以执行与 SQL Azure 数据库︰
- 除诸如[数据库角色][]可以用于创建更强大的应用程序用户帐户或不太强大的管理帐户。
- 精细[的权限][]，可以控制单个列、 表、 视图、 过程和数据库中的其他对象可以执行哪些操作。
- 可以使用[存储过程][]来限制可以在数据库执行的操作。

从 Azure 管理门户管理数据库和逻辑服务器或使用 Azure 资源管理器 API 由您的门户用户帐户角色分配控制。 有关此主题的详细信息，请参阅[在 Azure 预览门户的基于角色的访问控制][]。



## 加密

Azure SQL 数据仓库可以帮助您的数据通过加密来保护您的数据时，它处于"闲置"，或存储在数据库文件和备份，并使用[透明数据加密][]。 若要加密数据库，数据库所有者身份连接，然后执行︰


```

ALTER DATABASE [AdventureWorks] SET ENCRYPTION ON;

```

您可以从[Azure 门户网站][]中的数据库设置来启用透明数据加密。



## 审核

审核和跟踪数据库事件可以帮助您保持法规遵从性并且识别可疑的活动。 SQL 数据仓库审核允许您对审核日志数据库中的事件记录在 Azure 存储帐户。 SQL 数据仓库审核还集成了 Microsoft 电源 BI，以便于向下钻取报表和分析。 有关详细信息，请参阅[开始使用 SQL 数据库的审核][]。



## 下一步行动
开发的更多提示，请参阅[开发概述][]。

<!--Image references-->

<!--Article references-->
[开发概述]: sql-data-warehouse-overview-develop.md


<!--MSDN references-->
[SQL 数据库的 azure 防火墙]: https://msdn.microsoft.com/library/ee621782.aspx
[数据库角色]: https://msdn.microsoft.com/library/ms189121.aspx
[管理数据库和 SQL Azure 数据库中的登录名]: https://msdn.microsoft.com/library/ee336235.aspx
[权限]: https://msdn.microsoft.com/library/ms191291.aspx
[存储的过程]: https://msdn.microsoft.com/library/ms190782.aspx 
[透明数据加密]: http://go.microsoft.com/fwlink/?LinkId=526242
[开始使用 SQL 数据库的审核]: sql-database-auditing-get-started.md
[Azure 门户]: https://portal.azure.com/

<!--Other Web references-->
[在 Azure 预览门户的基于角色的访问控制]: http://azure.microsoft.com/en-us/documentation/articles/role-based-access-control-configure.aspx
