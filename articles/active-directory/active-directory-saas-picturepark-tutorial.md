---
ms.openlocfilehash: 40d1b35ba35748e322b12bf14ab939099d5f3fae
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties pageTitle="教程︰ Azure Active Directory 集成与 Picturepark |Microsoft Azure" description="了解如何使用 Picturepark Azure Active Directory 以启用单一登录、 自动化资源调配，和更多。" services="active-directory" authors="MarkusVi"  documentationCenter="na" manager="stevenpo"/>
<tags ms.service="active-directory" ms.devlang="na" ms.topic="article" ms.tgt_pltfrm="na" ms.workload="identity" ms.date="08/01/2015" ms.author="markvi" />
#教程︰ 与 Picturepark 的 Azure Active Directory 集成
>[AZURE.TIP]反馈，请单击[此处](http://go.microsoft.com/fwlink/?LinkId=524466)。
  
本教程的目的是显示 Azure 和 Picturepark 的集成。  
在本教程中介绍的方案假定您已经有以下各项︰

-   有效的 Azure 订购
-   Picturepark 租户
  
后学完本教程，您已分配给 Picturepark Azure AD 用户将能够到 Picturepark 公司网站 （服务提供程序初始化登录），或使用[简介访问权限面板](https://msdn.microsoft.com/library/dn308586)应用程序的单一登录
  
在本教程中介绍的方案由以下构造块组成︰

1.  启用应用程序集成为 Picturepark
2.  配置单一登录
3.  配置用户设置
4.  分配用户

![方案](./media/active-directory-saas-picturepark-tutorial/IC795055.png "Scenario")
##启用应用程序集成为 Picturepark
  
本部分的目的是概述如何启用应用程序集成为 Picturepark。

###若要启用应用程序集成为 Picturepark，请执行以下步骤︰

1.  在 Azure 管理门户中，在左侧的导航窗格中，单击**活动目录**。

    ![活动目录](./media/active-directory-saas-picturepark-tutorial/IC700993.png "Active Directory")

2.  从**目录**列表中，选择要为其启用目录集成的目录。

3.  若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序](./media/active-directory-saas-picturepark-tutorial/IC700994.png "Applications")

4.  单击页面底部的**添加**。

    ![添加应用程序](./media/active-directory-saas-picturepark-tutorial/IC749321.png "Add application")

5.  在**您想要执行**对话框中，单击**添加应用程序从库**。

    ![从 gallerry 中添加应用程序](./media/active-directory-saas-picturepark-tutorial/IC749322.png "Add an application from gallerry")

6.  在**搜索框**中，键入**Picturepark**。

    ![应用程序库](./media/active-directory-saas-picturepark-tutorial/IC795056.png "Application Gallery")

7.  在结果窗格中，选择**Picturepark**，，然后单击**完成**添加应用程序。

    ![Picturepark](./media/active-directory-saas-picturepark-tutorial/IC795057.png "Picturepark")
##配置单一登录
  
本部分的目的是概述如何使用户能够使用基于 SAML 协议联合 Azure AD 中使用他们的帐户验证到 Picturepark。  
Picturepark 为单一登录配置需要检索证书的指纹值。  
如果您不熟悉此过程，请参阅[如何检索证书的指纹值](http://youtu.be/YKQF266SAxI)。.

###若要配置单一登录，请执行以下步骤︰

1.  在 Azure 广告门户中， **Picturepark**应用程序集成在页上，单击**配置单一登录**来打开**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-picturepark-tutorial/IC795058.png "Configure Single Sign-On")

2.  在**您想怎样来登录到 Picturepark 的用户**页上**Microsoft Azure AD 单一登录**，选择，然后单击**下一步**。

    ![配置单一登录](./media/active-directory-saas-picturepark-tutorial/IC795059.png "Configure Single Sign-On")

3.  在**配置应用程序 URL**页上，在**Picturepark 号上的 URL**文本框中，键入您的 URL 使用以下模式"*http://company.picturepark.com*"，然后单击**下一步**。

    ![配置应用程序的 URL](./media/active-directory-saas-picturepark-tutorial/IC795060.png "Configure App URL")

4.  在**配置单一登录 Picturepark 在**页上，下载您的证书，单击**下载证书**，然后将保存在您的计算机的本地证书文件。

    ![配置单一登录](./media/active-directory-saas-picturepark-tutorial/IC795061.png "Configure Single Sign-On")

5.  在不同的 web 浏览器窗口中，以管理员身份登录到 Picturepark 公司网站。

6.  在顶部的工具栏中，单击**管理工具**，然后单击**管理控制台**。

    ![管理控制台](./media/active-directory-saas-picturepark-tutorial/IC795062.png "Management Console")

7.  单击**身份验证**，然后单击**身份提供程序**。

    ![身份验证](./media/active-directory-saas-picturepark-tutorial/IC795063.png "Authentication")

8.  在**标识提供程序配置**部分中，执行以下步骤︰

    ![标识提供程序配置](./media/active-directory-saas-picturepark-tutorial/IC795064.png "Identity provider configuration")

    1.  单击**添加**。
    2.  键入您的配置的名称。
    3.  选择**设为默认值**。
    4.  在 Azure 的门户中，在**配置单一登录在 Picturepark**对话框页面上， **SAML SSO URL**值，复制，然后将其粘贴到文本框中**颁发的 URI** 。
    5.  从导出的证书，复制的**指纹**值，然后将其粘贴到**受信任的颁发者拇指打印**文本框。  

        >[AZURE.TIP]有关详细信息，请参阅[如何检索证书的指纹值](http://youtu.be/YKQF266SAxI)

    6.  单击**JoinDefaultUsersGroup**。
    7.  若要设置**电子邮件地址**属性的**声明**文本框中，键入**http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**。
        ![配置](./media/active-directory-saas-picturepark-tutorial/IC795065.png "Configuration")
    8.  单击**保存**。

9.  在 Azure 广告门户，选择单一登录配置进行确认，然后单击**完成**关闭**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-picturepark-tutorial/IC795066.png "Configure Single Sign-On")

##配置用户设置
  
为了使 Azure AD 用户能够登录到 Picturepark，他们必须到 Picturepark 配置。  
对于 Picturepark，资源调配是手动任务。

###若要设置用户帐户，请执行以下步骤︰

1.  登录到**Picturepark**租户。

2.  在顶部的工具栏中，单击**管理工具**，然后单击**用户**。

    ![用户](./media/active-directory-saas-picturepark-tutorial/IC795067.png "Users")

3.  在**用户概述**选项卡上，单击**新建**。

    ![用户管理](./media/active-directory-saas-picturepark-tutorial/IC795068.png "User management")

4.  在**创建用户**对话框中，请执行以下步骤︰

    ![创建用户](./media/active-directory-saas-picturepark-tutorial/IC795069.png "Create User")

    1.  类型︰**电子邮件地址**、**密码**、**确认密码**、**名字**、**姓氏**、**公司**、**国家**、**邮政编码**、**城市**的有效 Azure 活动目录用户所需 ot 调配到相关的文本框。
    2.  选择一种**语言**。
    3.  单击**创建**。

>[AZURE.NOTE]您可以使用任何其他 Picturepark 用户帐户创建工具或 Api 来设置 AAD 用户帐户由 Picturepark 提供。

##分配用户
  
若要测试您的配置，您需要将 Azure 的 AD 用户授予您要允许使用通过将他们分配到您应用程序的访问权限。

###要将用户分配到 Picturepark 中，执行以下步骤︰

1.  在 Azure 广告门户中，创建一个测试帐户。

2.  在**Picturepark**应用程序集成页上，单击**将用户分配**。

    ![分配用户](./media/active-directory-saas-picturepark-tutorial/IC795070.png "Assign Users")

3.  选择测试用户，单击**分配**，然后单击**是**以确认您的分配。

    ![是](./media/active-directory-saas-picturepark-tutorial/IC767830.png "Yes")
  
如果您想要测试单个登录设置，打开后盖。 关于访问面板的详细信息，请参阅[访问权限面板简介](https://msdn.microsoft.com/library/dn308586)。
测试
