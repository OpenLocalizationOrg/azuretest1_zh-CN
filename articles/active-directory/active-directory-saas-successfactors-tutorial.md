---
ms.openlocfilehash: 35073a00701835e2f2bca227ab125d55b72b64a1
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties pageTitle="教程︰ Azure 与 active Directory 集成与 SuccessFactors |Microsoft Azure" description="了解如何使用 SuccessFactors Azure Active Directory 以启用单一登录、 自动化资源调配，和更多。" services="active-directory" authors="MarkusVi"  documentationCenter="na" manager="stevenpo"/>
<tags ms.service="active-directory" ms.devlang="na" ms.topic="article" ms.tgt_pltfrm="na" ms.workload="identity" ms.date="08/01/2015" ms.author="markvi" />
#教程︰ 与 SuccessFactors 的 Azure 与 active Directory 集成
>[AZURE.TIP]反馈，请单击[此处](http://go.microsoft.com/fwlink/?LinkId=529792)。
  
本教程的目的是显示**SP 启动单一登录模式**在 Azure 和 SuccessFactors 的集成。  
在本教程中介绍的方案假定您已经有以下各项︰

-   有效的 Azure 订购
-   SuccessFactors 一个登录启用的订阅中 SP 启动模式
  
后学完本教程，您已分配给 SuccessFactors Azure AD 用户将能够到 SuccessFactors 公司网站 （服务提供程序初始化登录），或使用[简介访问权限面板](https://msdn.microsoft.com/library/dn308586)应用程序的单一登录
  
在本教程中介绍的方案由以下构造块组成︰

1.  启用应用程序集成为 SuccessFactors
2.  配置单一登录
3.  配置用户设置
4.  分配用户

![方案](./media/active-directory-saas-successfactors-tutorial/IC791135.png "Scenario")

##启用应用程序集成为 SuccessFactors
  
本部分的目的是概述如何启用应用程序集成为 SuccessFactors。

###若要启用应用程序集成为 SuccessFactors，请执行以下步骤︰

1.  在 Azure 管理门户中，在左侧的导航窗格中，单击**活动目录**。

    ![活动目录](./media/active-directory-saas-successfactors-tutorial/IC700993.png "Active Directory")

2.  从**目录**列表中，选择要为其启用目录集成的目录。

3.  若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序](./media/active-directory-saas-successfactors-tutorial/IC700994.png "Applications")

4.  单击页面底部的**添加**。

    ![添加应用程序](./media/active-directory-saas-successfactors-tutorial/IC749321.png "Add application")

5.  在**您想要执行**对话框中，单击**添加应用程序从库**。

    ![从 gallerry 中添加应用程序](./media/active-directory-saas-successfactors-tutorial/IC749322.png "Add an application from gallerry")

6.  在**搜索框**中，键入**SuccessFactors**。

    ![应用程序库](./media/active-directory-saas-successfactors-tutorial/IC791136.png "Appliation Gallery")

7.  在结果窗格中，选择**SuccessFactors**，，然后单击**完成**添加应用程序。

    ![SuccessFactors](./media/active-directory-saas-successfactors-tutorial/IC791137.png "SuccessFactors")

##配置单一登录
  
本部分的目的是概述如何使用户能够使用基于 SAML 协议联合 Azure AD 中使用他们的帐户验证到 SuccessFactors。
  
若要获取单一登录配置，您需要联系您 SuccessFactors 的支持团队。

###若要配置单一登录，请执行以下步骤︰

1.  在 Azure 广告门户中， **SuccessFactors**应用程序集成在页上，单击**配置单一登录**来打开**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-successfactors-tutorial/IC791138.png "Configure Single Sign-On")

2.  在**您想怎样来登录到 SuccessFactors 的用户**页上**Microsoft Azure AD 单一登录**，选择，然后单击**下一步**。

    ![配置单一登录](./media/active-directory-saas-successfactors-tutorial/IC791139.png "Configure Single Sign-On")

3.  在**配置应用程序 URL**页上，执行下面的步骤，然后单击**下一步**。

    ![配置应用程序的 URL](./media/active-directory-saas-successfactors-tutorial/IC791140.png "Configure App URL")

    1.  在**SuccessFactors 号上的 URL**文本框中，键入用户用于登录到您的 SuccessFactors 应用程序 URL (例如:"*https://performancemanager4.successfactors.com/sf/home?company=CompanyName&loginMethod=SSO*")。
    2.  在**SuccessFactors 回复 URL**文本框中，键入**https://performancemanager4.successfactors.com/saml2/SAMLAssertionConsumer?company=CompanyName**。

        >[AZURE.NOTE] 此值只是一个临时占位符。  
        >实际值让您 SuccessFactors 的支持团队。  
        >稍后在本教程中，您发现联系方式您 SuccessFactors 的支持团队。  
        >在本次对话中，您将收到实际的 SuccessFactors 回复 URL。

4.  在**配置单一登录 SuccessFactors 在**页上，下载您的证书，单击**下载证书**，然后将保存在您的计算机上的证书文件。

    ![配置单一登录](./media/active-directory-saas-successfactors-tutorial/IC791141.png "Configure Single Sign-On")

5.  若要获取基于 SAML 单一登录配置，请联系您的 SuccessFactors 的支持团队和他们提供以下各项︰

    1.  下载的证书
    2.  远程登录 URL
    3.  远程注销 URL

    >[AZURE.IMPORTANT] 请让您的 SuccessFactors 的支持团队能够 NameId 格式将参数设置为"*未指定*"。

    Successfactors 的支持团队将向您发送正确的**Successfactors 回复 URL**所需的**配置应用程序 URL**对话框。

6.  在 Azure 广告门户，选择单一登录配置进行确认，然后单击**完成**关闭**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-successfactors-tutorial/IC791142.png "Configure Single Sign-On")

##配置用户设置
  
为了使 Azure AD 用户能够登录到 SuccessFactors，他们必须被调配到 SuccessFactors。  
对于 SuccessFactors，资源调配是手动任务。
  
若要获得在 SuccessFactors 中创建的用户，您需要与 SuccessFactors 的支持团队联系。

##分配用户
  
若要测试您的配置，您需要将 Azure 的 AD 用户授予您要允许使用通过将他们分配到您应用程序的访问权限。

###要将用户分配到 SuccessFactors 中，执行以下步骤︰

1.  在 Azure 广告门户中，创建一个测试帐户。

2.  在**SuccessFactors**应用程序集成页上，单击**将用户分配**。

    ![分配用户](./media/active-directory-saas-successfactors-tutorial/IC791143.png "Assign Users")

3.  选择测试用户，单击**分配**，然后单击**是**以确认您的分配。

    ![是](./media/active-directory-saas-successfactors-tutorial/IC767830.png "Yes")
  
如果您想要测试单个登录设置，打开后盖。 关于访问面板的详细信息，请参阅[访问权限面板简介](https://msdn.microsoft.com/library/dn308586)。
测试
