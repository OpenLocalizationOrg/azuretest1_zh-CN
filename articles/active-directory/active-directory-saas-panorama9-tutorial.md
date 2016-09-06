---
ms.openlocfilehash: 48737678c823dbd8ce5bcb274dc713b8b3ed38e2
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties pageTitle="教程︰ Azure Active Directory 集成与 Panorama9 |Microsoft Azure" description="了解如何使用 Panorama9 Azure Active Directory 以启用单一登录、 自动化资源调配，和更多。" services="active-directory" authors="MarkusVi"  documentationCenter="na" manager="stevenpo"/>
<tags ms.service="active-directory" ms.devlang="na" ms.topic="article" ms.tgt_pltfrm="na" ms.workload="identity" ms.date="08/01/2015" ms.author="markvi" />
#教程︰ 与 Panorama9 的 Azure Active Directory 集成
>[AZURE.TIP]反馈，请单击[此处](http://go.microsoft.com/fwlink/?LinkId=528724)。
  
本教程的目的是显示 Azure 和 Panorama9 的集成。  
在本教程中介绍的方案假定您已经有以下各项︰

-   有效的 Azure 订购
-   Panorama9-启用单一登录的订阅
  
后学完本教程，您已分配给 Panorama9 Azure AD 用户将能够到 Panorama9 公司网站 （服务提供程序初始化登录），或使用[简介访问权限面板](https://msdn.microsoft.com/library/dn308586)应用程序的单一登录
  
在本教程中介绍的方案由以下构造块组成︰

1.  启用应用程序集成为 Panorama9
2.  配置单一登录
3.  配置用户设置
4.  分配用户

![方案](./media/active-directory-saas-panorama9-tutorial/IC790016.png "Scenario")
##启用应用程序集成为 Panorama9
  
本部分的目的是概述如何启用应用程序集成为 Panorama9。

###若要启用应用程序集成为 Panorama9，请执行以下步骤︰

1.  在 Azure 管理门户中，在左侧的导航窗格中，单击**活动目录**。

    ![活动目录](./media/active-directory-saas-panorama9-tutorial/IC700993.png "Active Directory")

2.  从**目录**列表中，选择要为其启用目录集成的目录。

3.  若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序](./media/active-directory-saas-panorama9-tutorial/IC700994.png "Applications")

4.  单击页面底部的**添加**。

    ![添加应用程序](./media/active-directory-saas-panorama9-tutorial/IC749321.png "Add application")

5.  在**您想要执行**对话框中，单击**添加应用程序从库**。

    ![从 gallerry 中添加应用程序](./media/active-directory-saas-panorama9-tutorial/IC749322.png "Add an application from gallerry")

6.  在**搜索框**中，键入**Panorama9**。

    ![应用程序库](./media/active-directory-saas-panorama9-tutorial/IC790017.png "Application Gallery")

7.  在结果窗格中，选择**Panorama9**，，然后单击**完成**添加应用程序。

    ![Panorama9](./media/active-directory-saas-panorama9-tutorial/IC790018.png "Panorama9")
##配置单一登录
  
本部分的目的是概述如何使用户能够使用基于 SAML 协议联合 Azure AD 中使用他们的帐户验证到 Panorama9。  
Panorama9 为单一登录配置需要检索证书的指纹值。  
如果您不熟悉此过程，请参阅[如何检索证书的指纹值](http://youtu.be/YKQF266SAxI)。

###若要配置单一登录，请执行以下步骤︰

1.  在 Azure 广告门户中， **Panorama9**应用程序集成在页上，单击**配置单一登录**来打开**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-panorama9-tutorial/IC790019.png "Configure Single Sign-On")

2.  在**您想怎样来登录到 Panorama9 的用户**页上**Microsoft Azure AD 单一登录**，选择，然后单击**下一步**。

    ![配置单一登录](./media/active-directory-saas-panorama9-tutorial/IC790020.png "Configure Single Sign-On")

3.  **配置应用程序 URL**页面上的**Panorama9 号上的 URL**文本框中，键入用户用于登录到 Panorama9 您的 URL (例如:"*https://dashboard.panorama9.com/saml/access/3262*")，然后单击**下一步**。

    ![配置应用程序的 URL](./media/active-directory-saas-panorama9-tutorial/IC790021.png "Configure App URL")

4.  在**配置单一登录 Panorama9 在**页上，下载您的证书，单击**下载证书**，然后在您的计算机上本地保存。

    ![配置单一登录](./media/active-directory-saas-panorama9-tutorial/IC790022.png "Configure Single Sign-On")

5.  在不同的 web 浏览器窗口中，以管理员身份登录到 Panorama9 公司网站。

6.  在顶部的工具栏中，单击**管理**，然后单击**扩展**。

    ![Extensions](./media/active-directory-saas-panorama9-tutorial/IC790023.png "Extensions")

7.  在**扩展名**对话框中，单击**单一登录**。

    ![单一登录](./media/active-directory-saas-panorama9-tutorial/IC790024.png "Single Sign-On")

8.  在**设置**部分中，执行以下步骤︰

    ![Settings](./media/active-directory-saas-panorama9-tutorial/IC790025.png "Settings")

    1.  在 Azure 的门户中，在**配置单一登录在 Panorama9**对话框页面上，**单一登录服务 URL**值，复制，然后将其粘贴到**标识提供程序 URL**文本框。
    2.  从导出的证书，复制的**指纹**值，然后将其粘贴到**证书指纹**文本框。  

        >[AZURE.TIP]有关详细信息，请参阅[如何检索证书的指纹值](http://youtu.be/YKQF266SAxI)

    3.  单击**保存**。

9.  在 Azure 广告门户，选择单一登录配置进行确认，然后单击**完成**关闭**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-panorama9-tutorial/IC790026.png "Configure Single Sign-On")
##配置用户设置
  
为了使 Azure AD 用户能够登录到 Panorama9，他们必须到 Panorama9 配置。  
对于 Panorama9，资源调配是手动任务。

###若要配置用户设置，请执行以下步骤︰

1.  以管理员身份登录到您的**Panorama9**公司网站。

2.  在菜单上，单击**管理**，然后单击**用户**。

    ![用户](./media/active-directory-saas-panorama9-tutorial/IC790027.png "Users")

3.  单击**+**。

4.  在用户数据部分中，执行以下步骤︰

    ![用户](./media/active-directory-saas-panorama9-tutorial/IC790028.png "Users")

    1.  在**电子邮件**文本框中，键入您要提供有效的 Azure Active Directory 用户的电子邮件地址。
    2.  单击**保存**。

>[AZURE.NOTE]您可以使用任何其他 Panorama9 用户帐户创建工具或 Api 来设置 AAD 用户帐户由 Panorama9 提供。

##分配用户
  
若要测试您的配置，您需要将 Azure 的 AD 用户授予您要允许使用通过将他们分配到您应用程序的访问权限。

###要将用户分配到 Panorama9 中，执行以下步骤︰

1.  在 Azure 广告门户中，创建一个测试帐户。

2.  在**Panorama9**应用程序集成页上，单击**将用户分配**。

    ![分配用户](./media/active-directory-saas-panorama9-tutorial/IC790029.png "Assign Users")

3.  选择测试用户，单击**分配**，然后单击**是**以确认您的分配。

    ![是](./media/active-directory-saas-panorama9-tutorial/IC767830.png "Yes")
  
如果您想要测试单个登录设置，打开后盖。 关于访问面板的详细信息，请参阅[访问权限面板简介](https://msdn.microsoft.com/library/dn308586)。
测试
