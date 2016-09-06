---
ms.openlocfilehash: e84e601c409573c68813b78ff23a956450643e7b
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
>[AZURE.NOTE]若要完成此过程，您必须具有一个已验证电子邮件地址的 Google 帐户。 若要创建一个新的 Google 帐户，请转到<a href="http://go.microsoft.com/fwlink/p/?LinkId=268302" target="_blank">accounts.google.com</a>。


1. 定位到<a href="http://cloud.google.com/console" target="_blank">Google 云控制台</a>网站，登录与您的 Google 帐户凭据，然后单击**创建项目**。

    ![](./media/notification-hubs-android-get-started/mobile-services-google-new-project.png)   

    >[AZURE.NOTE]当已经有一个现有的项目时，您将被定向到<strong>项目</strong>后登录页。 若要从仪表板中创建一个新项目，展开<strong>API 项目</strong>，单击<strong>创建...</strong>在<strong>其他项目</strong>，然后输入项目名称，然后单击<strong>创建项目</strong>。

2. 输入项目名称、 接受服务的条款，单击**创建**。 如果需要，执行 SMS 验证过程，并再次单击**创建**。

3. 在**项目**部分中进行项目编号的注释。 

    在本教程后面部分与在客户端 PROJECT_ID 变量设置此值。

4. 在左侧列中，展开**Api 和身份验证**单击**Api** ，然后向下滚动，单击**邮件为 Android 的云**。 然后在下一页上单击**启用 API**并接受服务条款。 

    ![](./media/notification-hubs-android-get-started/mobile-services-google-enable-GCM.png)

5. 单击**凭据**，然后单击**创建新密钥** 

    ![](./media/notification-hubs-android-get-started/mobile-services-google-create-server-key.png)

6. 在**创建一个新密钥**，请单击**服务器密钥**。 在下一个窗口中单击**创建**。

    ![](./media/notification-hubs-android-get-started/mobile-services-google-create-server-key2.png)

7. 记下的**API 密钥**值。

    ![](./media/notification-hubs-android-get-started/mobile-services-google-create-server-key3.png) 

    您将使用此 API 密钥值来启用 Azure GCM 验证身份并发送推式通知代表您的应用程序。

