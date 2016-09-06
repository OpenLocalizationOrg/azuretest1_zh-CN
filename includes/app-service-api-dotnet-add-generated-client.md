---
ms.openlocfilehash: b4aff71f78652623384d31196e969965f229cfaa
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
3. 在**解决方案资源管理器**中右击项目 （而不是解决方案），并选择**添加 > Azure API 应用程序客户端**。 

    ![](./media/app-service-api-dotnet-add-generated-client/03-add-azure-api-client-v3.png)
    
3. 在**添加 Azure API 应用程序客户端**对话框中，单击**下载从 Azure API 应用程序**。 

5. 从下拉列表中，选择您想要调用的 API 应用程序。 

7. 单击**确定**。 

    ![生成屏幕](./media/app-service-api-dotnet-add-generated-client/04-select-the-api-v3.png)

    向导下载 API 的元数据文件，并生成一个类型化的接口调用 API 的应用程序。

    ![发生生成](./media/app-service-api-dotnet-add-generated-client/05-metadata-downloading-v3.png)

    代码生成完成后，您将看到一个新的文件夹，可在**解决方案资源管理器**中使用 API 的应用程序的名称。 此文件夹包含实现的客户端类和数据模型的代码。 

    ![生成已完成](./media/app-service-api-dotnet-add-generated-client/06-code-gen-output-v3.png)
