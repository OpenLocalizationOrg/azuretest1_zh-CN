---
ms.openlocfilehash: ee892ae03c887737a56aa29e13e2b73416d43fe6
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties pageTitle="教程︰ Azure Active Directory 集成框 |Microsoft Azure" description="了解如何使用 Azure Active Directory 框以启用单一登录、 自动化资源调配，和更多。" services="active-directory" authors="MarkusVi"  documentationCenter="na" manager="stevenpo"/>
<tags ms.service="active-directory" ms.devlang="na" ms.topic="article" ms.tgt_pltfrm="na" ms.workload="identity" ms.date="08/01/2015" ms.author="markvi" />




#教程︰ Azure Active Directory 集成框


>[AZURE.TIP]反馈，请单击[此处](http://go.microsoft.com/fwlink/?LinkId=522410)。
  
本教程的目的是显示集成的 Azure 和框。  
在本教程中介绍的方案假定您已经有以下各项︰

-   有效的 Azure 订购
-   在框中测试租户
  
后学完本教程，您已分配给框 Azure AD 用户将能够到框公司网站 （服务提供程序初始化登录），或使用[简介访问权限面板](active-directory-saas-access-panel-introduction.md)应用程序的单一登录
  
在本教程中介绍的方案由以下构造块组成︰

1.  启用应用程序集成框
2.  配置单一登录
3.  配置用户设置
4.  分配用户

![方案](./media/active-directory-saas-box-tutorial/IC769537.png "Scenario")



##启用应用程序集成框
  
本部分的目的是概述如何启用应用程序集成的框。

###若要启用应用程序集成的框中，请执行以下步骤︰

1.  在 Azure 管理门户中，在左侧的导航窗格中，单击**活动目录**。

    ![活动目录](./media/active-directory-saas-box-tutorial/IC700993.png "Active Directory")

2.  从**目录**列表中，选择要为其启用目录集成的目录。

3.  若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序](./media/active-directory-saas-box-tutorial/IC700994.png "Applications")

4.  单击页面底部的**添加**。

    ![添加应用程序](./media/active-directory-saas-box-tutorial/IC749321.png "Add application")

5.  在**您想要执行**对话框中，单击**添加应用程序从库**。

    ![从 gallerry 中添加应用程序](./media/active-directory-saas-box-tutorial/IC749322.png "Add an application from gallerry")

6.  在**搜索框**中，键入**框**。

    ![应用程序库](./media/active-directory-saas-box-tutorial/IC701023.png "Application gallery")

7.  在结果窗格中，选中**框**，，然后单击**完成**添加应用程序。

    ![盒状](./media/active-directory-saas-box-tutorial/IC701024.png "Box")



##配置单一登录
  
本部分的目的是概述如何使用户能够通过身份验证框中使用他们的帐户在 Azure AD 使用基于 SAML 协议的联合。 <br>
作为此过程的一部分，您需要将元数据上传到 Box.com。

###若要配置单一登录，请执行以下步骤︰

1.  在 Azure 广告门户中，**框**应用程序集成在页上，单击**配置单一登录**来打开**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-box-tutorial/IC769538.png "Configure single sign-on")

2.  在**您想怎样来登录到框中的用户**页上**Microsoft Azure AD 单一登录**，选择，然后单击**下一步**。

    ![配置单一登录](./media/active-directory-saas-box-tutorial/IC769539.png "Configure single sign-on")

3.  在**配置应用程序 URL**页面上的**框租户 URL**文本框中，键入框租户 URL (例如︰ https://<mydomainname>。 box.com)，然后单击**下一步**。

    ![配置应用程序的 URL](./media/active-directory-saas-box-tutorial/IC669826.png "Configure app URL")

4.  在**配置单一登录框在**页面上，可以下载元数据，请单击**下载元数据**，然后在您的计算机上的本地数据文件。

    ![配置单一登录](./media/active-directory-saas-box-tutorial/IC669824.png "Configure single sign-on")

5.  转发到框中该元数据文件支持团队。 支持团队需要为您的单一登录配置。

6.  选择单一登录配置进行确认，然后单击**完成**关闭**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-box-tutorial/IC769540.png "Configure single sign-on")
##配置用户设置
  
本部分的目的是概述如何启用 Active Directory 中的用户帐户的资源调配。

###若要配置单一登录，请执行以下步骤︰

1. 在 Azure 管理门户中，**框**应用程序集成在页上，单击**配置用户设置**以打开**配置用户设置**对话框。 <br> <br> ![启用用户自动资源调配](./media/active-directory-saas-box-tutorial/IC769541.png "Enable automatic user provisioning")

2. 在**启用用户供应到框**对话框页面上，单击**启用用户供应**。 <br><br>  ![启用用户自动资源调配](./media/active-directory-saas-box-tutorial/IC769544.png "Enable automatic user provisioning")

3. 在**登录框为授予访问权限**页上提供所需的凭据，然后单击**授权**。 <br><br> ![启用用户自动资源调配](./media/active-directory-saas-box-tutorial/IC769546.png "Enable automatic user provisioning")


4. 单击**授予访问权限框中的**授权此操作并返回到 Azure 管理门户网站。 <br><br> ![启用用户自动资源调配](./media/active-directory-saas-box-tutorial/IC769549.png "Enable automatic user provisioning")

5. 若要完成配置，请单击完成按钮。 <br><br> ![启用用户自动资源调配](./media/active-directory-saas-box-tutorial/IC769551.png "Enable automatic user provisioning")



##分配用户
  
若要测试您的配置，您需要将 Azure 的 AD 用户授予您要允许使用通过将他们分配到您应用程序的访问权限。

###若要将用户分配到框中，请执行以下步骤︰

1. 在 Azure 广告门户中，创建一个测试帐户。

2. 在**框中**应用程序集成页上，单击**将用户分配**。 <br><br> ![分配用户](./media/active-directory-saas-box-tutorial/IC769552.png "Assign users")

3.  选择测试用户，单击**分配**，然后单击**是**以确认您的分配。 <br><br> ![是](./media/active-directory-saas-box-tutorial/IC767830.png "Yes")
  

您现在应该等待 10 分钟并验证帐户的已同步到框中。

作为第一个验证步骤，您可以检查供应状态，通过单击本页框应用程序集成 Azure 管理门户上 D 中的仪表板。

<br><br> ![仪表板](./media/active-directory-saas-box-tutorial/IC769553.png "Dashboard")

由相关的状态指示已成功完成资源调配周期用户︰

<br><br> ![集成状态](./media/active-directory-saas-box-tutorial/IC769555.png "Integration status")


在框中您租户中，同步的用户列在**管理控制台**中的**管理用户**。

<br><br> ![集成状态](./media/active-directory-saas-box-tutorial/IC769556.png "Integration status")


## 其他资源

* [如何将与 Azure Active Directory 集成在 SaaS 应用程序的教程列表](active-directory-saas-tutorial-list.md)
* [应用程序访问权限和单一登录使用 Azure 活动目录是什么？](active-directory-appssoaccess-whatis.md)测试
