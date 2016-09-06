---
ms.openlocfilehash: a201d13a406e6a4c9b4139606df74701f8297f5c
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
在本节中，您将使用 Visual Studio 在 IIS Express 开发工作站上的本地主机移动服务。 然后您将测试应用程序和后端服务。


1. 在 Visual Studio，按 F7 键或单击**生成解决方案**从**生成**菜单来构建 Windows 应用商店应用程序和移动服务。 验证这两个项目生成，没有错误在 Visual Studio 的输出窗口

2. 在 Visual Studio，按 F5 键，或单击从**调试**菜单来运行应用程序和主机在本地 IIS Express 的移动服务的**启动调试**。 

 
3. 输入新 todoitem 的文本。 然后单击**保存**。 这移动在 IIS Express 本地承载的服务创建的数据库中插入新的 todoItem。 

    ![](./media/mobile-services-dotnet-backend-test-local-service-data/new-local-todoitem.png)

4. 单击复选框以将其标记为已完成的项目之一。

    ![](./media/mobile-services-dotnet-backend-test-local-service-data/local-item-checked.png)

5. 在 Visual Studio 中可以通过打开服务器资源管理器并展开数据连接的后端服务创建的数据库中查看所做的更改。 右击下**MS_TableConnectionString**的 TodoItems 表，单击**显示表格数据**

    ![](./media/mobile-services-dotnet-backend-test-local-service-data/vs-show-local-table-data.png)