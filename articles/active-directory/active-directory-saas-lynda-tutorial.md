---
ms.openlocfilehash: 9e1acb66f22cf11e330765bd35ced21c20d47244
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties pageTitle="教程︰ Azure Active Directory 集成与 Lynda.com |Microsoft Azure" description="了解如何使用 Lynda.com Azure Active Directory 以启用单一登录、 自动化资源调配，和更多。" services="active-directory" authors="MarkusVi"  documentationCenter="na" manager="stevenpo"/>
<tags ms.service="active-directory" ms.devlang="na" ms.topic="article" ms.tgt_pltfrm="na" ms.workload="identity" ms.date="08/01/2015" ms.author="markvi" />
#教程︰ 与 Lynda.com 的 Azure Active Directory 集成
>[AZURE.TIP]反馈，请单击[此处](http://go.microsoft.com/fwlink/?LinkId=524768)。
  
本教程的目的是显示 Azure 和 Lynda.com 的集成。  
在本教程中介绍的方案假定您已经有以下各项︰

-   有效的 Azure 订购
-   Lynda.com 租户
  
后学完本教程，您已分配给 Lynda.com Azure AD 用户将能够到 Lynda.com 公司网站 （服务提供程序初始化登录），或使用[简介访问权限面板](https://msdn.microsoft.com/library/dn308586)应用程序的单一登录
  
在本教程中介绍的方案由以下构造块组成︰

1.  启用应用程序集成为 Lynda.com
2.  配置单一登录
3.  配置用户设置
4.  分配用户

![方案](./media/active-directory-saas-lynda-tutorial/IC781046.png "Scenario")
##启用应用程序集成为 Lynda.com
  
本部分的目的是概述如何启用应用程序集成为 Lynda.com。

###若要启用应用程序集成为 Lynda.com，请执行以下步骤︰

1.  在 Azure 管理门户中，在左侧的导航窗格中，单击**活动目录**。

    ![活动目录](./media/active-directory-saas-lynda-tutorial/IC700993.png "Active Directory")

2.  从**目录**列表中，选择要为其启用目录集成的目录。

3.  若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序](./media/active-directory-saas-lynda-tutorial/IC700994.png "Applications")

4.  单击页面底部的**添加**。

    ![添加应用程序](./media/active-directory-saas-lynda-tutorial/IC749321.png "Add application")

5.  在**您想要执行**对话框中，单击**添加应用程序从库**。

    ![从 gallerry 中添加应用程序](./media/active-directory-saas-lynda-tutorial/IC749322.png "Add an application from gallerry")

6.  在**搜索框**中，键入**Lynda.com**。

    ![应用程序库](./media/active-directory-saas-lynda-tutorial/IC777524.png "Application Gallery")

7.  在结果窗格中，选择**Lynda.com**，，然后单击**完成**添加应用程序。

    ![Lynda.com](./media/active-directory-saas-lynda-tutorial/IC777525.png "Lynda.com")
##配置单一登录
  
本部分的目的是概述如何使用户能够使用基于 SAML 协议联合 Azure AD 中使用他们的帐户验证到 Lynda.com。

>[AZURE.IMPORTANT]为了能够在 Lynda.com 租户上配置单一登录，您需要首先联系 Lynda.com 技术支持以获取启用此功能。

###若要配置单一登录，请执行以下步骤︰

1.  在 Azure 广告门户中， **Lynda.com**应用程序集成在页上，单击**配置单一登录**来打开**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-lynda-tutorial/IC777526.png "Configure single sign-on")

2.  在**您想怎样来登录到 Lynda.com 的用户**页上**Microsoft Azure AD 单一登录**，选择，然后单击**下一步**。

    ![配置单一登录](./media/active-directory-saas-lynda-tutorial/IC777527.png "Configure single sign-on")

3.  在**配置应用程序 URL**页面上的**Lynda.com 号中 URL**文本框中，键入 Lynda.com 租户 URL (例如︰ *https://shib.lynda.com/Shibboleth.sso/InCommon?providerId=https://sts.windows-ppe.net/6247032d-9415-403c-b72b-277e3fb6f2c8/&target= https://shib.lynda.com/InCommon*)，然后单击**下一步**。

    ![配置应用程序的 URL](./media/active-directory-saas-lynda-tutorial/IC781047.png "Configure app URL")

4.  在**配置单一登录 Lynda.com 在**页面上，要下载元数据，请单击**下载元数据**，然后保存在您的计算机的本地证书文件。

    ![配置单一登录](./media/active-directory-saas-lynda-tutorial/IC777529.png "Configure single sign-on")

5.  将下载元数据文件发送到 Lynda.com 的支持团队。 Lynda.com 的支持团队会为您的单一登录配置。

6.  在 Azure 广告门户，选择单一登录配置进行确认，然后单击**完成**关闭**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-lynda-tutorial/IC777530.png "Configure single sign-on")
##配置用户设置
  
没有为您配置用户供应到 Lynda.com 操作项。  
当分派的用户尝试登录到使用访问权限面板中的 Lynda.com 时，Lynda.com 将检查用户是否存在。  
如果没有用户帐户可还，它是自动创建的 Lynda.com。

>[AZURE.NOTE]您可以使用任何其他 Lynda.com 用户帐户创建工具或 Api 来设置 AAD 用户帐户由 Lynda.com 提供。

##分配用户
  
若要测试您的配置，您需要将 Azure 的 AD 用户授予您要允许使用通过将他们分配到您应用程序的访问权限。

###要将用户分配到 Lynda.com 中，执行以下步骤︰

1.  在 Azure 广告门户中，创建一个测试帐户。

2.  在**Lynda.com**应用程序集成页上，单击**将用户分配**。

    ![分配用户](./media/active-directory-saas-lynda-tutorial/IC777531.png "Assign users")

3.  选择测试用户，单击**分配**，然后单击**是**以确认您的分配。

    ![是](./media/active-directory-saas-lynda-tutorial/IC767830.png "Yes")
  
如果您想要测试单个登录设置，打开后盖。 关于访问面板的详细信息，请参阅[访问权限面板简介](https://msdn.microsoft.com/library/dn308586)。
测试
