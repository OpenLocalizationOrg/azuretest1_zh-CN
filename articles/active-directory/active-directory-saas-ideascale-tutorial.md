---
ms.openlocfilehash: 85301f9e193fa3a56101f879a82a0d98b5b5eeb9
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties pageTitle="教程︰ Azure Active Directory 集成与 IdeaScale |Microsoft Azure" description="了解如何使用 IdeaScale Azure Active Directory 以启用单一登录、 自动化资源调配，和更多。" services="active-directory" authors="MarkusVi"  documentationCenter="na" manager="stevenpo"/>
<tags ms.service="active-directory" ms.devlang="na" ms.topic="article" ms.tgt_pltfrm="na" ms.workload="identity" ms.date="08/01/2015" ms.author="markvi" />
#教程︰ 与 IdeaScale 的 Azure Active Directory 集成
>[AZURE.TIP]反馈，请单击[此处](http://go.microsoft.com/fwlink/?LinkId=529830)。
  
本教程的目的是显示 Azure 和 IdeaScale 的集成。  
在本教程中介绍的方案假定您已经有以下各项︰

-   有效的 Azure 订购
-   IdeaScale-启用单一登录的订阅
  
后学完本教程，您已分配给 IdeaScale Azure AD 用户将能够到单一登录到应用程序中使用[简介访问权限面板](https://msdn.microsoft.com/library/dn308586)。
  
在本教程中介绍的方案由以下构造块组成︰

1.  启用应用程序集成为 IdeaScale
2.  配置单一登录
3.  配置用户设置
4.  分配用户

![方案](./media/active-directory-saas-ideascale-tutorial/IC790838.png "Scenario")
##启用应用程序集成为 IdeaScale
  
本部分的目的是概述如何启用应用程序集成为 IdeaScale。

###若要启用应用程序集成为 IdeaScale，请执行以下步骤︰

1.  在 Azure 管理门户中，在左侧的导航窗格中，单击**活动目录**。

    ![活动目录](./media/active-directory-saas-ideascale-tutorial/IC700993.png "Active Directory")

2.  从**目录**列表中，选择要为其启用目录集成的目录。

3.  若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序](./media/active-directory-saas-ideascale-tutorial/IC700994.png "Applications")

4.  单击页面底部的**添加**。

    ![添加应用程序](./media/active-directory-saas-ideascale-tutorial/IC749321.png "Add application")

5.  在**您想要执行**对话框中，单击**添加应用程序从库**。

    ![从 gallerry 中添加应用程序](./media/active-directory-saas-ideascale-tutorial/IC749322.png "Add an application from gallerry")

6.  在**搜索框**中，键入**IdeaScale**。

    ![应用程序库](./media/active-directory-saas-ideascale-tutorial/IC790841.png "Application Gallery")

7.  在结果窗格中，选择**IdeaScale**，，然后单击**完成**添加应用程序。

    ![IdeaScale](./media/active-directory-saas-ideascale-tutorial/IC790842.png "IdeaScale")
##配置单一登录
  
本部分的目的是概述如何使用户能够使用基于 SAML 协议联合 Azure AD 中使用他们的帐户验证到 IdeaScale。  
IdeaScale 为单一登录配置需要检索证书的指纹值。  
如果您不熟悉此过程，请参阅[如何检索证书的指纹值](http://youtu.be/YKQF266SAxI)。

###若要配置单一登录，请执行以下步骤︰

1.  在 Azure 广告门户中， **IdeaScale**应用程序集成在页上，单击**配置单一登录**来打开**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-ideascale-tutorial/IC790843.png "Configure Single Sign-On")

2.  在**您想怎样来登录到 IdeaScale 的用户**页上**Microsoft Azure AD 单一登录**，选择，然后单击**下一步**。

    ![配置单一登录](./media/active-directory-saas-ideascale-tutorial/IC790844.png "Configure Single Sign-On")

3.  在**配置应用程序 URL**页面上的**IdeaScale 号上的 URL**文本框中，键入用户用于登录到您的 IdeaScale 应用程序的 URL (例如:"*https://company.IdeaScale.com*")，然后单击**下一步**。

    ![配置应用程序的 URL](./media/active-directory-saas-ideascale-tutorial/IC790845.png "Configure App URL")

4.  在**配置单一登录 IdeaScale 在**页上，要下载元数据，请单击**下载元数据**，然后保存在您的计算机上的本地元数据文件。

    ![配置单一登录](./media/active-directory-saas-ideascale-tutorial/IC790846.png "Configure Single Sign-On")

5.  在不同的 web 浏览器窗口中，以管理员身份登录到 IdeaScale 公司网站。

6.  转到**社区设置**。

    ![社区设置](./media/active-directory-saas-ideascale-tutorial/IC790847.png "Community Settings")

7.  转到**安全\>单一登录设置**。

    ![单一登录设置](./media/active-directory-saas-ideascale-tutorial/IC790848.png "Single Signon Settings")

8.  作为**单一登录类型**，选择**SAML 2.0**。

    ![单一登录类型](./media/active-directory-saas-ideascale-tutorial/IC790849.png "Single Signon Type")

9.  在**单一登录设置**对话框中，请执行以下步骤︰

    ![单一登录设置](./media/active-directory-saas-ideascale-tutorial/IC790850.png "Single Signon Settings")

    1.  在 Azure 的门户中，在**配置单一登录在 IdeaScale**对话框页面上，**实体 ID**值，复制，然后将其粘贴到**SAML IdP 实体 ID**文本框。
    2.  您已下载元数据的文件，将内容复制，然后将其粘贴到文本框中**SAML IdP 元数据**。
    3.  在 Azure 的门户中，在**配置单一登录在 IdeaScale**对话框页复制**远程注销 URL**值，，然后将其粘贴到**注销成功 URL**文本框。
    4.  单击**保存更改**。

10. 在 Azure 广告门户，选择单一登录配置进行确认，然后单击**完成**关闭**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-ideascale-tutorial/IC790851.png "Configure Single Sign-On")
##配置用户设置
  
