---
ms.openlocfilehash: 16cdcf8a38dc23c397564e5c783c9b8bfc818e5a
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="管理数据库和 SQL Azure 数据库中的登录 |Microsoft Azure"
   description="如何使用服务器级别主体和其他帐户登录和在 SQL 数据库中的数据库进行管理"
   services="sql-database"
   documentationCenter=""
   authors="BYHAM"
   manager="jeffreyg"
   editor=""
   tags=""/>

<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management"
   ms.date="08/20/2015"
   ms.author="rickbyh"/>

# 管理数据库和 SQL Azure 数据库中的登录名

在 Microsoft Azure SQL 数据库，当您注册为服务，资源调配的过程会创建 SQL Azure 数据库服务器，**主**，和位于服务器级别主体的 SQL Azure 数据库服务器的登录名为的数据库。 该登录就类似于服务器级别主体， **sa**，在您的场所的 SQL Server 实例。 

SQL Azure 数据库服务器级别主体帐户始终有权管理所有服务器级和数据库级的安全。 本主题介绍如何使用服务器级别主体和其他帐户来管理登录名和数据库 SQL 数据库中。

> [AZURE.IMPORTANT] SQL 数据库 V12 允许用户在数据库中使用包含的数据库用户的身份验证。 包含的数据库用户不需要登录。 这使数据库可移植性，而且可以减少服务器级别主体能够控制对数据库的访问。 使包含的数据库用户都有重要的安全影响。 有关详细信息，请参阅[包含数据库用户-使您数据库可移植](https://msdn.microsoft.com/library/ff929188.aspx)、[创建用户 (事务处理 SQL)](https://msdn.microsoft.com/library/ms173463.aspx)和[包含数据库](https://msdn.microsoft.com/library/ff929071.aspx)。

## SQL 数据库安全管理概述

在 SQL 数据库中的安全管理是类似于 SQL Server 的本地实例的安全管理。 在数据库级别的安全管理是几乎完全相同，只在可用的参数有差异。 因为 SQL 数据库可以扩展到一个或多个物理计算机，SQL Azure 数据库服务器级管理使用不同的策略。 下表总结了不同于在 SQL Azure 数据库的后端 SQL Server 安全管理的方式。

| 点的差异 | SQL Server 内部 | SQL azure 数据库 |
|------------------------------------------------|-----------------------------------------------------------------------------|--------------------------------------------------|
| 管理服务器级别的安全         | SQL Server 管理 Studio 对象资源管理器中的安全文件夹       | Master 数据库和通过 Azure 门户 |
| 用于创建登录的服务器级别的安全角色 | securityadmin 固定服务器角色的详细信息，请参阅服务器级别角色 | 在母版中的 loginmanager 数据库角色         |
| 管理登录的命令                   | 创建登录、 更改登录信息，删除登录                                           | 创建登录，ALTER LOGIN，除去登录 （有一些参数限制，您必须连接到主数据库） |
| 视图，它显示所有登录名                     | sys.server_principals                                                       | sys.sql_logins （您必须连接到主数据库）|
| 用于创建数据库的服务器级角色       | dbcreator 固定数据库角色的详细信息，请参阅服务器级别角色   | 在 master 数据库中的 dbmanager 数据库角色 |
| 用于创建数据库命令                | 创建数据库                                                             | 创建数据库 （有一些参数限制，您必须连接到主数据库） |
| 列出所有数据库的视图                  | sys.databases                                                               | sys.databases （您必须连接到主数据库） |

### 服务器级管理和 master 数据库

SQL Azure 数据库服务器是一种抽象，定义了一组数据库。 与 SQL Azure 数据库服务器相关联的数据库可能驻留在不同的物理计算机，Microsoft 数据中心。 使用名为主控形状的单一数据库对所有这些执行服务器级管理。

跟踪的 master 数据库的登录，以及哪些登录有权创建数据库或其他登录名。 您必须连接到主数据库，只要创建、 更改或删除登录名或数据库。 **Master**数据库还有``sys.sql_logins``和``sys.databases``分别查看登录名和数据库，您可以使用的视图。 

> [AZURE.NOTE] ``USE``命令不受支持的数据库间进行切换。 建立直接到目标数据库的连接。

您可以管理用户和 Microsoft Azure SQL 数据库中的对象的数据库级安全方法相同的 SQL Server 本地实例。 没有可用相应命令仅在参数的差异。 有关详细信息，请参阅[SQL Azure 数据库安全准则和限制](https://msdn.microsoft.com/library/azure/ff394108.aspx)。

### 管理登录

通过连接到主数据库管理的服务器级别主体登录的登录名。 您可以使用``CREATE LOGIN``， ``ALTER LOGIN``，或``DROP LOGIN``语句。 下面的示例创建名为**login1**的登录︰

```
-- first, connect to the master database
CREATE LOGIN login1 WITH password='<ProvidePassword>';
```

> [AZURE.NOTE] 创建登录时，您必须使用强密码。 有关详细信息，请参阅[强密码](https://msdn.microsoft.com/library/ms161962.aspx)。

#### 使用新的登录名

为了连接到 Microsoft Azure SQL 数据库使用您创建的登录，必须先授予每个登录数据库级别权限通过使用``CREATE USER``命令。 有关详细信息，请参阅登录授予数据库级别权限。

因为某些工具执行表格格式数据流 (TDS) 不同，您可能需要将 SQL Azure 数据库服务器名称追加到连接字符串中使用的登录``<login>@<server>``表示法。 在这些情况下，单独的登录名和 SQL Azure 数据库服务器名称，而``@``符号。 例如，如果您的登录名被命名为**login1** ，SQL Azure 数据库服务器的完全限定的名是**servername.database.windows.net**，您的连接字符串的用户名参数应该是︰ **login1@servername**。 此限制将限制放上的文本，您可以选择登录名。 有关详细信息，请参阅[创建登录 (事务处理 SQL)](https://msdn.microsoft.com/library/ms189751.aspx)。

## 授予登录到服务器级别权限

为了让登录服务器级别主体以外的管理服务器级别的安全，Microsoft Azure SQL 数据库提供了两个安全角色︰ **loginmanager**，用于创建登录名和**dbmanager**来创建数据库。 只有在**master**数据库中的用户可以添加到这些数据库角色中。

> [AZURE.NOTE] 若要创建登录名或数据库，必须连接到**主**数据库 （即逻辑表示的**主机**）。

### Loginmanager 角色

正如**securityadmin**固定的服务器角色的内部实例的 SQL Server，Microsoft Azure SQL 数据库中的**loginmanager**数据库角色是否有权创建登录名。 服务器级别主体登录 （通过设置过程创建） 或**loginmanager**数据库角色的成员可以创建新的登录名。 

### Dbmanager 角色

Microsoft Azure SQL 数据库**dbmanager**数据库角色是类似于**dbcreator**的内部实例的 SQL Server 固定服务器角色。 服务器级别主体登录 （通过设置过程创建） 或**dbmanager**数据库角色的成员可以创建数据库。 一旦用户**dbmanager**数据库角色的成员，可以使用 SQL Azure 数据库创建数据库``CREATE DATABASE``命令，但这必须在 master 数据库中执行命令。 有关详细信息，请参阅[创建的数据库 (SQL Server 事务处理的 SQL)](https://msdn.microsoft.com/library/ms176061.aspx)。

### 如何将 SQL 数据库服务器级角色分配

若要创建登录名和关联，可以从数据库或其他登录名的用户，请执行以下步骤︰

1. 使用连接到**主**数据库 （由资源调配过程创建） 的服务器级别主体登录凭据或凭据现有**loginmanager**数据库角色的成员。
2. 创建一个登录使用``CREATE LOGIN``命令。 有关详细信息，请参阅[创建登录 (事务处理 SQL)](https://msdn.microsoft.com/library/ms189751.aspx)。
3. 为创建一个新用户登录在 master 数据库中使用``CREATE USER``命令。 有关详细信息，请参阅[创建用户 (事务处理 SQL)](https://msdn.microsoft.com/library/ms173463.aspx)。
4. 使用存储的过程``sp_addrolememeber``将新用户添加到**dbmanager**数据库角色和 / 或 loginmanager 数据库角色。

下面的代码示例演示如何创建登录名为**login1**，以及一个名为**login1User**的相应数据库用户就能创建数据库或其他登录到**主**数据库连接时︰

```
-- first, connect to the master database
CREATE LOGIN login1 WITH password='<ProvidePassword>';
CREATE USER login1User FROM LOGIN login1;
EXEC sp_addrolemember 'dbmanager', 'login1User';
EXEC sp_addrolemember 'loginmanager', 'login1User';
```

> [AZURE.NOTE] 创建登录时，您必须使用强密码。 有关详细信息，请参阅[强密码](https://msdn.microsoft.com/library/ms161962.aspx)。

## 授予数据库访问权限的登录名

必须在 master 数据库中创建的所有登录。 在创建登录之后，您可以为该登录另一个数据库中创建用户帐户。 Microsoft Azure SQL 数据库还支持方式相同的 SQL Server 内部部署的实例中的数据库角色。

若要创建一个用户帐户在另一个数据库中，假定您没有创建登录名或数据库中，请执行以下步骤︰

1. 连接到**主**数据库 （与登录名有**loginmanager**和**dbmanager**角色）。
2. 创建新登录使用``CREATE LOGIN``命令。 有关详细信息，请参阅[创建登录 (事务处理 SQL)](https://msdn.microsoft.com/library/ms189751.aspx)。 不支持 Windows 身份验证。
3. 创建一个新数据库使用``CREATE DATABASE``命令。 有关详细信息，请参阅[创建的数据库 (SQL Server 事务处理的 SQL)](https://msdn.microsoft.com/library/ms176061.aspx)。
4. 建立与新数据库 （与登录名创建数据库） 的连接。
5. 创建新的数据库使用新用户``CREATE USER``命令。 有关详细信息，请参阅[创建用户 (事务处理 SQL)](https://msdn.microsoft.com/library/ms173463.aspx)。

下面的代码示例演示如何创建名为**login1**的登录和名为**database1**的数据库︰

```
-- first, connect to the master database
CREATE LOGIN login1 WITH password='<ProvidePassword>';
CREATE DATABASE database1;
```

> [AZURE.NOTE] 创建登录时，您必须使用强密码。 有关详细信息，请参阅[强密码](https://msdn.microsoft.com/library/ms161962.aspx)。

这下一个示例演示如何创建一个名为**login1User**中对应于登录**login1**数据库**database1**的数据库用户︰

```
-- Establish a new connection to the database1 database
CREATE USER login1User FROM LOGIN login1;
```

此 Microsoft Azure SQL 数据库中的数据库级别权限模型是与后端 SQL Server 的实例相同。 信息，请参阅 SQL Server 联机丛书的引用中的以下主题。

- [管理登录、 用户和架构帮助主题](https://msdn.microsoft.com/library/aa337552.aspx) 
- [第 2 课︰ 配置数据库对象权限](https://msdn.microsoft.com/library/ms365345.aspx) 

> [AZURE.NOTE] 中可用的参数，Microsoft Azure SQL 数据库中的与安全相关事务处理 SQL 语句可能略有不同。 有关详细信息，请参阅[事务处理 SQL Azure SQL 数据库的引用](sql-database-transact-sql-information.md)。 

## 查看登录名和数据库

若要查看登录名和数据库 SQL Azure 数据库服务器上，使用主数据库的``sys.sql_logins``，``sys.databases``查看，分别。 下面的示例演示如何在 SQL Azure 数据库服务器上显示所有登录名和数据库的列表。

```
-- first, connect to the master database
SELECT * FROM sys.sql_logins;
SELECT * FROM sys.databases; 
```

## 请参见

[Azure 的 SQL 数据库安全准则和限制](sql-database-security-guidelines.md)