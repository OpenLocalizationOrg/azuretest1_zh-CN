---
ms.openlocfilehash: edcd614625eed1270602ef48baea3da123774972
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties pageTitle="教程︰ Azure Active Directory 集成与 CloudBees |Microsoft Azure" description="了解如何使用 CloudBees Azure Active Directory 以启用单一登录、 自动化资源调配，和更多。" services="active-directory" authors="MarkusVi"  documentationCenter="na" manager="stevenpo"/>
<tags ms.service="active-directory" ms.devlang="na" ms.topic="article" ms.tgt_pltfrm="na" ms.workload="identity" ms.date="08/01/2015" ms.author="markvi" />
#教程︰ 与 CloudBees 的 Azure Active Directory 集成
>[AZURE.TIP]反馈，请单击[此处](http://go.microsoft.com/fwlink/?LinkId=528737)。

本教程的目的是显示 Azure 和 CloudBees 的集成。
在本教程中介绍的方案假定您已经有以下各项︰

-   有效的 Azure 订购
-   CloudBees 租户

后学完本教程，您已分配给 CloudBees Azure AD 用户将能够到 CloudBees 公司网站 （服务提供程序初始化登录），或使用[简介访问权限面板](https://msdn.microsoft.com/library/azure/dn308586.aspx)应用程序的单一登录

在本教程中介绍的方案由以下构造块组成︰

1.  启用应用程序集成为 CloudBees
2.  配置单一登录
3.  配置用户设置
4.  分配用户

![方案](./media/active-directory-saas-cloudbees-tutorial/IC790415.png "Scenario")

##启用应用程序集成为 CloudBees

本部分的目的是概述如何启用应用程序集成为 CloudBees。

###若要启用应用程序集成为 CloudBees，请执行以下步骤︰

1.  在 Azure 管理门户中，在左侧的导航窗格中，单击**活动目录**。

    ![活动目录](./media/active-directory-saas-cloudbees-tutorial/IC700993.png "Active Directory")

2.  从**目录**列表中，选择要为其启用目录集成的目录。

3.  若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序](./media/active-directory-saas-cloudbees-tutorial/IC700994.png "Applications")

4.  单击页面底部的**添加**。

    ![添加应用程序](./media/active-directory-saas-cloudbees-tutorial/IC749321.png "Add application")

5.  在**您想要执行**对话框中，单击**添加应用程序从库**。

    ![从 gallerry 中添加应用程序](./media/active-directory-saas-cloudbees-tutorial/IC749322.png "Add an application from gallerry")

6.  在**搜索框**中，键入**CloudBees**。

    ![应用程序库](./media/active-directory-saas-cloudbees-tutorial/IC790416.png "Application Gallery")

7.  在结果窗格中，选择**CloudBees**，，然后单击**完成**添加应用程序。

    ![Cloudbees](./media/active-directory-saas-cloudbees-tutorial/IC790417.png "Cloudbees")

##配置单一登录

本部分的目的是概述如何使用户能够使用基于 SAML 协议联合 Azure AD 中使用他们的帐户验证到 CloudBees。  
CloudBees 为单一登录配置需要检索证书的指纹值。  
 如果您不熟悉此过程，请参阅[如何检索证书的指纹值](http://youtu.be/YKQF266SAxI)。

###若要配置单一登录，请执行以下步骤︰

1.  在 Azure 广告门户中， **CloudBees**应用程序集成在页上，单击**配置单一登录**来打开**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-cloudbees-tutorial/IC790418.png "Configure Single Sign-On")

2.  在**您想怎样来登录到 CloudBees 的用户**页上**Microsoft Azure AD 单一登录**，选择，然后单击**下一步**。

    ![配置单一登录](./media/active-directory-saas-cloudbees-tutorial/IC790419.png "Configure Single Sign-On")

3.  **配置应用程序 URL**页面上的**CloudBees 号上的 URL**文本框中，键入用户用于登录到 CoudBees 您的 URL (例如︰ *https://grandcentral.cloudbees.com/login*"，然后单击**下一步**。

    ![配置应用程序的 URL](./media/active-directory-saas-cloudbees-tutorial/IC790420.png "Configure App URL")

4.  在**配置单一登录 CloudBees 在**页上，以下载您的证书，请单击**下载证书**，然后在您的计算机上本地保存证书。

    ![配置单一登录](./media/active-directory-saas-cloudbees-tutorial/IC790421.png "Configure Single Sign-On")

5.  在不同的 web 浏览器窗口中，以管理员身份登录到 CloudBees 公司网站。

6.  在菜单上，单击**设置**。

    ![Settings](./media/active-directory-saas-cloudbees-tutorial/IC790422.png "Settings")

7.  单击**SSO 集成**选项卡。

8.  在**设置了 SAML 2.0 单一登录**部分中，执行以下步骤︰

    ![安装单一登录](./media/active-directory-saas-cloudbees-tutorial/IC790423.png "Setup Single Sign-On")

    1.  在 Azure 广告门户中，**配置单一登录 CloudBees 在**页上复制的**远程登录 URL**值，，然后将其粘贴到**远程登录 URL**文本框。
    2.  从导出的证书，复制的**指纹**值，然后将其粘贴到文本框中**现有的证书指纹**。
    
        >[AZURE.TIP]有关详细信息，请参阅[如何检索证书的指纹值](http://youtu.be/YKQF266SAxI)

    3.  在**身份验证域**文本框中，键入您的公司的域。
    4.  单击**保存**。

9.  在 Azure 广告门户，选择单一登录配置进行确认，然后单击**完成**关闭**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-cloudbees-tutorial/IC790424.png "Configure Single Sign-On")

##配置用户设置

为了使 Azure AD 用户能够登录到**CloudBees**，他们必须到**CloudBees**配置。  
 对于**CloudBees**，资源调配是手动任务。

###若要设置到 CloudBees 的用户帐户，请执行以下步骤︰

1.  以管理员身份登录到您的**CloudBees**公司网站。

2.  转到**编辑的用户**。

    ![用户](./media/active-directory-saas-cloudbees-tutorial/IC790429.png "Users")

3.  单击**添加**。

    ![Add](./media/active-directory-saas-cloudbees-tutorial/IC790430.png "Add")

4.  在**添加用户**部分中，执行以下步骤︰

    ![添加用户](./media/active-directory-saas-cloudbees-tutorial/IC790431.png "Add User")

    1.  键入**的电子邮件**、**名字**、**姓氏**和有效 Azure Active Directory 帐户要到相关文本框设置其他属性。
    2.  单击**添加用户**。

        >[AZURE.NOTE]登录说明的电子邮件将会发送到该帐户的所有者。

>[AZURE.NOTE]您可以使用任何其他 CloudBees 用户帐户创建工具或 Api 来设置 AAD 用户帐户由 CloudBees 提供。

##分配用户

若要测试您的配置，您需要将 Azure 的 AD 用户授予您要允许使用通过将他们分配到您应用程序的访问权限。

###要将用户分配到 CloudBees 中，执行以下步骤︰

1.  在 Azure 广告门户中，创建一个测试帐户。

2.  在**CloudBees**应用程序集成页上，单击**将用户分配**。

    ![分配用户](./media/active-directory-saas-cloudbees-tutorial/IC790432.png "Assign Users")

3.  选择测试用户，单击**分配**，然后单击**是**以确认您的分配。

    ![是](./media/active-directory-saas-cloudbees-tutorial/IC767830.png "Yes")

如果您想要测试单个登录设置，打开后盖。 关于访问面板的详细信息，请参阅[访问权限面板简介](https://msdn.microsoft.com/library/azure/dn308586.aspx)。
测试
