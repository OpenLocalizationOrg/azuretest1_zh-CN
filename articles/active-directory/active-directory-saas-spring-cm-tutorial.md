---
ms.openlocfilehash: 4782daa2a4f739e35985279f841b0d47359f7229
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties pageTitle="教程︰ 使用弹簧 CM 的 Azure Active Directory 集成 |Microsoft Azure" description="了解如何使用 Azure Active Directory 弹簧厘米以启用单一登录、 自动化资源调配，和更多。" services="active-directory" authors="MarkusVi"  documentationCenter="na" manager="stevenpo"/>
<tags ms.service="active-directory" ms.devlang="na" ms.topic="article" ms.tgt_pltfrm="na" ms.workload="identity" ms.date="08/01/2015" ms.author="markvi" />
#教程︰ 使用弹簧 CM 的 Azure Active Directory 集成
>[AZURE.TIP]反馈，请单击[此处](http://go.microsoft.com/fwlink/?LinkId=536554)。
  
本教程的目的是显示如何将 Azure Active Directory 和 SpringCM 之间的单一登录设置。
  
在本教程中介绍的方案假定您已经有以下各项︰

-   有效的 Azure 订购
-   SpringCM-启用单一登录的订阅
  
后学完本教程，您已分配给 SpringCM Azure Active Directory 用户将能够为单一登录使用 AAD 访问面板。

1.  启用应用程序集成为 SpringCM
2.  配置单一登录
3.  配置用户设置
4.  分配用户

![方案](./media/active-directory-saas-spring-cm-tutorial/IC797044.png "Scenario")

##启用应用程序集成为 SpringCM
  
本部分的目的是概述如何启用应用程序集成为 SpringCM。

###若要启用应用程序集成为 SpringCM，请执行以下步骤︰

1.  在 Azure 管理门户中，在左侧的导航窗格中，单击**活动目录**。

    ![活动目录](./media/active-directory-saas-spring-cm-tutorial/IC700993.png "Active Directory")

2.  从**目录**列表中，选择要为其启用目录集成的目录。

3.  若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序](./media/active-directory-saas-spring-cm-tutorial/IC700994.png "Applications")

4.  单击页面底部的**添加**。

    ![添加应用程序](./media/active-directory-saas-spring-cm-tutorial/IC749321.png "Add application")

5.  在**您想要执行**对话框中，单击**添加应用程序从库**。

    ![从 gallerry 中添加应用程序](./media/active-directory-saas-spring-cm-tutorial/IC749322.png "Add an application from gallerry")

6.  在**搜索框**中，键入**SpringCM**。

    ![应用程序库](./media/active-directory-saas-spring-cm-tutorial/IC797045.png "Application Gallery")

7.  在结果窗格中，选择**SpringCM**，，然后单击**完成**添加应用程序。

    ![SpringCM](./media/active-directory-saas-spring-cm-tutorial/IC797046.png "SpringCM")

##配置单一登录
  
这一节概述如何使用户能够与他们 Azure Active Directory，使用联合身份验证基于 SAML 协议中的帐户验证到 SpringCM。

###若要配置单一登录，请执行以下步骤︰

1.  在 Azure 广告门户中， **SpringCM**应用程序集成在页上，单击**配置单一登录**来打开**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-spring-cm-tutorial/IC797047.png "Configure single Sign-On")

2.  在**您想怎样来登录到 SpringCM 的用户**页上**Microsoft Azure AD 单一登录**，选择，然后单击**下一步**。

    ![配置单一登录](./media/active-directory-saas-spring-cm-tutorial/IC797048.png "Configure single Sign-On")

