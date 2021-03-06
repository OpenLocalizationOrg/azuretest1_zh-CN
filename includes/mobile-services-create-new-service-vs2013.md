---
ms.openlocfilehash: 6b053774f1dc4e94057cd8e8c2f5ab2ccb71c703
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---


以下步骤在 Azure 中创建新的移动服务，并将代码添加到将您的应用程序连接到此新的服务项目。 Visual Studio 2013年到 Azure 连接，以您的名义使用您提供的凭据创建新的移动服务。 创建新的移动服务时，必须指定一个移动服务用于存储应用程序数据的 Azure SQL 数据库。 


1. 在 Visual Studio 2013，打开解决方案资源管理器，用鼠标右键单击 Windows 应用商店应用程序项目，单击**添加**，然后单击**连接服务...**。  

2. 在服务管理器对话框中，单击**创建服务...**，然后选择**&lt;管理...&gt;**从在对话框中创建移动服务的**订阅**。  

    ![创建服务管理订阅](./media/mobile-services-create-new-service-vs2013/mobile-create-service-from-vs2013.png)

3. 管理 Microsoft Azure 订阅文档，请单击**签名...**若要登录到您的 Azure 帐户 （如果需要），请选择可用的订阅，然后单击**关闭**。

    当您的订阅已具有一个或多个现有的移动服务时，将显示服务名称。 

5. 返回在**创建移动服务**对话框中，选择您的**订阅**， **JavaScript**后端在**运行时**和您移动的服务，为**区域**，然后键入您的移动服务的**名称**。

    >[AZURE.NOTE]移动服务名称必须是唯一的。 一个红色的 X 显示**名称**旁边时提供的名称不可用。 

6. 在**数据库**中，选择**&lt;创建一个可用的 SQL 数据库&gt;**，提供**服务器的用户名**，**服务器密码**，并**确认服务器密码**，然后单击**创建**。

    ![在 VS 2013 中创建新的移动服务](./media/mobile-services-create-new-service-vs2013/mobile-create-service-from-vs2013-2.png)


    > [AZURE.NOTE]
    > 作为本教程中，您创建一个新的可用 SQL 数据库实例和服务器。 您可以重用此新数据库并管理它，就像任何其他 SQL 数据库实例。 您只能有一个可用的数据库实例。 如果您已经在同一个地区作为新的移动服务有一个数据库，您可以改为选择现有数据库。 当您选择一个现有的数据库时，请确保您提供正确的登录凭据。 如果提供不正确的登录凭据，移动服务创建处于不正常状态。

7. 创建了移动服务后，从服务管理器中的列表中选择新创建的移动服务并单击**确定**。

    向导完成后，安装所需的 NuGet 程序包、 对移动服务客户端库的引用添加到项目中，并更新项目源代码。
