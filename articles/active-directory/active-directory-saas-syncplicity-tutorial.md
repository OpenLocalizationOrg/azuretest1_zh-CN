---
ms.openlocfilehash: e8941db9f534038e4057fa26a36e9125b821716b
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties pageTitle="教程︰ Azure Active Directory 集成与 Syncplicity |Microsoft Azure" description="了解如何使用 Syncplicity Azure Active Directory 以启用单一登录、 自动化资源调配，和更多。" services="active-directory" authors="MarkusVi"  documentationCenter="na" manager="stevenpo"/>
<tags ms.service="active-directory" ms.devlang="na" ms.topic="article" ms.tgt_pltfrm="na" ms.workload="identity" ms.date="08/01/2015" ms.author="markvi" />
#教程︰ 与 Syncplicity 的 Azure Active Directory 集成
>[AZURE.TIP]反馈，请单击[此处](http://go.microsoft.com/fwlink/?LinkId=522417)。
  
本教程的目的是显示如何将 Azure 活动目录 (AAD) 和 Syncplicity 之间的单一登录设置。
  
在本教程中介绍的方案假定您已经有以下各项︰

-   有效的 Azure 订购
-   Syncplicity 租户
  
在完成本教程之后, AAD 用户对其具有分配的 Syncplicity 不会访问能一号到 Syncplicity 公司网站 （服务提供程序初始化登录），或使用 AAD 访问权限面板应用程序。

1.  启用应用程序集成为 Syncplicity
2.  配置单一登录
3.  配置用户设置
4.  分配用户

![方案](./media/active-directory-saas-syncplicity-tutorial/IC769524.png "Scenario")

##启用应用程序集成为 Syncplicity
  
本部分的目的是概述如何启用应用程序集成为 Syncplicity。

###若要启用应用程序集成为 Syncplicity，请执行以下步骤︰

1.  在 Azure 管理门户中，在左侧的导航窗格中，单击**活动目录**。

    ![活动目录](./media/active-directory-saas-syncplicity-tutorial/IC700993.png "Active Directory")

2.  从**目录**列表中，选择要为其启用目录集成的目录。

3.  若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序](./media/active-directory-saas-syncplicity-tutorial/IC700994.png "Applications")

4.  单击页面底部的**添加**。

    ![添加应用程序](./media/active-directory-saas-syncplicity-tutorial/IC749321.png "Add application")

5.  在**您想要执行**对话框中，单击**添加应用程序从库**。

    ![从 gallerry 中添加应用程序](./media/active-directory-saas-syncplicity-tutorial/IC749322.png "Add an application from gallerry")

6.  在**搜索框**中，键入**Syncplicity**。

    ![Syncplicity 应用程序库](./media/active-directory-saas-syncplicity-tutorial/IC769532.png "Syncplicity application gallery")

7.  在结果窗格中，选择**Syncplicity**，，然后单击**完成**添加应用程序。

    ![Syncplicity](./media/active-directory-saas-syncplicity-tutorial/IC769533.png "Syncplicity")

##配置单一登录
  
这一节概述如何使用户能够与他们 Azure Active Directory，使用联合身份验证基于 SAML 协议中的帐户验证到 Syncplicity。

###若要配置单一登录，请执行以下步骤︰

1.  在 Azure 广告门户中， **Syncplicity**应用程序集成在页上，单击**配置单一登录**来打开**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-syncplicity-tutorial/IC769534.png "Configure single sign-on")

2.  在**您想怎样来登录到 Syncplicity 的用户**页上**Microsoft Azure AD 单一登录**，选择，然后单击**下一步**。

    ![Microsoft Azure 广告单登录](./media/active-directory-saas-syncplicity-tutorial/IC769535.png "Microsoft Azure AD Single Sign-On")

