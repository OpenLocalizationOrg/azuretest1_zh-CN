---
ms.openlocfilehash: b6902d9f267fdf5c501b1ca58bfe011cd94ccb78
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
现在，该应用程序已更新为使用移动服务的后端存储，可以对移动服务，使用 Android 模拟器或 Android 电话对其进行测试。

1. 从**运行**菜单上，单击**运行**以启动项目。

    这将执行您的应用程序，Android sdk，使用客户端库将发送一个查询，返回的项目，从您的移动服务生成。

5. 像以前一样，键入有意义的文本，然后单击**添加**。

    将新项插入发送给移动服务。

    您可以重新启动该应用程序以查看所做的更改已保存到 Azure 中的数据库。 您还可以检查使用 Azure 管理门户数据库︰ 在接下来的两个步骤执行此操作以查看数据库中所做的更改。


4. 在 Azure 管理门户中，单击与您的移动服务关联的数据库管理。

    ![](./media/mobile-services-dotnet-backend-windows-store-dotnet-get-started-data/manage-sql-azure-database.png)

5. 管理门户中执行查询以查看 Windows 应用商店应用程序所做的更改。 您的查询将会类似于下面的查询，但使用数据库的名称而不是`todolist`。

        SELECT * FROM [todolist].[todoitems]

    ![](./media/mobile-services-dotnet-backend-windows-store-dotnet-get-started-data/sql-azure-query.png)

Android 为到此结束**有关数据入门**教程。