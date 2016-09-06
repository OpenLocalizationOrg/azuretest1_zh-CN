---
ms.openlocfilehash: e2bbe6fae910c721c33593700d506e8b179b0d8d
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="连接到 Excel 的 SQL Azure 数据库"
    description="Excel 电子表格到 SQL Azure 数据库的研究报告和数据。"
    services="sql-database"
    documentationCenter=""
    authors="joseidz"
    manager="joseidz"
    editor="joseidz"/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="07/09/2015"
    ms.author="joseidz"/>


# 连接到 Excel 的 SQL Azure 数据库
将 Excel 连接到 SQL Azure 数据库并且对数据库中的数据创建报表。

## 先决条件
- Azure SQL 数据库配置和运行。 若要创建新的 SQL 数据库，请参阅[Microsoft Azure SQL 数据库入门](sql-database-get-started.md)。
- [Microsoft Excel 2013 年](https://products.office.com/en-US/)（或 Microsoft Excel 2010）

## 连接到 SQL 数据库，并创建报表
1.  打开 Excel。
2.  在页面顶部的菜单栏中单击**数据**。
3.  单击**从其他来源**，那么请**从 SQL Server**。 将打开**数据连接向导**。

    ![数据连接向导][1]
4.  在**服务器名称**框中，键入 SQL Azure 数据库服务器名称。 示例：

        adventureserver.database.windows.net
5.  在**登录凭据**部分中，选择**使用以下的用户名和密码**，然后键入相应的 SQL 数据库服务器的凭据。 然后单击**下一步**。

    注意︰ 两个[PowerPivot](https://www.microsoft.com/download/details.aspx?id=102)和[电源查询](https://www.microsoft.com/download/details.aspx?id=39379)加载 excel 具有非常相似的经验。

6. 在**选择数据库和表**对话框中，从下拉菜单中选择**AdventureWorks**数据库，从表和视图的列表中选择**vGetAllCategories** ，然后单击**下一步**。

    ![选择数据库和表][5]
7. 在**保存数据连接文件并完成**对话框中，单击**完成**。
8. 在**导入数据**对话框中，选择**数据透视图**，请单击**确定**。

    ![选择导入数据][2]
9. 在**数据透视图字段**对话框中，选择下列配置创建的报表可用于每个类别的产品数。

    ![配置][3]
10. 成功如下所示︰

    ![成功][4]

## 下一步行动

如果您是软件即服务 (SaaS) 开发人员，了解[弹性数据库池](sql-database-elastic-pool.md)。 您可以轻松地管理大型数据库，使用[弹性数据库作业](sql-database-elastic-jobs-overview.md)的集合。

<!--Image references-->
[1]: ./media/sql-database-connect-excel/connect-to-database-server.png
[2]: ./media/sql-database-connect-excel/import-data.png
[3]: ./media/sql-database-connect-excel/power-pivot.png
[4]: ./media/sql-database-connect-excel/power-pivot-results.png
[5]: ./media/sql-database-connect-excel/select-database-and-table.png
