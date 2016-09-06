---
ms.openlocfilehash: 5baaa8c75ba8a4deb935f465efc82c4eb1cce91b
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    urlDisplayName="How to connect to an Azure SQL database using SSMS"
    pageTitle="如何连接到使用 SSMS SQL Azure 数据库 |Microsoft Azure"
    metaKeywords=""
    description="了解如何连接到使用 SSMS SQL Azure 数据库。"
    metaCanonical=""
    services="sql-database"
    documentationCenter=""
    title="How to connect to an Azure SQL database using SSMS"
    authors="stevestein" solutions=""
    manager="jeffreyg" editor="" />

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/31/2015"
    ms.author="sstein" />

# 使用 SQL Server 管理 Studio 进行连接
本文介绍了如何安装 SQL Server 管理 Studio (SSMS)、 连接到 Azure，数据库服务器，然后执行一个简单查询，使用事务处理 SQL 语句。

您首先需要在 Azure 中的 SQL 数据库。 在[开始使用 Microsoft Azure SQL 数据库](sql-database-get-started.md)中，则可以创建一个快速的说明。 此处的示例基于 AdventureWorks 示例数据库在这篇文章，但是相同的步骤操作，直到您执行该查询时，适用于任何 SQL 数据库。

## 安装并启动 SQL Server 管理 Studio (SSMS)
使用 SQL 数据库，您应该使用 SSMS 的最新版本。 请参阅[下载 SQL Server 管理 Studio](https://msdn.microsoft.com/library/mt238290.aspx)即可使用它。 最新的版本，与 SSMS 时自动通知您最新的更新可用时。

## 启动 SSMS 并连接到您的 SQL 数据库服务器
1. 类型"Microsoft SQL Server 管理 Studio"窗口中搜索框，，，然后单击启动 SSMS 桌面应用程序。
2. 在**连接到服务器**对话框中的**服务器名称**框中，键入承载服务器的 SQL 数据库格式名称*&lt;服务器名 >*。**database.windows.net**。
3. 从**身份验证**列表中选择**SQL Server 身份验证**。
4. 键入**登录名**和**密码**，当您创建服务器上，设置了，然后单击**连接**。

    ![SSMS 连接到 SQL Azure 数据库服务器](./media/sql-database-connect-query-ssms/1-connect.png)

### 如果连接失败
连接失败的最常见原因是服务器名、 用户名或密码，以及不允许出于安全考虑连接的服务器中的错误。 请确保服务器的防火墙设置允许从本地计算机的 IP 地址和 SSMS 客户端使用的 IP 地址的连接。 有时他们是不同的。 

如果由于防火墙规则，则连接会失败，错误消息中报告的 IP 地址。 将此 IP 地址添加到服务器的防火墙规则。 有关详细信息，请参阅[如何︰ 配置防火墙设置 （Azure SQL 数据库）](sql-database-configure-firewall-settings.md)。

## 运行查询示例
连接之后，您可以运行的示例查询。 如果您没有创建[开始使用 Microsoft Azure SQL 数据库](sql-database-get-started.md)中使用 AdventureWorks 示例数据库，此查询将不会起作用。 跳过直接到下一步的行动，以了解详细信息。

1. 在**对象资源管理器**中导航到**AdventureWorks**数据库。
2. 用鼠标右键单击数据库，然后选择**新建查询**。

    ![新建查询](./media/sql-database-connect-query-ssms/4-run-query.png)

3. 在查询窗口中复制并粘贴以下代码。

        SELECT
        CustomerId
        ,Title
        ,FirstName
        ,LastName
        ,CompanyName
        FROM SalesLT.Customer;

4. 单击 [**执行**] 按钮。  下面的屏幕快照演示成功的查询。

    ![Sucess](./media/sql-database-connect-query-ssms/5-success.png)

## 下一步行动
事务处理 SQL 语句可用于创建和管理在基本相同的方法使用 SQL Server，您可以在 Azure 中的数据库。 如果您熟悉事务处理 SQL 中使用 SQL Server，请参阅[Azure SQL 数据库事务处理 SQL 信息)](sql-database-transact-sql-information.md)的区别摘要。

如果您是初次接触事务处理 SQL，请参阅[教程︰ 写入事务处理 SQL 语句](https://msdn.microsoft.com/library/ms365303.aspx)和[事务处理 SQL 参考 （数据库引擎）](https://msdn.microsoft.com/library/bb510741.aspx)。