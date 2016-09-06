---
ms.openlocfilehash: fbf3d59948b13222d320fcd09b6af85699216068
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---


接下来，您将创建新的移动服务将数据存储在内存列表。 请按照以下步骤创建新的移动服务。

1. 登录到[Azure 的管理门户](https://manage.windowsazure.com/)。 
2.  在导航窗格的底部，单击**+ 新建**。

    ![新加](./media/mobile-services-create-new-service-data/plus-new.png)

3.  展开**计算**和**手机信息服务**，然后单击**创建**。

    ![-创建移动的](./media/mobile-services-create-new-service-data/mobile-create.png)

    这将显示对话框中**新的移动服务**。

4.  在**创建移动服务**页中，选择**创建免费 20 MB 的 SQL 数据库**，然后在**URL**文本框中键入新的移动服务的子域名称并等待名称验证。 名称验证完成后，单击右箭头按钮以转到下一个页面。   

    ![移动创建 page1](./media/mobile-services-create-new-service-data/mobile-create-page1.png)

    这将显示**指定数据库设置**页。

    
    > [AZURE.NOTE] 作为本教程中，您可以创建新的 SQL 数据库实例和服务器。 您可以重用此新数据库并管理它，就像任何其他 SQL 数据库实例。 如果您已经有数据库在同一个地区作为新的移动服务，可以选择**使用现有的数据库**，然后选择该数据库。 使用在其他地区数据库的最好不要因为额外带宽成本和较长的延迟。

5.  在**名称**键入新数据库的名称，然后键入**登录名**，这是新的 SQL 数据库服务器的管理员登录名、 键入和确认密码，然后单击检查按钮以完成此过程。

    ![移动创建 page2](./media/mobile-services-create-new-service-data/mobile-create-page2.png)

    
    > [AZURE.NOTE] 当您提供的密码不符合最低要求或不匹配，则会显示警告。  
    >
    > 我们建议您记下的管理员登录名和密码，则指定;您将需要此信息，若要在以后重复使用 SQL 数据库实例或服务器。

您现在已经创建了一种新的移动服务，可通过移动应用程序。 接下来，您将添加一个新表中存储应用程序数据。 通过替换内存中的集合的应用程序将使用此表。

