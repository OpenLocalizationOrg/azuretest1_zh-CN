---
ms.openlocfilehash: 7038f07dca511686a4eacd9063828a01acb902f7
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---


请按照以下步骤创建新的移动服务。

1.  登录到[管理门户]。 在导航窗格的底部，单击**+ 新建**。 展开**计算**和**手机信息服务**，然后单击**创建**。

    ![](./media/mobile-services-create-new-service/mobile-create.png)

    这将显示**移动服务**对话框。

2.  在**移动服务**对话框，选择**创建一个免费 20 MB SQL 数据库**，选择**JavaScript**运行时，然后在**URL**文本框中键入新的移动服务的子域名称。 单击向右箭头按钮，以转至下一个页面。

    ![](./media/mobile-services-create-new-service/mobile-create-page1.png)

    这将显示**指定数据库设置**页。
    
    >[AZURE.NOTE]作为本教程中，您可以创建新的 SQL 数据库实例和服务器。 您可以重用此新数据库并管理它，就像任何其他 SQL 数据库实例。 如果您已经有数据库在同一个地区作为新的移动服务，可以选择**使用现有的数据库**，然后选择该数据库。 使用在其他地区数据库的最好不要因为额外带宽成本和较长的延迟。

3.  在**名称**键入新数据库的名称，然后键入**登录名**，这是新的 SQL 数据库服务器的管理员登录名、 键入和确认密码，然后单击检查按钮以完成此过程。
    ![](./media/mobile-services-create-new-service/mobile-create-page2.png)

您现在已经创建了一种新的移动服务，可通过移动应用程序。



<!-- URLs. -->
[管理门户]: https://manage.windowsazure.com/
