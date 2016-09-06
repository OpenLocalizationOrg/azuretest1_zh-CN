---
ms.openlocfilehash: 52e6f682080c97718edcc9b3e3e10b96aad62f16
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties pageTitle="教程︰ Azure Active Directory 集成与 Coupa |Microsoft Azure" description="了解如何使用 Coupa Azure Active Directory 以启用单一登录、 自动化资源调配，和更多。" services="active-directory" authors="MarkusVi"  documentationCenter="na" manager="stevenpo"/>
<tags ms.service="active-directory" ms.devlang="na" ms.topic="article" ms.tgt_pltfrm="na" ms.workload="identity" ms.date="08/01/2015" ms.author="markvi" />
#教程︰ 与 Coupa 的 Azure Active Directory 集成
>[AZURE.TIP]反馈，请单击[此处](http://go.microsoft.com/fwlink/?LinkId=529806)。

本教程的目的是显示 Azure 和 Coupa 的集成。  
在本教程中介绍的方案假定您已经有以下各项︰

-   有效的 Azure 订购
-   Coupa-启用单一登录的订阅

后学完本教程，您已分配给 Coupa Azure AD 用户将能够到单一登录到应用程序中使用[接入面板简介](https://msdn.microsoft.com/library/dn308586)

在本教程中介绍的方案由以下构造块组成︰

1.  启用应用程序集成为 Coupa
2.  配置单一登录
3.  配置用户设置
4.  分配用户

![方案](./media/active-directory-saas-coupa-tutorial/IC791897.png "Scenario")
##启用应用程序集成为 Coupa

本部分的目的是概述如何启用应用程序集成为 Coupa。

###若要启用应用程序集成为 Coupa，请执行以下步骤︰

1.  在 Azure 管理门户中，在左侧的导航窗格中，单击**活动目录**。

    ![活动目录](./media/active-directory-saas-coupa-tutorial/IC700993.png "Active Directory")

2.  从**目录**列表中，选择要为其启用目录集成的目录。

3.  若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序](./media/active-directory-saas-coupa-tutorial/IC700994.png "Applications")

4.  单击页面底部的**添加**。

    ![添加应用程序](./media/active-directory-saas-coupa-tutorial/IC749321.png "Add application")

5.  在**您想要执行**对话框中，单击**添加应用程序从库**。

    ![从 gallerry 中添加应用程序](./media/active-directory-saas-coupa-tutorial/IC749322.png "Add an application from gallerry")

6.  在**搜索框**中，键入**Coupa**。

    ![应用程序库](./media/active-directory-saas-coupa-tutorial/IC791898.png "Application Gallery")

7.  在结果窗格中，选择**Coupa**，，然后单击**完成**添加应用程序。

    ![Coupa](./media/active-directory-saas-coupa-tutorial/IC791899.png "Coupa")
##配置单一登录

本部分的目的是概述如何使用户能够使用基于 SAML 协议联合 Azure AD 中使用他们的帐户验证到 Coupa。  
Coupa 为单一登录配置需要检索证书的指纹值。  
如果您不熟悉此过程，请参阅[如何检索证书的指纹值](http://youtu.be/YKQF266SAxI)。

###若要配置单一登录，请执行以下步骤︰

1.  以管理员身份登录到您的 Coupa 公司网站。

2.  转到**安装程序\>安全控件**。

    ![安全控制](./media/active-directory-saas-coupa-tutorial/IC791900.png "Security Controls")

3.  要将 Coupa 元数据文件下载到您的计算机，请单击**下载并导入 SP 元数据**。

    ![Coupa SP 元数据](./media/active-directory-saas-coupa-tutorial/IC791901.png "Coupa SP metadata")

4.  在不同的浏览器窗口中，登录到 Azure Active Directory 门户。

5.  在**Coupa**应用程序集成页上，单击**配置单一登录**来打开**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-coupa-tutorial/IC791902.png "Configure Single Sign-On")

6.  在**您想怎样来登录到 Coupa 的用户**页上**Microsoft Azure AD 单一登录**，选择，然后单击**下一步**。

    ![配置单一登录](./media/active-directory-saas-coupa-tutorial/IC791903.png "Configure Single Sign-On")

7.  在**配置应用程序 URL**页上，请执行以下步骤︰

    ![配置应用程序的 URL](./media/active-directory-saas-coupa-tutorial/IC791904.png "Configure App URL")

    1.  在**符号上 URL**文本框中，键入用户用于登录到您的 Coupa 应用程序的 URL (例如:"*http://company.Coupa.com*")。
    2.  打开您下载的 Coupa 元数据文件，然后再复制**AssertionConsumerService 的索引 URL**。
    3.  在**Coupa 回复 URL**文本框中，粘贴的**AssertionConsumerService 的索引 URL**值。
    4.  单击**下一步**。

8.  在**配置单一登录 Coupa 在**页面上，要下载元数据文件，请单击**下载元数据**，然后保存在您的计算机上的本地文件。

    ![配置单一登录](./media/active-directory-saas-coupa-tutorial/IC791905.png "Configure Single Sign-On")

9.  在 Coupa 公司网站上，转到**安装程序\>安全控件**。

    ![安全控制](./media/active-directory-saas-coupa-tutorial/IC791900.png "Security Controls")

10. 在**使用 Coupa 凭据登录**部分中，执行以下步骤︰

    ![使用 Coupa 凭据登录](./media/active-directory-saas-coupa-tutorial/IC791906.png "Log in using Coupa credentials")

    1.  选择**使用 SAML 登录**。
    2.  单击**浏览**上载下载的 Azure 活动的元数据文件。
    3.  单击**保存**。

11. 在 Azure 广告门户，选择单一登录配置进行确认，然后单击**完成**关闭**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-coupa-tutorial/IC791907.png "Configure Single Sign-On")
##配置用户设置

