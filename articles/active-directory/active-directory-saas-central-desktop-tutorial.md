---
ms.openlocfilehash: f88ca0b90c14320a9a412995261e5e9b08904af8
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties pageTitle="教程︰ Azure Active Directory 集成与中央桌面 |Microsoft Azure" description="了解如何使用 Azure Active Directory 中心桌面启用单一登录、 自动化资源调配，和更多。" services="active-directory" authors="MarkusVi"  documentationCenter="na" manager="stevenpo"/>
<tags ms.service="active-directory" ms.devlang="na" ms.topic="article" ms.tgt_pltfrm="na" ms.workload="identity" ms.date="08/01/2015" ms.author="markvi" />
#教程︰ Azure Active Directory 集成与中央桌面
>[AZURE.TIP]反馈，请单击[此处](http://go.microsoft.com/fwlink/?LinkId=522411)。

本教程的目的是显示的 Azure 和中央桌面集成。 在本教程中介绍的方案假定您已经有以下各项︰

-   有效的 Azure 订购
-   中央桌面单一登录启用订阅中心桌面租户 /

在本教程中介绍的方案由以下构造块组成︰

1.  实现为中心的桌面应用程序集成
2.  配置单一登录
3.  配置用户设置
4.  分配用户

![方案](./media/active-directory-saas-central-desktop-tutorial/IC769558.png "Scenario")
##实现为中心的桌面应用程序集成

本部分的目的是概述如何启用中央的桌面应用程序的集成。

###若要启用中央的桌面应用程序的集成，请执行以下步骤︰

1.  在 Azure 管理门户中，在左侧的导航窗格中，单击**活动目录**。

    ![活动目录](./media/active-directory-saas-central-desktop-tutorial/IC700993.png "Active Directory")

2.  从**目录**列表中，选择要为其启用目录集成的目录。

3.  若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序](./media/active-directory-saas-central-desktop-tutorial/IC700994.png "Applications")

4.  单击页面底部的**添加**。

    ![添加应用程序](./media/active-directory-saas-central-desktop-tutorial/IC749321.png "Add application")

5.  在**您想要执行**对话框中，单击**添加应用程序从库**。

    ![从 gallerry 中添加应用程序](./media/active-directory-saas-central-desktop-tutorial/IC749322.png "Add an application from gallerry")

6.  在**搜索框**中，键入**桌面中心**。

    ![应用程序库](./media/active-directory-saas-central-desktop-tutorial/IC769559.png "Application gallery")

7.  在结果窗格中，选择**中心桌面**，然后单击**完成**添加应用程序。

    ![集中的桌面](./media/active-directory-saas-central-desktop-tutorial/IC769560.png "Central Desktop")
##配置单一登录

本部分的目的是概述如何使用户能够使用基于 SAML 协议联合 Azure AD 中使用他们的帐户验证到中央桌面。  
作为此过程的一部分，您都需要将 base 64 编码的证书上传到中央桌面租户。  
如果您不熟悉此过程，请参阅[如何将转换到一个文本文件的二进制证书](http://youtu.be/PlgrzUZ-Y1o)。

Nase

###若要配置单一登录，请执行以下步骤︰

1.  在 Azure 广告门户中，在**中央的桌面**应用程序集成页上，单击**配置单一登录**来打开**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-central-desktop-tutorial/IC749323.png "Configure single sign-on")

2.  在**您想怎样来登录到中心桌面的用户**页上**Microsoft Azure AD 单一登录**，选择，然后单击**下一步**。

    ![配置单一登录](./media/active-directory-saas-central-desktop-tutorial/IC777628.png "Configure single sign-on")