3.  在**配置应用程序 URL**页上，在**Syncplicity 号中 URL**文本框中，单击**下一步**使用的 URL 的用户登录到您的 Syncplicity 应用程序的类型。 

    应用程序 URL 为您 Syncplicity 租户的 URL (例如︰ *http://company.Syncplicity.com*):

    ![配置应用程序的 URL](./media/active-directory-saas-syncplicity-tutorial/IC769536.png "Configure app URL")

4.  在**配置单一登录 Syncplicity 在**页上，下载您的证书，单击**下载证书**，然后将证书文件本地保存到您的计算机。

    ![配置单一登录](./media/active-directory-saas-syncplicity-tutorial/IC769543.png "Configure single sign-on")

5.  登录到**Syncplicity**租户。

6.  在顶部菜单中，单击**管理**，选择**设置**，然后单击**自定义域名和单一登录**。

    ![Syncplicity](./media/active-directory-saas-syncplicity-tutorial/IC769545.png "Syncplicity")

7.  在**单一登录 (SSO)**对话框页面上，请执行以下步骤︰

    ![单一登录\(SSO\)](./media/active-directory-saas-syncplicity-tutorial/IC769550.png "Single Sign-On \(SSO\)")

    1.  在**自定义域**文本框中，键入您的域的名称。
    2.  作为**单一登录状态**，选择**启用**。
    3.  在 Microsoft Azure 门户中，在**配置单一登录 Syncplicity 在**页上，将**实体 ID**值，复制，然后将其粘贴到**实体 Id**文本框。
    4.  在 Microsoft Azure 门户中，在**配置单一登录 Syncplicity 在**页上，将**单一登录服务 URL**值，复制，然后将其粘贴到**登录页的 URL**文本框。
    5.  在 Microsoft Azure 门户中，在**配置单一登录 Syncplicity 在**页上，**远程注销 URL**值，复制，然后将其粘贴到**注销页 URL**文本框。
    6.  中**标识提供者证书**，单击**选择文件**，然后上载您从 Microsoft Azure 门户下载的证书。
    7.  单击**保存更改**。

8.  在 Azure 广告门户，选择单一登录配置进行确认，然后单击**完成**关闭**配置单一登录**对话框。

    ![确认](./media/active-directory-saas-syncplicity-tutorial/IC769554.png "Confirmation")

##配置用户设置
  
AAD 用户能够登录，他们必须为 Syncplicity 应用程序设置。 本部分介绍如何在 Syncplicity 中创建 AAD 的用户帐户。

###若要设置到 Syncplicity 的用户帐户，请执行以下步骤︰

1.  登录到**Syncplicity**租户中 (例如︰ *https://company.Syncplicity.com*)。

2.  单击**管理员**，然后选择**用户帐户**。

3.  单击**添加用户**。

    ![管理用户](./media/active-directory-saas-syncplicity-tutorial/IC769764.png "Manage Users")

4.  键入您希望调配为**角色**选择**用户**，然后单击**下一步**的 AAD 帐户的**电子邮件地址**。

    ![帐户信息](./media/active-directory-saas-syncplicity-tutorial/IC769765.png "Account Information")

    >[AZURE.NOTE] AAD 帐户持有人将获得包括链接确认并激活该帐户的电子邮件。

5.  公司新用户应成为成员，然后单击**下一步**中选择的组。

    ![组成员身份](./media/active-directory-saas-syncplicity-tutorial/IC769772.png "Group Membership")

    >[AZURE.NOTE] 如果不没有列出任何组，只需单击**下一步**。

6.  选择您想要在用户的计算机上的 Syncplicity 的控制之下，然后单击**下一步**的文件夹。

    ![Syncplicity 文件夹](./media/active-directory-saas-syncplicity-tutorial/IC769773.png "Syncplicity Folders")

>[AZURE.NOTE] 您可以使用任何其他 Syncplicity 用户帐户创建工具或 Api 来设置 AAD 用户帐户由 Syncplicity 提供。

##分配用户
  
若要测试您的配置，您需要将 Azure 的 AD 用户授予您要允许使用通过将他们分配到您应用程序的访问权限。

###要将用户分配到 Syncplicity 中，执行以下步骤︰

1.  在 Azure 广告门户中，创建一个测试帐户。

2.  在**Syncplicity**应用程序集成页上，单击**将用户分配**。

    ![分配用户](./media/active-directory-saas-syncplicity-tutorial/IC769557.png "Assign users")

3.  选择测试用户，单击**分配**，然后单击**是**以确认您的分配。

    ![是](./media/active-directory-saas-syncplicity-tutorial/IC767830.png "Yes")
  
如果您想要测试单个登录设置，打开后盖。 关于访问面板的详细信息，请参阅[访问权限面板简介](https://msdn.microsoft.com/library/dn308586)。


测试
