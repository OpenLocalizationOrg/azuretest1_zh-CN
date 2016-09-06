---
ms.openlocfilehash: e6fdc6034887a88ef57d7753b1b0023d2d6764b3
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
1. 在**解决方案资源管理器**中右击项目 （而不是解决方案），然后单击**发布**。 

    ![项目发布菜单选项](./media/app-service-api-pub-web-create/20-publish-gesture-v3.png)

2. 单击**配置文件**选项卡并单击**Microsoft Azure API 应用程序 （预览）**。 

    ![发布 Web 对话框](./media/app-service-api-pub-web-create/21-select-api-apps-for-deployment-v2.png)

3. 单击**新建**设置 Azure 订购新 API 应用程序。

    ![选择现有的 API 服务对话框](./media/app-service-api-pub-web-create/23-publish-to-apiapps-v3.png)

4. 在**创建 API 的应用程序**对话框中，输入以下命令︰

    - **API 的应用程序名称**，输入您在本教程使用的名称。 
    - 如果您有多个 Azure 的订阅，则选择要使用的一个。
    - 对于**应用程序的服务方案**，从您现有的应用程序服务计划，选择或选择**创建新的应用程序服务计划**并输入一个新的计划的名称。 
    - 对于**资源组**，请从您现有的资源组中选择或**创建新的资源组**中选择和输入一个名称。 
    - **访问级别**，请选择**对任何人都可用**。 您可以限制访问以后通过 Azure 预览门户。
    - **区域**，选择靠近您的区域。  

    ![配置 Microsoft Azure Web 应用程序对话框](./media/app-service-api-pub-web-create/24-new-api-app-dialog-v3.png)

5. 单击**确定**以创建 API 的应用程序在您的订阅。 

    此过程可能需要几分钟的时间，Visual Studio 将显示一个确认对话框。  

6. 在确认对话框中单击**确定**。 
 
    设置过程在 Azure 订阅创建资源组和 API 的应用程序。 Visual Studio 在**Azure 服务活动应用程序**窗口中将显示进度。 

    ![通过 Azure 应用程序服务活动窗口的状态通知](./media/app-service-api-pub-web-create/26-provisioning-success-v3.png)