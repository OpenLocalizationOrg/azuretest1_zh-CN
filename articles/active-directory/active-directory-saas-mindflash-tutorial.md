---
ms.openlocfilehash: 25972f988be9d2e59e4dccd757fc44987415abbe
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties pageTitle="教程︰ Azure Active Directory 集成与 Mindflash |Microsoft Azure" description="了解如何使用 Mindflash Azure Active Directory 以启用单一登录、 自动化资源调配，和更多。" services="active-directory" authors="MarkusVi"  documentationCenter="na" manager="stevenpo"/>
<tags ms.service="active-directory" ms.devlang="na" ms.topic="article" ms.tgt_pltfrm="na" ms.workload="identity" ms.date="08/01/2015" ms.author="markvi" />
#教程︰ 与 Mindflash 的 Azure Active Directory 集成
>[AZURE.TIP]反馈，请单击[此处](http://go.microsoft.com/fwlink/?LinkId=528416)。
  
本教程的目的是显示 Azure 和 Mindflash 的集成。  
在本教程中介绍的方案假定您已经有以下各项︰

-   有效的 Azure 订购
-   Mindflash-启用单一登录的订阅
  
后学完本教程，您已分配给 Mindflash Azure AD 用户将能够到 Mindflash 公司网站 （服务提供程序初始化登录），或使用[简介访问权限面板](https://msdn.microsoft.com/library/dn308586)应用程序的单一登录
  
在本教程中介绍的方案由以下构造块组成︰

1.  启用应用程序集成为 Mindflash
2.  配置单一登录
3.  配置用户设置
4.  分配用户

![方案](./media/active-directory-saas-mindflash-tutorial/IC787132.png "Scenario")
##启用应用程序集成为 Mindflash
  
本部分的目的是概述如何启用应用程序集成为 Mindflash。

###若要启用应用程序集成为 Mindflash，请执行以下步骤︰

1.  在 Azure 管理门户中，在左侧的导航窗格中，单击**活动目录**。

    ![活动目录](./media/active-directory-saas-mindflash-tutorial/IC700993.png "Active Directory")

2.  从**目录**列表中，选择要为其启用目录集成的目录。

3.  若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序](./media/active-directory-saas-mindflash-tutorial/IC700994.png "Applications")

4.  单击页面底部的**添加**。

    ![添加应用程序](./media/active-directory-saas-mindflash-tutorial/IC749321.png "Add application")

5.  在**您想要执行**对话框中，单击**添加应用程序从库**。

    ![从 gallerry 中添加应用程序](./media/active-directory-saas-mindflash-tutorial/IC749322.png "Add an application from gallerry")

6.  在**搜索框**中，键入**Mindflash**。

    ![应用程序库](./media/active-directory-saas-mindflash-tutorial/IC787133.png "Application Gallery")

7.  在结果窗格中，选择**Mindflash**，，然后单击**完成**添加应用程序。

    ![Mindflash](./media/active-directory-saas-mindflash-tutorial/IC787134.png "Mindflash")
##配置单一登录
  
本部分的目的是概述如何使用户能够使用基于 SAML 协议联合 Azure AD 中使用他们的帐户验证到 Mindflash。

###若要配置单一登录，请执行以下步骤︰

1.  在 Azure 广告门户中， **Mindflash**应用程序集成在页上，单击**配置单一登录**来打开**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-mindflash-tutorial/IC787135.png "Configure Single Sign-On")

2.  在**您想怎样来登录到 Mindflash 的用户**页上**Microsoft Azure AD 单一登录**，选择，然后单击**下一步**。

    ![配置单一登录](./media/active-directory-saas-mindflash-tutorial/IC787136.png "Configure Single Sign-On")

3.  在**配置应用程序 URL**页上，在**符号上 URL**文本框中，键入您的 URL 使用以下模式"*http://company.mindflash.com*"，然后单击**下一步**。

    ![配置应用程序的 URL](./media/active-directory-saas-mindflash-tutorial/IC787137.png "Configure App URL")

4.  在**配置单一登录 Mindflash 在**页上，单击**下载元数据**，然后将保存在您的计算机上的元数据文件。

    ![配置单一登录](./media/active-directory-saas-mindflash-tutorial/IC787138.png "Configure Single Sign-On")

5.  将元数据文件发送给 Mindflash 的支持团队。

    >[AZURE.NOTE] 单一登录配置已通过 Mindflash 的支持团队来执行。 一旦配置完成后，您将得到通知。

6.  在 Azure 广告门户，选择单一登录配置进行确认，然后单击**完成**关闭**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-mindflash-tutorial/IC787139.png "Configure Single Sign-On")
##配置用户设置
  
为了使 Azure AD 用户能够登录到 Mindflash，他们必须到 Mindflash 配置。  
对于 Mindflash，资源调配是手动任务。

###若要设置用户帐户，请执行以下步骤︰

1.  以管理员身份登录到您的**Mindflash**公司网站。

2.  转到**管理用户**。

    ![管理用户](./media/active-directory-saas-mindflash-tutorial/IC787140.png "Manage Users")

3.  单击**添加用户**，然后单击**新建**。

4.  在**添加新用户**部分中，执行以下步骤︰

    ![添加新用户](./media/active-directory-saas-mindflash-tutorial/IC787141.png "Add New Users")

    1.  键入**名字**、**姓氏**和**电子邮件**有效 AAD 帐户，您希望提供到相关的文本框。
    2.  单击**添加**。

>[AZURE.NOTE]您可以使用任何其他 Mindflash 用户帐户创建工具或 Api 来设置 AAD 用户帐户由 Mindflash 提供。

##分配用户
  
若要测试您的配置，您需要将 Azure 的 AD 用户授予您要允许使用通过将他们分配到您应用程序的访问权限。

###要将用户分配到 Mindflash 中，执行以下步骤︰

1.  在 Azure 广告门户中，创建一个测试帐户。

2.  在**Mindflash**应用程序集成页上，单击**将用户分配**。

    ![分配用户](./media/active-directory-saas-mindflash-tutorial/IC787142.png "Assign users")

3.  选择测试用户，单击**分配**，然后单击**是**以确认您的分配。

    ![是](./media/active-directory-saas-mindflash-tutorial/IC767830.png "Yes")
  
如果您想要测试单个登录设置，打开后盖。 关于访问面板的详细信息，请参阅[访问权限面板简介](https://msdn.microsoft.com/library/dn308586)。
测试
