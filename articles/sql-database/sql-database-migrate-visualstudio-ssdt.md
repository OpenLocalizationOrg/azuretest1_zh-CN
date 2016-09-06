---
ms.openlocfilehash: d5617c2724a777e329b242a9fae7965151098ea3
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="使用 Visual Studio 和 SSDT 迁移" 
   description="Microsoft Azure SQL 数据库，数据库迁移、 导入数据库，导出数据库迁移向导" 
   services="sql-database" 
   documentationCenter="" 
   authors="carlrabeler" 
   manager="jeffreyg" 
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-management" 
   ms.date="08/24/2015"
   ms.author="carlrab"/>

#更新数据库中的位置，然后将部署到 SQL Azure 数据库

![alt 文本](./media/sql-database-migrate-visualstudio-ssdt/01VSSSDTDiagram.png)

的内部部署数据库迁移到 Azure SQL 数据库 V12 要求架构更改，因为此数据库使用 SQL Server 功能在 Azure SQL 数据库，或用来测试是否存在内部数据库中不受支持的功能不受支持时，请使用此选项。 

使用[最新的 Visual Studio 的 SQL Server 数据工具](https://msdn.microsoft.com/library/mt204009.aspx)Visual Studio 2013年更新 4 或更高版本。

使用此选项︰

 - 第一次使用 Visual Studio ("SSDT") 的 SQL Server 数据工具从源数据库创建数据库项目。 
 - 项目的目标平台将被设置为 Azure SQL 数据库 V12 并生成项目时要确定所有兼容性问题。 
 - 项目成功生成，一旦架构发布回源数据库的一个副本 （不覆盖源数据库）。
 - 数据比较功能在 SSDT 然后用于比较源数据库到新创建的 SQL Azure 兼容数据库和新数据库更新源数据库中的数据。 
 - 然后到 Azure 使用 SSMS，直接或通过导出，然后导入 BACPAC 文件部署更新的数据库。
 
>[AZURE.NOTE] 注意︰ 如果仅限于架构的部署要求，更新的架构可以直接从发布 Visual Studio 为 SQL Azure 数据库。

## 迁移步骤

1.  在 Visual Studio 中打开**SQL Server 对象资源管理器**。 使用**添加 SQL Server**连接到 SQL Server 实例，包含要迁移的数据库。 定位数据库资源管理器中，右击它并选择**创建新的项目...** 

    ![alt 文本](./media/sql-database-migrate-visualstudio-ssdt/02MigrateSSDT.png)

2.  配置导入设置导入**应用程序范围对象**。 取消选中要导入下面的选项︰ 引用登录、 权限和数据库设置。

    ![alt 文本](./media/sql-database-migrate-visualstudio-ssdt/03MigrateSSDT.png)

3.  单击**启动**导入数据库并创建该项目，其中将包含数据库中每个对象的 T-SQL 脚本文件。 在项目文件夹中嵌套的脚本文件。

    ![alt 文本](./media/sql-database-migrate-visualstudio-ssdt/04MigrateSSDT.png)

4.  Visual Studio 解决方案资源管理器中，右键单击数据库项目，并选择属性。 这将打开**项目设置**页面应该在其配置 Microsoft Azure SQL 数据库 V12 到目标平台。

    ![alt 文本](./media/sql-database-migrate-visualstudio-ssdt/05MigrateSSDT.png)

5.  右键单击该项目并选择**生成**以生成该项目。

    ![alt 文本](./media/sql-database-migrate-visualstudio-ssdt/06MigrateSSDT.png)

6.  **错误列表**显示每个不兼容。 在这种情况下，用户名 NT AUTHORITY\NETWORK 服务不兼容。 由于不兼容，您可以对它进行注释或将其删除 （和地址从数据库解决方案中删除此登录名和角色的含义）。 

    ![alt 文本](./media/sql-database-migrate-visualstudio-ssdt/07MigrateSSDT.png)  
7.  双击要打开的查询窗口和注释掉该脚本，脚本的第一个脚本，然后执行该脚本。 
    ![alt 文本](./media/sql-database-migrate-visualstudio-ssdt/08MigrateSSDT.png)

8.  每个脚本包含不兼容问题，直到没有错误保持重复此过程。
    ![alt 文本](./media/sql-database-migrate-visualstudio-ssdt/09MigrateSSDT.png)
 
9.  数据库没有错误时右键单击项目并选择**发布**生成并将数据库发布到源数据库 （强烈建议使用副本，至少最初） 一份。 
 - 在发布之前，根据源 SQL Server 版本 （早于 SQL Server 2014年），您可能需要重置该项目的目标平台启用部署。 
 - 如果您正在迁移旧版本的 SQL Server 数据库必须不引入任何功能的项目，除非先将数据库迁移到 SQL Server 的新版本不支持源 SQL Server 中。 

    ![alt 文本](./media/sql-database-migrate-visualstudio-ssdt/10MigrateSSDT.png)

    ![alt 文本](./media/sql-database-migrate-visualstudio-ssdt/11MigrateSSDT.png)

10. 在 SQL Server 对象资源管理器中，用鼠标右键单击您的源数据库，然后单击**数据比较**，以了解由向导进行了哪些更改原始数据库项目进行比较。 选择您的数据库的 Azure SQL V12 版本，然后单击**完成**。

    ![alt 文本](./media/sql-database-migrate-visualstudio-ssdt/12MigrateSSDT.png)

    ![alt 文本](./media/sql-database-migrate-visualstudio-ssdt/13MigrateSSDT.png)

12. 查看检测到的差异，然后单击要将数据从源数据库迁移到 Azure SQL V12 数据库的**更新目标**。 

    ![alt 文本](./media/sql-database-migrate-visualstudio-ssdt/14MigrateSSDT.png)

14. 部署到 Azure SQL 数据库使用 SSMS Azure SQL V12 兼容的数据库架构和数据。 请参阅[迁移兼容数据库使用 SSMS。](sql-database-migrate-ssms.md)