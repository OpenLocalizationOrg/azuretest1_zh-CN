---
ms.openlocfilehash: e4ddd2f27841e98c80eef7e04aa72cc8dfafda03
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties pageTitle="教程︰ Azure Active Directory 集成与 15Five |Microsoft Azure" description="了解如何使用 15Five Azure Active Directory 以启用单一登录、 自动化资源调配，和更多。" services="active-directory" authors="MarkusVi"  documentationCenter="na" manager="stevenpo"/>
<tags ms.service="active-directory" ms.devlang="na" ms.topic="article" ms.tgt_pltfrm="na" ms.workload="identity" ms.date="08/01/2015" ms.author="markvi" />
#教程︰ 与 15Five 的 Azure Active Directory 集成
>[AZURE.TIP]反馈，请单击[此处](http://go.microsoft.com/fwlink/?LinkId=528017)。

本教程的目的是显示 Azure 和 15Five 的集成。 在本教程中介绍的方案假定您已经有以下各项︰

-   有效的 Azure 订购
-   15Five-启用单一登录的订阅

后学完本教程，您已分配给 15Five Azure AD 用户将能够到 15Five 公司网站 （服务提供程序初始化登录），或使用[简介访问权限面板](https://msdn.microsoft.com/library/dn308586)应用程序的单一登录

在本教程中介绍的方案由以下构造块组成︰

1.  启用应用程序集成为 15Five
2.  配置单一登录
3.  配置用户设置
4.  分配用户

![方案](./media/active-directory-saas-15five-tutorial/IC784667.png "Scenario")
##启用应用程序集成为 15Five

本部分的目的是概述如何启用应用程序集成为 15Five。

###若要启用应用程序集成为 15Five，请执行以下步骤︰

1.  在 Azure 管理门户中，在左侧的导航窗格中，单击**活动目录**。

    ![活动目录](./media/active-directory-saas-15five-tutorial/IC700993.png "Active Directory")

2.  从**目录**列表中，选择要为其启用目录集成的目录。

3.  若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序](./media/active-directory-saas-15five-tutorial/IC700994.png "Applications")

4.  单击页面底部的**添加**。

    ![添加应用程序](./media/active-directory-saas-15five-tutorial/IC749321.png "Add application")

5.  在**您想要执行**对话框中，单击**添加应用程序从库**。

    ![从 gallerry 中添加应用程序](./media/active-directory-saas-15five-tutorial/IC749322.png "Add an application from gallerry")

6.  在**搜索框**中，键入**15Five**。

    ![应用程序库](./media/active-directory-saas-15five-tutorial/IC784668.png "Application Gallery")

7.  在结果窗格中，选择**15Five**，，然后单击**完成**添加应用程序。

    ![15Five](./media/active-directory-saas-15five-tutorial/IC784669.png "15Five")
##配置单一登录

本部分的目的是概述如何使用户能够通过身份验证的使用他们的帐户 15Five Azure AD 使用联合身份验证基于 SAML 协议中。

###若要配置单一登录，请执行以下步骤︰

1.  在 Azure 广告门户中， **15Five**应用程序集成在页上，单击**配置单一登录**来打开**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-15five-tutorial/IC784670.png "Configure single sign-on")

2.  在**您想怎样来登录到 15Five 的用户**页上**Microsoft Azure AD 单一登录**，选择，然后单击**下一步**。

    ![配置单一登录](./media/active-directory-saas-15five-tutorial/IC784671.png "Configure single sign-on")

3.  在**配置应用程序 URL**页上，在**15Five 号中 URL**文本框中，键入您的 URL 使用以下模式"*https://company.15Five.com*"，然后单击**下一步**。

    ![配置应用程序的 URL](./media/active-directory-saas-15five-tutorial/IC784672.png "Configure App URL")

4.  在**配置单一登录 15Five 在**页上，单击**下载元数据**，然后将转发到 15Five 的支持团队的元数据文件。

    ![配置单一登录](./media/active-directory-saas-15five-tutorial/IC784673.png "Configure single sign-on")

    >[AZURE.NOTE] 单一登录需要启用 15Five 的支持团队。

5.  在 Azure 广告门户，选择单一登录配置进行确认，然后单击**完成**关闭**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-15five-tutorial/IC784674.png "Configure single sign-on")
##配置用户设置

为了使 Azure AD 用户能够登录到 15Five，他们必须到 15Five 配置。  
对于 15Five，资源调配是手动任务。

###若要配置用户设置，请执行以下步骤︰

1.  以管理员身份登录到您的**15Five**公司网站。

2.  转到**管理公司**。

    ![管理公司](./media/active-directory-saas-15five-tutorial/IC784675.png "Manage Company")

3.  转到**人\>添加人**。

    ![人脉](./media/active-directory-saas-15five-tutorial/IC784676.png "People")

4.  在添加新用户部分中，执行以下步骤︰

    ![添加新的联系人](./media/active-directory-saas-15five-tutorial/IC784677.png "Add New Person")

    1.  键入的**名字**、**姓氏**、**标题**、 要到相关文本框设置有效的 Azure Active Directory 帐户的**电子邮件地址**。
    2.  单击**完成**。

    >[AZURE.NOTE] Azure 的广告帐户持有者会收到包括确认该帐户之前将变为活动状态的链接的电子邮件。

>[AZURE.NOTE] 您可以使用任何其他 15Five 用户帐户创建工具或 Api 提供的 15Five 来设置 AAD 用户帐户。

##分配用户

若要测试您的配置，您需要将 Azure 的 AD 用户授予您要允许使用通过将他们分配到您应用程序的访问权限。

###要将用户分配到 15Five 中，执行以下步骤︰

1.  在 Azure 广告门户中，创建一个测试帐户。

2.  在**15Five**应用程序集成页上，单击**将用户分配**。

    ![分配用户](./media/active-directory-saas-15five-tutorial/IC784678.png "Assign users")

3.  选择测试用户，单击**分配**，然后单击**是**以确认您的分配。

    ![是](./media/active-directory-saas-15five-tutorial/IC767830.png "Yes")

如果您想要测试单个登录设置，打开后盖。 关于访问面板的详细信息，请参阅[访问权限面板简介](https://msdn.microsoft.com/library/dn308586)。

测试
