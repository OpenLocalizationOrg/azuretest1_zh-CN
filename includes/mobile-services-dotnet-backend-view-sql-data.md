---
ms.openlocfilehash: 46c031191ef96da1a0425b5d7981365099b6e605
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---

本教程的最后一个可选步骤是检查与手机信息服务相关的 SQL 数据库中存储的数据进行评审。 

1. 在 Azure 管理门户中，单击与您的移动服务关联的数据库管理。
 
    ![登录 SQL 数据库进行管理](./media/mobile-services-dotnet-backend-view-sql-data/manage-sql-azure-database.png)

2. 管理门户中执行查询以查看 Windows 应用商店应用程序所做的更改。 您的查询将会类似于下面的查询，但使用数据库的名称而不是<code>todolist</code>。</p>

        SELECT * FROM [todolist].[todoitems]

    ![查询 SQL 数据库中存储的项的](./media/mobile-services-dotnet-backend-view-sql-data/sql-azure-query.png)

    注意，该表包含 Id、 __createdAt、 __updatedAt 和 __version 列。 这些列支持脱机数据同步和[EntityData](http://msdn.microsoft.com/library/microsoft.windowsazure.mobile.service.entitydata.aspx)在基类中实现。 有关详细信息，请参阅 [开始使用脱机数据同步]。