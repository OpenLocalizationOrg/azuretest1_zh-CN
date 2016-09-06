---
ms.openlocfilehash: 609634d29918766be3684de6f1779c712bd16496
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties pageTitle="教程︰ Azure Active Directory 集成与 Clarizen |Microsoft Azure" description="了解如何使用 Clarizen Azure Active Directory 以启用单一登录、 自动化资源调配，和更多。" services="active-directory" authors="MarkusVi"  documentationCenter="na" manager="stevenpo"/>
<tags ms.service="active-directory" ms.devlang="na" ms.topic="article" ms.tgt_pltfrm="na" ms.workload="identity" ms.date="08/01/2015" ms.author="markvi" />
#教程︰ 与 Clarizen 的 Azure Active Directory 集成
>[AZURE.TIP]反馈，请单击[此处](http://go.microsoft.com/fwlink/?LinkId=526793)。

本教程的目的是显示 Azure 和 Clarizen 的集成。  
在本教程中介绍的方案假定您已经有以下各项︰

-   有效的 Azure 订购
-   Clarizen-启用单一登录的订阅

后学完本教程，您已分配给 Clarizen Azure AD 用户将能够到 Clarizen 公司网站 （服务提供程序初始化登录），或使用[简介访问权限面板](https://msdn.microsoft.com/library/dn308586)应用程序的单一登录

在本教程中介绍的方案由以下构造块组成︰

1.  启用应用程序集成为 Clarizen
2.  配置单一登录
3.  配置用户设置
4.  分配用户

![方案](./media/active-directory-saas-clarizen-tutorial/IC784679.png "Scenario")
##启用应用程序集成为 Clarizen

本部分的目的是概述如何启用应用程序集成为 Clarizen。

###若要启用应用程序集成为 Clarizen，请执行以下步骤︰

1.  在 Azure 管理门户中，在左侧的导航窗格中，单击**活动目录**。

    ![活动目录](./media/active-directory-saas-clarizen-tutorial/IC700993.png "Active Directory")

2.  从**目录**列表中，选择要为其启用目录集成的目录。

3.  若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序](./media/active-directory-saas-clarizen-tutorial/IC700994.png "Applications")

4.  单击页面底部的**添加**。

    ![添加应用程序](./media/active-directory-saas-clarizen-tutorial/IC749321.png "Add application")

5.  在**您想要执行**对话框中，单击**添加应用程序从库**。

    ![从 gallerry 中添加应用程序](./media/active-directory-saas-clarizen-tutorial/IC749322.png "Add an application from gallerry")

6.  在**搜索框**中，键入**Clarizen**。

    ![应用程序库](./media/active-directory-saas-clarizen-tutorial/IC784680.png "Application Gallery")

7.  在结果窗格中，选择**Clarizen**，，然后单击**完成**添加应用程序。

    ![Clarizen](./media/active-directory-saas-clarizen-tutorial/IC784681.png "Clarizen")
##配置单一登录

本部分的目的是概述如何使用户能够使用基于 SAML 协议联合 Azure AD 中使用他们的帐户验证到 Clarizen。

###若要配置单一登录，请执行以下步骤︰

1.  在 Azure 广告门户中， **Clarizen**应用程序集成在页上，单击**配置单一登录**来打开**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-clarizen-tutorial/IC784682.png "Configure Single Sign-On")

2.  在**您想怎样来登录到 Clarizen 的用户**页上**Microsoft Azure AD 单一登录**，选择，然后单击**下一步**。

    ![配置单一登录](./media/active-directory-saas-clarizen-tutorial/IC784683.png "Configure Single Sign-On")

3.  在**配置单一登录 Clarizen 在**页上，下载您的证书，单击**下载证书**，然后将保存在您的计算机上的证书文件。

    ![配置单一登录](./media/active-directory-saas-clarizen-tutorial/IC784684.png "Configure Single Sign-On")

4.  在不同的 web 浏览器窗口中，登录到您作为管理员的**Clarizen**公司网站 (如︰ *https://app2.clarizen.com/Clarizen/Pages/Service/Login.aspx*)。

5.  单击您的用户名，然后单击**设置**。

    ![Settings](./media/active-directory-saas-clarizen-tutorial/IC784685.png "Settings")

6.  单击**全局设置**选项卡，然后单击**联合身份验证**中，旁边的**编辑**。

    ![全局设置](./media/active-directory-saas-clarizen-tutorial/IC786906.png "Global Settings")

7.  在**联合身份验证**对话框中，请执行以下步骤︰

    ![联合身份验证](./media/active-directory-saas-clarizen-tutorial/IC785892.png "Federated Authentication")

    1.  请单击**上载**以上载您已下载的证书。
    2.  在 Azure 的门户中，在**配置单一登录在 Clarizen**对话框页面上，将**单一登录服务 URL**值，复制，然后将其粘贴到**登录 URL**文本框。
    3.  在 Azure 的门户中，在**配置单 Clarizen 在注销**对话框页面上，复制**单个 Sign-Out 服务 URL**值，，然后将其粘贴到**Sign-out URL**文本框。
    4.  选择**使用 POST**。
    5.  单击**保存**。

8.  在 Azure 广告门户，选择单一登录配置进行确认，然后单击**完成**关闭**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-clarizen-tutorial/IC784688.png "Configure Single Sign-On")
##配置用户设置

为了使 Azure AD 用户能够登录到 Clarizen，他们必须到 Clarizen 配置。  
对于 Clarizen，资源调配是手动任务。

###若要设置用户帐户，请执行以下步骤︰

1.  以管理员身份登录到您的**Clarizen**公司网站。

2.  单击**用户**。

    ![人脉](./media/active-directory-saas-clarizen-tutorial/IC784689.png "People")

3.  单击**邀请用户**。

    ![邀请用户](./media/active-directory-saas-clarizen-tutorial/IC784690.png "Invite Users")

4.  在邀请用户对话框页面上，请执行以下步骤︰

    ![邀请其他人](./media/active-directory-saas-clarizen-tutorial/IC784691.png "Invite People")

    1.  在**电子邮件**文本框中，键入您要提供一个有效的 Azure Active Directory 帐户的电子邮件地址。
    2.  单击**邀请**。

    >[AZURE.NOTE] Azure Active Directory 帐户持有者将收到一封电子邮件，点击链接以确认他们的帐户，再将变为活动状态。

##分配用户

若要测试您的配置，您需要将 Azure 的 AD 用户授予您要允许使用通过将他们分配到您应用程序的访问权限。

###要将用户分配到 Clarizen 中，执行以下步骤︰

1.  在 Azure 广告门户中，创建一个测试帐户。

2.  在**Clarizen**应用程序集成页上，单击**将用户分配**。

    ![分配用户](./media/active-directory-saas-clarizen-tutorial/IC784692.png "Assign Users")

3.  选择测试用户，单击**分配**，然后单击**是**以确认您的分配。

    ![是](./media/active-directory-saas-clarizen-tutorial/IC767830.png "Yes")

如果您想要测试单个登录设置，打开后盖。 关于访问面板的详细信息，请参阅[访问权限面板简介](https://msdn.microsoft.com/library/dn308586)。

测试