3.  在**配置应用程序 URL**页上，执行以下步骤，，然后单击**下一步**: 

    -   在**中央桌面登录在 URL**文本框中，键入中心桌面租户的 URL (例如︰ *http://contoso.centraldesktop.com*)。
    -   在中央桌面回复 URL 文本框中，键入您中央的桌面 AssertionConsumerService URL (例如︰ https://contoso.centraldesktop.com/saml2-assertion.php)。

    >[AZURE.NOTE] 可以从桌面的中心元数据中获取的值 (例如︰ *http://contoso.centraldesktop.com*)。

    ![配置应用程序的 URL](./media/active-directory-saas-central-desktop-tutorial/IC769561.png "Configure app URL")

4.  在**配置单一登录中心桌面在**页上，下载您的证书，单击**下载证书**，然后将保存在您的计算机上的证书文件。

    ![配置单一登录](./media/active-directory-saas-central-desktop-tutorial/IC769562.png "Configure single sign-on")

5.  登录到**桌面中心**租户。

6.  转到**设置**，单击**高级**，然后单击**单一登录**。

    ![设置-高级](./media/active-directory-saas-central-desktop-tutorial/IC769563.png "Setup - Advanced")

7.  在**单一号上设置**页上，请执行以下步骤︰

    ![单一登录设置](./media/active-directory-saas-central-desktop-tutorial/IC769564.png "Single Sign On Settings")

    1.  选择**启用 SAML v2 单一签入**。
    2.  在 Azure 广告门户中，在**配置单一登录中心桌面在**页上，将**颁发者 URL**值，复制，然后将其粘贴到**SSO URL**文本框。
    3.  在 Azure 广告门户中，**配置单一登录中心桌面在**页上复制的**远程登录 URL**值，，然后将其粘贴到**SSO 登录 URL**文本框。
    4.  在 Azure 广告门户中，**配置单一登录中心桌面在**页上复制**单个 Sign-Out 服务 URL**值，，然后将其粘贴到**SSO 注销 URL**文本框。

8.  在**邮件签名验证方法**部分中，执行以下步骤︰

    ![消息签名验证方法](./media/active-directory-saas-central-desktop-tutorial/IC769565.png "Message Signature Verification Method")

    1.  选择**证书**。
    2.  从**SSO 证书**列表中，选择**RSH SHA256**。
    3.  从下载证书创建一个文本文件，复制内容的文本文件，然后将其粘贴到**SSO 证书**字段。  

        >[AZURE.TIP] 有关详细信息，请参阅[如何将转换到一个文本文件的二进制证书](http://youtu.be/PlgrzUZ-Y1o)

    4.  选择**显示 SAMLv2 登录页的链接**。

9.  单击**更新**。

10. 在 Azure 广告门户，选择单一登录配置进行确认，然后单击**完成**关闭**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-central-desktop-tutorial/IC769566.png "Configure single sign-on")
##配置用户设置

AAD 用户能够登录，他们必须向中央的桌面应用程序设置。 本部分介绍如何创建在中央桌面 AAD 的用户帐户。

###若要设置用户帐户添加到集中的桌面︰

1.  登录到桌面中心租户。

2.  转到**人\>内部成员**。

3.  单击**添加内部成员**。

    ![人脉](./media/active-directory-saas-central-desktop-tutorial/IC781051.png "People")

4.  在**新成员的电子邮件地址**文本框中，键入 AAD 所需帐户设置，然后单击**下一步**。

    ![新成员的电子邮件地址](./media/active-directory-saas-central-desktop-tutorial/IC781052.png "Email Addresses of New Members")

5.  单击**添加内部成员**。

    ![添加内部成员](./media/active-directory-saas-central-desktop-tutorial/IC781053.png "Add Internal Member")

    >[AZURE.NOTE] 您已经添加的用户将收到包含他们需要单击以激活该帐户的确认链接的电子邮件。

>[AZURE.NOTE] 您可以使用任何其他中心桌面用户帐户创建工具或 Api 来设置 AAD 用户帐户提供中央桌面

##分配用户

若要测试您的配置，您需要将 Azure 的 AD 用户授予您要允许使用通过将他们分配到您应用程序的访问权限。

###若要将用户分配到中心的桌面，请执行以下步骤︰

1.  在 Azure 广告门户中，创建一个测试帐户。

2.  在**中央的桌面**应用程序集成页上，单击**将用户分配**。

    ![分配用户](./media/active-directory-saas-central-desktop-tutorial/IC769567.png "Assign users")

3.  选择测试用户，单击**分配**，然后单击**是**以确认您的分配。

    ![是](./media/active-directory-saas-central-desktop-tutorial/IC767830.png "Yes")

如果您想要测试单个登录设置，打开后盖。 关于访问面板的详细信息，请参阅[访问权限面板简介](https://msdn.microsoft.com/library/dn308586)。

测试