为了使 Azure AD 用户能够登录到 Coupa，他们必须到 Coupa 配置。  
对于 Coupa，资源调配是手动任务。

###若要配置用户设置，请执行以下步骤︰

1.  以管理员身份登录到您的**Coupa**公司网站。

2.  在菜单上，单击**安装**，然后单击**用户**。

    ![用户](./media/active-directory-saas-coupa-tutorial/IC791908.png "Users")

3.  单击**创建**。

    ![创建用户](./media/active-directory-saas-coupa-tutorial/IC791909.png "Create Users")

4.  在**创建用户**部分中，执行以下步骤︰

    ![用户详细信息](./media/active-directory-saas-coupa-tutorial/IC791910.png "User Details")

    1.  键入**登录**、**名字**、**姓氏**、**单一的登录 ID**，要到相关文本框设置有效的 Azure Active Directory 帐户的**电子邮件**属性。
    2.  单击**创建**。

    >[AZURE.NOTE] Azure Active Directory 帐户持有者将得到一封电子邮件，以确认该帐户，然后将变为活动状态的链接。

>[AZURE.NOTE] 您可以使用任何其他 Coupa 用户帐户创建工具或 Api 来设置 AAD 用户帐户由 Coupa 提供。

##分配用户

若要测试您的配置，您需要将 Azure 的 AD 用户授予您要允许使用通过将他们分配到您应用程序的访问权限。

###要将用户分配到 Coupa 中，执行以下步骤︰

1.  在 Azure 广告门户中，创建一个测试帐户。

2.  在**Coupa**应用程序集成页上，单击**将用户分配**。

    ![分配用户](./media/active-directory-saas-coupa-tutorial/IC791911.png "Assign Users")

3.  选择测试用户，单击**分配**，然后单击**是**以确认您的分配。

    ![是](./media/active-directory-saas-coupa-tutorial/IC767830.png "Yes")

如果您想要测试单个登录设置，打开后盖。 关于访问面板的详细信息，请参阅[访问权限面板简介](https://msdn.microsoft.com/library/dn308586)。

测试