为了使 Azure AD 用户能够登录到 IdeaScale，他们必须被调配到 IdeaScale。  
对于 IdeaScale，资源调配是手动任务。

###若要配置用户设置，请执行以下步骤︰

1.  以管理员身份登录到您的**IdeaScale**公司网站。

2.  转到**社区设置**。

    ![社区设置](./media/active-directory-saas-ideascale-tutorial/IC790847.png "Community Settings")

3.  转到**的基本设置\>成员管理**。

4.  单击**添加成员**。

    ![成员管理](./media/active-directory-saas-ideascale-tutorial/IC790852.png "Member Management")

5.  在添加新成员部分中，执行以下步骤︰

    ![添加新成员](./media/active-directory-saas-ideascale-tutorial/IC790853.png "Add New Member")

    1.  在**电子邮件地址**文本框中，键入您要提供一个有效的 AAD 帐户的电子邮件地址。
    2.  单击**保存更改**。

    >[AZURE.NOTE] Azure Active Directory 帐户持有者将得到一封电子邮件，以确认该帐户，然后将变为活动状态的链接。

>[AZURE.NOTE] 您可以使用任何其他 IdeaScale 用户帐户创建工具或 Api 来设置 AAD 用户帐户由 IdeaScale 提供。

##分配用户
  
若要测试您的配置，您需要将 Azure 的 AD 用户授予您要允许使用通过将他们分配到您应用程序的访问权限。

###要将用户分配到 IdeaScale 中，执行以下步骤︰

1.  在 Azure 广告门户中，创建一个测试帐户。

2.  在**IdeaScale**应用程序集成页上，单击**将用户分配**。

    ![分配用户](./media/active-directory-saas-ideascale-tutorial/IC790854.png "Assign Users")

3.  选择测试用户，单击**分配**，然后单击**是**以确认您的分配。

    ![是](./media/active-directory-saas-ideascale-tutorial/IC767830.png "Yes")
  
如果您想要测试单个登录设置，打开后盖。 关于访问面板的详细信息，请参阅[访问权限面板简介](https://msdn.microsoft.com/library/dn308586)。
测试
