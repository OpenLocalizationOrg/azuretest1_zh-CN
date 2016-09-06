---
ms.openlocfilehash: d2ee264956b5958030ce22e1225f9a1132c87ce7
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties pageTitle="教程︰ Azure Active Directory 集成与 RightAnswers |Microsoft Azure" description="了解如何使用 RightAnswers Azure Active Directory 以启用单一登录、 自动化资源调配，和更多。" services="active-directory" authors="MarkusVi"  documentationCenter="na" manager="stevenpo"/>
<tags ms.service="active-directory" ms.devlang="na" ms.topic="article" ms.tgt_pltfrm="na" ms.workload="identity" ms.date="08/01/2015" ms.author="markvi" />
#教程︰ 与 RightAnswers 的 Azure Active Directory 集成
>[AZURE.TIP]反馈，请单击[此处](http://go.microsoft.com/fwlink/?LinkId=615354)。
  
本教程的目的是显示 Azure 和 RightAnswers 的集成。 在本教程中介绍的方案假定您已经有以下各项︰

-   有效的 Azure 订购
-   RightAnswers-启用单一登录的订阅
  
后学完本教程，您已分配给 RightAnswers Azure AD 用户将能够到单一登录到应用程序中使用[接入面板简介](https://msdn.microsoft.com/library/dn308586)
  
在本教程中介绍的方案由以下构造块组成︰

1.  启用应用程序集成为 RightAnswers
2.  配置单一登录
3.  配置用户设置
4.  分配用户

![方案](./media/active-directory-saas-rightanswers-tutorial/IC802925.png "Scenario")
##启用应用程序集成为 RightAnswers
  
本部分的目的是概述如何启用应用程序集成为 RightAnswers。

###若要启用应用程序集成为 RightAnswers，请执行以下步骤︰

1.  在 Azure 管理门户中，在左侧的导航窗格中，单击**活动目录**。

    ![活动目录](./media/active-directory-saas-rightanswers-tutorial/IC700993.png "Active Directory")

2.  从**目录**列表中，选择要为其启用目录集成的目录。

3.  若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序](./media/active-directory-saas-rightanswers-tutorial/IC700994.png "Applications")

4.  单击页面底部的**添加**。

    ![添加应用程序](./media/active-directory-saas-rightanswers-tutorial/IC749321.png "Add application")

5.  在**您想要执行**对话框中，单击**添加应用程序从库**。

    ![从 gallerry 中添加应用程序](./media/active-directory-saas-rightanswers-tutorial/IC749322.png "Add an application from gallerry")

6.  在**搜索框**中，键入**RightAnswers**。

    ![应用程序库](./media/active-directory-saas-rightanswers-tutorial/IC802926.png "Application Gallery")

7.  在结果窗格中，选择**RightAnswers**，，然后单击**完成**添加应用程序。
##配置单一登录
  
本部分的目的是概述如何使用户能够使用基于 SAML 协议联合 Azure AD 中使用他们的帐户验证到 RightAnswers。

###若要配置单一登录，请执行以下步骤︰

1.  在 Azure 广告门户中， **RightAnswers**应用程序集成在页上，单击**配置单一登录**来打开**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-rightanswers-tutorial/IC802927.png "Configure Single Sign-On")

2.  在**您想怎样来登录到 RightAnswers 的用户**页上**Microsoft Azure AD 单一登录**，选择，然后单击**下一步**。

    ![配置单一登录](./media/active-directory-saas-rightanswers-tutorial/IC802928.png "Configure Single Sign-On")

3.  **配置应用程序设置**页上的**符号在 URL**文本框中，键入您为登录到 RightAnswers 应用程序的用户使用的 URL (例如︰ *https://fortify.rightanswers.com/portal/ss/*)，然后单击**下一步**。

    ![配置应用程序设置](./media/active-directory-saas-rightanswers-tutorial/IC802929.png "Configure App Settings")

4.  在**配置单一登录 RightAnswers 在**页上，要下载元数据，请单击**下载元数据**，然后保存在您的计算机上的本地元数据文件。

    ![配置单一登录](./media/active-directory-saas-rightanswers-tutorial/IC802930.png "Configure Single Sign-On")

5.  将下载元数据文件发送到 RightAnswers 的支持团队。

    >[AZURE.NOTE] RightAnswers 的支持团队就不得不执行实际的 SSO 配置。
当您的订阅已启用 SSO，您将得到通知。

6.  在 Azure 广告门户，选择单一登录配置进行确认，然后单击**完成**关闭**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-rightanswers-tutorial/IC802931.png "Configure Single Sign-On")
##配置用户设置
  
为了使 Azure AD 用户能够登录到 RightAnswers，他们必须到 RightAnswers 配置。  
对于 RightAnswers，资源调配是自动执行的任务。  
没有为您的操作项。
  
用户会自动创建在必要时在第一次登录尝试。

>[AZURE.NOTE]您可以使用任何其他 RightAnswers 用户帐户创建工具或 Api 来设置 AAD 用户帐户由 RightAnswers 提供。

##分配用户
  
若要测试您的配置，您需要将 Azure 的 AD 用户授予您要允许使用通过将他们分配到您应用程序的访问权限。

###要将用户分配到 RightAnswers 中，执行以下步骤︰

1.  在 Azure 广告门户中，创建一个测试帐户。

2.  在**RightAnswers**应用程序集成页上，单击**将用户分配**。

    ![分配用户](./media/active-directory-saas-rightanswers-tutorial/IC802932.png "Assign Users")

3.  选择测试用户，单击**分配**，然后单击**是**以确认您的分配。

    ![是](./media/active-directory-saas-rightanswers-tutorial/IC767830.png "Yes")
  
如果您想要测试单个登录设置，打开后盖。 关于访问面板的详细信息，请参阅[访问权限面板简介](https://msdn.microsoft.com/library/dn308586)。
测试
