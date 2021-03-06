---
ms.openlocfilehash: b148d8198100325c5788e8e4349e8b7765564c2a
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties pageTitle="教程︰ Azure Active Directory 集成与特使 |Microsoft Azure" description="了解如何使用 Azure Active Directory 特使以启用单一登录、 自动化资源调配，和更多。" services="active-directory" authors="MarkusVi"  documentationCenter="na" manager="stevenpo"/>
<tags ms.service="active-directory" ms.devlang="na" ms.topic="article" ms.tgt_pltfrm="na" ms.workload="identity" ms.date="08/01/2015" ms.author="markvi" />
#教程︰ 与特使的 Azure Active Directory 集成
>[AZURE.TIP]反馈，请单击[此处](http://go.microsoft.com/fwlink/?LinkId=524324)。
  
本教程的目的是显示 Azure 和特使的集成。  
在本教程中介绍的方案假定您已经有以下各项︰

-   有效的 Azure 订购
-   特使租户
  
后学完本教程，您已分配给特使 Azure AD 用户将能够到特使公司网站 （服务提供程序初始化登录），或使用[简介访问权限面板](https://msdn.microsoft.com/library/dn308586)应用程序的单一登录
  
在本教程中介绍的方案由以下构造块组成︰

1.  启用应用程序集成的特使
2.  配置单一登录
3.  配置用户设置
4.  分配用户

![方案](./media/active-directory-saas-envoy-tutorial/IC776759.png "Scenario")
##启用应用程序集成的特使
  
本部分的目的是概述如何启用应用程序集成为特使。

###若要启用应用程序集成的特使，执行以下步骤︰

1.  在 Azure 管理门户中，在左侧的导航窗格中，单击**活动目录**。

    ![活动目录](./media/active-directory-saas-envoy-tutorial/IC700993.png "Active Directory")

2.  从**目录**列表中，选择要为其启用目录集成的目录。

3.  若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序](./media/active-directory-saas-envoy-tutorial/IC700994.png "Applications")

4.  单击页面底部的**添加**。

    ![添加应用程序](./media/active-directory-saas-envoy-tutorial/IC749321.png "Add application")

5.  在**您想要执行**对话框中，单击**添加应用程序从库**。

    ![从 gallerry 中添加应用程序](./media/active-directory-saas-envoy-tutorial/IC749322.png "Add an application from gallerry")

6.  在**搜索框**中，键入**特使**。

    ![应用程序库](./media/active-directory-saas-envoy-tutorial/IC776760.png "Application gallery")

7.  在结果窗格中，选择**代表**，，然后单击**完成**添加应用程序。

    ![特使](./media/active-directory-saas-envoy-tutorial/IC776777.png "Envoy")
##配置单一登录
  
本部分的目的是概述如何使用户能够使用基于 SAML 协议联合 Azure AD 中使用他们的帐户验证到特使。  
配置单一登录的特使要求检索证书的指纹值。  
如果您不熟悉此过程，请参阅[如何检索证书的指纹值](http://youtu.be/YKQF266SAxI)

###若要配置单一登录，请执行以下步骤︰

1.  在 Azure 广告门户中，**特使**应用程序集成在页上，单击**配置单一登录**来打开**配置单一登录**对话框。

    ![启用单一登录](./media/active-directory-saas-envoy-tutorial/IC776778.png "Enable single sign-on")

2.  在**您想怎样用户登录特使到**页上，选择**Microsoft Azure AD 单一登录**，，然后单击**下一步**。

    ![配置单一登录](./media/active-directory-saas-envoy-tutorial/IC776779.png "Configure single sign-on")

3.  在**配置应用程序 URL**页上，在**特使号在 URL**文本框中，键入您的 URL 使用以下模式"*https://\<租户名称\>。Envoy.com*"，然后单击**下一步**。

    ![配置应用程序的 URL](./media/active-directory-saas-envoy-tutorial/IC776780.png "Configure app URL")

4.  在**配置单一登录特使在**页上，下载您的证书，单击**证书下载**，然后保存为本地证书文件**c:\\Envoy.cer**。

    ![配置单一登录](./media/active-directory-saas-envoy-tutorial/IC776781.png "Configure single sign-on")

5.  在不同的 web 浏览器窗口中，以管理员身份登录到您特使公司网站。

6.  在顶部工具栏上，单击**设置**。

    ![特使](./media/active-directory-saas-envoy-tutorial/IC776782.png "Envoy")

7.  单击**公司**。

    ![单位](./media/active-directory-saas-envoy-tutorial/IC776783.png "Company")

8.  单击**SAML**。

    ![SAML](./media/active-directory-saas-envoy-tutorial/IC776784.png "SAML")

9.  在**SAML 验证**配置部分中，执行以下步骤︰

    ![SAML 验证](./media/active-directory-saas-envoy-tutorial/IC776785.png "SAML authentication")

    >[AZURE.NOTE] 总部的位置 ID 的值是自动生成的应用程序。

    1.  从导出的证书，复制的**指纹**值，然后将其粘贴到**指纹**文本框。  

        >[AZURE.TIP] 有关详细信息，请参阅[如何检索证书的指纹值](http://youtu.be/YKQF266SAxI)

    2.  在 Azure 的门户中，在**配置单一登录在特使**对话框页**SAML SSO URL**值，复制，然后将其粘贴到**标识提供程序 HTTP SAML URL**文本框。
    3.  单击**保存更改**。

10. 在 Azure 广告门户，选择单一登录配置进行确认，然后单击**完成**关闭**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-envoy-tutorial/IC776786.png "Configure single sign-on")
##配置用户设置
  
没有要将资源调配给特使的用户配置的操作项。  
当分派的用户尝试登录到使用访问权限面板中的特使时，特使将检查用户是否存在。  
如果没有用户帐户可还，它是自动创建的特使。
##分配用户
  
若要测试您的配置，您需要将 Azure 的 AD 用户授予您要允许使用通过将他们分配到您应用程序的访问权限。

###要将用户分配到特使，执行以下步骤︰

1.  在 Azure 广告门户中，创建一个测试帐户。

2.  **代表**应用程序集成在页上，单击**将用户分配**。

    ![分配用户](./media/active-directory-saas-envoy-tutorial/IC776787.png "Assign users")

3.  选择测试用户，单击**分配**，然后单击**是**以确认您的分配。

    ![是](./media/active-directory-saas-envoy-tutorial/IC767830.png "Yes")
  
如果您想要测试单个登录设置，打开后盖。 关于访问面板的详细信息，请参阅[访问权限面板简介](https://msdn.microsoft.com/library/dn308586)。
测试
