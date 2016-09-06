---
ms.openlocfilehash: 4323829bf6500befcbe745ce358fb30df6d2f487
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---

苹果推送通知服务 (APNS) 使用证书进行身份验证的推式通知。 按照这些说明来创建要发送和接收通知的必要强制证书。 官方的 APNS 功能文档，请参阅[苹果推送通知服务](http://go.microsoft.com/fwlink/p/?LinkId=272584)。

##生成证书签名请求文件

首先您必须生成证书签名请求 (CSR) 文件中，苹果用于生成签名的强制证书。

1. 从**实用程序**文件夹或**其他**文件夹，请运行钥匙链访问工具。

2. 单击**访问钥匙链**，展开**证书助手**，然后单击**申请一个证书从证书颁发机构...**。

    ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-request-cert-from-ca.png)

3. 选择您的**用户的电子邮件地址**和**通用名称**，请确保选中**保存到磁盘**，然后单击**继续**。 将**CA 的电子邮件地址**字段留空，因为它不需要。

    ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-csr-info.png)

4. 在**另存为**中键入证书签名请求 (CSR) 文件的名称，**位置**，在选择的位置，然后单击**保存**。

    ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-save-csr.png)

    这将 CSR 文件保存在所选的位置;默认位置是桌面。 请记住，选择此文件的位置。

接下来，将向您的应用程序注册苹果、 启用推式通知和上载此导出的 CSR 创建推式证书。

##注册您的推送通知的应用程序

为了能够向 iOS 应用程序发送推式通知，必须向应用程序注册苹果并还推式通知的注册。  

1. 如果尚未注册您的应用程序，浏览到<a href="http://go.microsoft.com/fwlink/p/?LinkId=272456" target="_blank">Io 资源调配门户网站</a>在苹果开发人员中心，登录与您的苹果 ID，请单击**标识符**，然后单击**应用程序 Id**，并最后单击**+**号以注册新的应用程序。

    ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-ios-appids.png)


2. **说明应用程序 ID**名称中键入一个描述性的名称为您的应用程序。 

    **显式应用程序 ID**，在窗体中输入**包标识符** `<Organization Identifier>.<Product Name>` [应用程序分发指南](http://go.microsoft.com/fwlink/?LinkId=613485)中提到。 *组织标识符*和所使用的*产品名称*必须匹配的组织标识符和创建 XCode 项目时将使用的产品名称。 在下面的 screeshot *NotificationHubs*用作组织标识符和*GetStarted*用作产品名称。 确保此匹配的值将使用 XCode 项目中将允许您使用 XCode 正确的发布配置文件。 
    
    检查应用程序服务部分中的**推送通知**选项，然后单击**继续**。

    ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-new-appid-info.png)

    这将生成您的应用程序 ID，并要求您确认此信息。 单击**提交**


    ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-confirm-new-appid.png)


    一旦单击**提交**，您将看到**完成注册**屏幕中，如下所示。 单击**完成**。


    ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-appid-registration-complete.png)


3. 找到的应用程序 ID 仅创建，且所在的行上单击。

    ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-ios-appids2.png)

    单击应用程序 ID 将显示应用程序的详细信息。 单击**编辑**按钮。

    ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-edit-appid.png)

4. 滚动到屏幕的底部，单击**创建证书...**按钮下面的**开发推 SSL 证书**。

    ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-appid-create-cert.png)

    此时将显示"添加 iOS 证书"的助手。

    > [AZURE.NOTE] 本教程使用开发证书。 注册生产证书时使用相同的过程。 只需确保您使用相同的证书类型时发送通知。

5. 单击**选择文件**，浏览到您在第一项任务，该 CSR 文件的保存的位置，然后单击**生成**。

    ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-appid-cert-choose-csr.png)

6. 门户创建证书后，单击**下载**按钮，并单击**完成**。

    ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-appid-download-cert.png)

    此下载签名证书并将其保存到您的计算机中下载文件夹。

    ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-cert-downloaded.png)

    > [AZURE.NOTE] 默认情况下，下载的文件开发证书的名称为**aps_development.cer**。

7. 双击已下载的强制证书**aps_development.cer**。

    这将安装新的证书在钥匙链，如下所示︰

    ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-cert-in-keychain.png)

    > [AZURE.NOTE] 您的证书中的名称可能会有所不同，但它将带有前缀**苹果开发 iOS 推送服务︰**。

以后，您将使用此证书来生成.p12 文件以启用身份验证与 APNS。

##创建应用程序设置的配置文件

1. 回<a href="http://go.microsoft.com/fwlink/p/?LinkId=272456" target="_blank">Io 资源调配门户网站</a>，选择**设置的配置文件**，选择**所有**，然后单击**+**按钮以创建新的配置文件。 这将启动**添加 iOS Provisiong 配置文件**向导

    ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-new-provisioning-profile.png)

2. 选择正在**开发**的**iOS 应用程序开发**的 provisiong 配置文件类型，然后单击**继续**。 


3. 接下来，选择您刚创建**的应用程序 ID**下拉列表中，应用程序 ID，然后单击**继续**

    ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-select-appid-for-provisioning.png)


4. 在**选择证书**屏幕中，选择您用于代码签名的常规开发证书并单击**继续**。 这不是您刚才创建的推证书。

    ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-provisioning-select-cert.png)


5. 接下来，选择要用于测试的**设备**并单击**继续**

    ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-provisioning-select-devices.png)


6. 最后，在**配置文件名称**选择的配置文件的名称，单击**生成**并单击**完成**

    ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-provisioning-name-profile.png)


    这将创建新的设置配置文件。

    ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-provisioning-profile-ready.png)