3.  在**配置应用程序 URL**页上，在**SpringCM 号上的 URL**文本框中，键入用户用于登录到 SpringCM 的应用程序的 URL，然后单击**下一步**。 

    应用程序 URL 为您 SpringCM 租户的 URL (例如︰ *https://na11.springcm.com/atlas/SSO/SSOEndpoint.ashx?aid=16826*):

    ![配置应用程序的 URL](./media/active-directory-saas-spring-cm-tutorial/IC797049.png "Configure App URL")

4.  在**配置单一登录 SpringCM 在**页上，下载您的证书，单击**下载证书**，然后将证书文件本地保存到您的计算机。

    ![配置单一登录](./media/active-directory-saas-spring-cm-tutorial/IC797050.png "Configure Single SignOn")

5.  在不同的 web 浏览器窗口中，登录到您的**SpringCM**公司网站以管理员的身份。

6.  在顶部菜单中，单击**转到**，单击**首选项**、，，然后，在**帐户首选参数**部分中，单击**SAML SSO**。

    ![SAML SSO](./media/active-directory-saas-spring-cm-tutorial/IC797051.png "SAML SSO")

7.  在标识提供程序配置部分中，执行以下步骤︰

    ![标识提供程序配置](./media/active-directory-saas-spring-cm-tutorial/IC797052.png "Identity Provider Configuration")

    1.  若要上载您已下载的 Azure Active Directory 证书，请单击**选择颁发者证书**或**更改颁发者证书**。
    2.  在 Microsoft Azure 门户中，在**配置单一登录 SpringCM 在**页上，将**颁发者 URL**值，复制，然后将其粘贴到**颁发者**文本框。
    3.  在 Microsoft Azure 门户中，在**配置单一登录 SpringCM 在**页上， **Singel 登录服务 URL**值，复制，然后将其粘贴到文本框中**启动服务提供商 (SP) 的终结点**。
    4.  作为**SAML 启用**，选择**启用**。
    5.  单击**保存**。

8.  在 Azure 广告门户，选择单一登录配置进行确认，然后单击**完成**关闭**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-spring-cm-tutorial/IC797053.png "Configure Single SignOn")

##配置用户设置
  
为了使 Azure Active Directory 用户能够登录到 SpringCM，他们必须被调配到 SpringCM。  
对于 SpringCM，资源调配是手动任务。

>[AZURE.NOTE] 有关详细信息，请参阅[创建和编辑 SpringCM 用户](http://knowledge.springcm.com/create-and-edit-a-springcm-user)

###若要设置到 SpringCM 的用户帐户，请执行以下步骤︰

1.  以管理员身份登录到您的**SpringCM**公司网站。

2.  单击**转到**，，然后单击**通讯簿**。

    ![创建用户](./media/active-directory-saas-spring-cm-tutorial/IC797054.png "Create User")

3.  单击**创建用户**。

4.  选择一个**用户角色**。

5.  选择**发送激活电子邮件**。

6.  键入名字、 姓氏和电子邮件地址有效的 Azure Active Directory 用户帐户要到相关文本框设置。

7.  将用户添加到**安全组**。

8.  单击**保存**。

>[AZURE.NOTE] 您可以使用任何其他 SpringCM 用户帐户创建工具或 Api 来设置 AAD 用户帐户由 SpringCM 提供。

##分配用户
  
若要测试您的配置，您需要将 Azure 的 AD 用户授予您要允许使用通过将他们分配到您应用程序的访问权限。

###要将用户分配到 SpringCM 中，执行以下步骤︰

1.  在 Azure 广告门户中，创建一个测试帐户。

2.  在**SpringCM**应用程序集成页上，单击**将用户分配**。

    ![分配用户](./media/active-directory-saas-spring-cm-tutorial/IC797055.png "Assign Users")

3.  选择测试用户，单击**分配**，然后单击**是**以确认您的分配。

    ![是](./media/active-directory-saas-spring-cm-tutorial/IC767830.png "Yes")
  
如果您想要测试单个登录设置，打开后盖。 关于访问面板的详细信息，请参阅[访问权限面板简介](https://msdn.microsoft.com/library/dn308586)。





测试
