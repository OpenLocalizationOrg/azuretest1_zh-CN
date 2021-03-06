---
ms.openlocfilehash: 09cb8a651215dd592462e53928b37702c0cf12da
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties pageTitle="教程︰ Azure Active Directory 集成与 Bime |Microsoft Azure" description="了解如何使用 Azure Active Directory Bime 启用单一登录、 自动化资源调配，和更多。" services="active-directory" authors="MarkusVi"  documentationCenter="na" manager="stevenpo"/>
<tags ms.service="active-directory" ms.devlang="na" ms.topic="article" ms.tgt_pltfrm="na" ms.workload="identity" ms.date="08/01/2015" ms.author="markvi" />
#Bime 与教程︰ Azure Active Directory 集成
>[AZURE.TIP]反馈，请单击[此处](http://go.microsoft.com/fwlink/?LinkId=524328)。

本教程的目的是显示 Azure 和 Bime 集成。  
在本教程中介绍的方案假定您已经有以下各项︰

-   有效的 Azure 订购
-   Bime 租户

后学完本教程，您已分配给 Bime Azure AD 用户将能够到 Bime 公司网站 （服务提供程序初始化登录），或使用[简介访问权限面板](https://msdn.microsoft.com/library/dn308586)应用程序的单一登录

在本教程中介绍的方案由以下构造块组成︰

1.  启用应用程序集成为 Bime
2.  配置单一登录
3.  配置用户设置
4.  分配用户

![方案](./media/active-directory-saas-bime-tutorial/IC775552.png "Scenario")
##启用应用程序集成为 Bime

本部分的目的是概述如何启用 Bime 用于应用程序集成。

###若要启用 Bime 用于应用程序集成，请执行以下步骤︰

1.  在 Azure 管理门户中，在左侧的导航窗格中，单击**活动目录**。

    ![活动目录](./media/active-directory-saas-bime-tutorial/IC700993.png "Active Directory")

2.  从**目录**列表中，选择要为其启用目录集成的目录。

3.  若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序](./media/active-directory-saas-bime-tutorial/IC700994.png "Applications")

4.  单击页面底部的**添加**。

    ![添加应用程序](./media/active-directory-saas-bime-tutorial/IC749321.png "Add application")

5.  在**您想要执行**对话框中，单击**添加应用程序从库**。

    ![从 gallerry 中添加应用程序](./media/active-directory-saas-bime-tutorial/IC749322.png "Add an application from gallerry")

6.  在**搜索框**中，键入**Bime**。

    ![应用程序库](./media/active-directory-saas-bime-tutorial/IC775553.png "Application Gallery")

7.  在结果窗格中，选择**Bime**，，然后单击**完成**添加应用程序。

    ![Bime](./media/active-directory-saas-bime-tutorial/IC775554.png "Bime")
##配置单一登录

本部分的目的是概述如何使用户能够使用基于 SAML 协议联合 Azure AD 中使用他们的帐户验证到 Bime。  
Bime 为单一登录配置需要检索证书的指纹值。  
如果您不熟悉此过程，请参阅[如何检索证书的指纹值](http://youtu.be/YKQF266SAxI)。

###若要配置单一登录，请执行以下步骤︰

1.  在 Azure 广告门户， **Bime**应用程序集成在页上，单击**配置单一登录**来打开**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-bime-tutorial/IC771709.png "Configure single sign-on")

2.  在**您想怎样用户能够登录到 Bime**页上**Microsoft Azure AD 单一登录**，选择，然后单击**下一步**。

    ![配置单一登录](./media/active-directory-saas-bime-tutorial/IC775555.png "Configure Single Sign-On")

3.  在**配置应用程序 URL**页上，在**Bime 号在 URL**文本框中，键入您的 URL 使用以下模式"*https://\<租户名称\>。Bimeapp.com*"，然后单击**下一步**。

    ![配置应用程序的 URL](./media/active-directory-saas-bime-tutorial/IC775556.png "Configure App URL")

4.  在**配置单一登录 Bime 在**页上，下载您的证书，单击**证书下载**，然后保存为本地证书文件**c:\\Bime.cer**。

    ![配置单一登录](./media/active-directory-saas-bime-tutorial/IC775557.png "Configure Single Sign-On")

5.  在不同的 web 浏览器窗口中，以管理员身份登录到 Bime 公司网站。

6.  在工具栏中，单击**管理**，然后选择**帐户**。

    ![管理员](./media/active-directory-saas-bime-tutorial/IC775558.png "Admin")

7.  在帐户配置页，请执行以下步骤︰

    ![配置单一登录](./media/active-directory-saas-bime-tutorial/IC775559.png "Configure Single Sign-On")

    1.  选择**启用 SAML 身份验证**。
    2.  在 Azure 的门户中，在**配置单一登录在 Bime**对话框页面上的**远程登录 URL**值，复制，然后将其粘贴到**远程登录 URL**文本框。
    3.  从导出的证书，复制的**指纹**值，然后将其粘贴到**证书指纹**文本框。  

        >[AZURE.TIP] 有关详细信息，请参阅[如何检索证书的指纹值](http://youtu.be/YKQF266SAxI)

    4.  单击**保存**。

8.  在 Azure 广告门户，选择单一登录配置进行确认，然后单击**完成**关闭**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-bime-tutorial/IC775560.png "Configure Single Sign-On")
##配置用户设置

为了使 Azure AD 用户能够登录到 Bime，他们必须到 Bime 调配。  
Bime，在资源调配是手动任务。

###若要配置用户设置，请执行以下步骤︰

1.  登录到**Bime**租户。

2.  在工具栏中，单击**管理**，然后选择**用户**。

    ![管理员](./media/active-directory-saas-bime-tutorial/IC775561.png "Admin")

3.  在**用户列表**中，单击**添加新用户**（"+"）。

    ![用户](./media/active-directory-saas-bime-tutorial/IC775562.png "Users")

4.  在**用户详细信息**对话框页面上，请执行以下步骤︰

    ![用户详细信息](./media/active-directory-saas-bime-tutorial/IC775563.png "User Details")

    1.  请输入名字、 姓氏、 登录，您希望提供一个有效的 AAD 帐户的电子邮件。
    2.  单击保存。

>[AZURE.NOTE] 您可以使用任何其他 Bime 用户帐户创建工具或 Api 提供 Bime 来设置 AAD 用户帐户。

##分配用户

若要测试您的配置，您需要将 Azure 的 AD 用户授予您要允许使用通过将他们分配到您应用程序的访问权限。

###若要将用户分配到 Bime，请执行以下步骤︰

1.  在 Azure 广告门户中，创建一个测试帐户。

2.  **Bime**应用程序集成在页上，单击**将用户分配**。

    ![分配用户](./media/active-directory-saas-bime-tutorial/IC775564.png "Assign users")

3.  选择测试用户，单击**分配**，然后单击**是**以确认您的分配。

    ![是](./media/active-directory-saas-bime-tutorial/IC767830.png "Yes")

如果您想要测试单个登录设置，打开后盖。 关于访问面板的详细信息，请参阅[访问权限面板简介](https://msdn.microsoft.com/library/dn308586)。

测试
