---
ms.openlocfilehash: 83d42334b7eebc2e4009f3ba116563f3fe622ec3
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties pageTitle="教程︰ Azure Active Directory 集成与 InsideView |Microsoft Azure" description="了解如何使用 InsideView Azure Active Directory 以启用单一登录、 自动化资源调配，和更多。" services="active-directory" authors="MarkusVi"  documentationCenter="na" manager="stevenpo"/>
<tags ms.service="active-directory" ms.devlang="na" ms.topic="article" ms.tgt_pltfrm="na" ms.workload="identity" ms.date="08/01/2015" ms.author="markvi" />
#教程︰ 与 InsideView 的 Azure Active Directory 集成
>[AZURE.TIP]反馈，请单击[此处](http://go.microsoft.com/fwlink/?LinkId=529077)。
  
本教程的目的是显示 Azure 和 InsideView 的集成。  
在本教程中介绍的方案假定您已经有以下各项︰

-   有效的 Azure 订购
-   InsideView 租户
  
后学完本教程，您已分配给 InsideView Azure AD 用户将能够到 InsideView 公司网站 （服务提供程序初始化登录），或使用[简介访问权限面板](https://msdn.microsoft.com/library/dn308586)应用程序的单一登录
  
在本教程中介绍的方案由以下构造块组成︰

1.  启用应用程序集成为 InsideView
2.  配置单一登录
3.  配置用户设置
4.  分配用户

![方案](./media/active-directory-saas-insideview-tutorial/IC794128.png "Scenario")
##启用应用程序集成为 InsideView
  
本部分的目的是概述如何启用应用程序集成为 InsideView。

###若要启用应用程序集成为 InsideView，请执行以下步骤︰

1.  在 Azure 管理门户中，在左侧的导航窗格中，单击**活动目录**。

    ![活动目录](./media/active-directory-saas-insideview-tutorial/IC700993.png "Active Directory")

2.  从**目录**列表中，选择要为其启用目录集成的目录。

3.  若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序](./media/active-directory-saas-insideview-tutorial/IC700994.png "Applications")

4.  单击页面底部的**添加**。

    ![添加应用程序](./media/active-directory-saas-insideview-tutorial/IC749321.png "Add application")

5.  在**您想要执行**对话框中，单击**添加应用程序从库**。

    ![从 gallerry 中添加应用程序](./media/active-directory-saas-insideview-tutorial/IC749322.png "Add an application from gallerry")

6.  在**搜索框**中，键入**InsideView**。

    ![应用程序库](./media/active-directory-saas-insideview-tutorial/IC794129.png "Application Gallery")

7.  在结果窗格中，选择**InsideView**，，然后单击**完成**添加应用程序。

    ![InsideView](./media/active-directory-saas-insideview-tutorial/IC794130.png "InsideView")
##配置单一登录
  
本部分的目的是概述如何使用户能够使用基于 SAML 协议联合 Azure AD 中使用他们的帐户验证到 InsideView。  
作为此过程的一部分，您将需要创建一个 base 64 编码的证书文件。  
如果您不熟悉此过程，请参阅[如何将转换到一个文本文件的二进制证书](http://youtu.be/PlgrzUZ-Y1o)。

###若要配置单一登录，请执行以下步骤︰

1.  在 Azure 广告门户中， **InsideView**应用程序集成在页上，单击**配置单一登录**来打开**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-insideview-tutorial/IC794131.png "Configure Single SignOn")

2.  在**您想怎样来登录到 InsideView 的用户**页上**Microsoft Azure AD 单一登录**，选择，然后单击**下一步**。

    ![配置单一登录](./media/active-directory-saas-insideview-tutorial/IC794132.png "Configure Single SignOn")

3.  在**配置应用程序 URL**页面上的**InsideView 回复 URL**文本框中，键入 InsideView SSO URL (例如︰ `https://my.insideview.com/iv/<STS Name>/login.iv`)，然后单击**下一步**。

    ![配置应用程序的 URL](./media/active-directory-saas-insideview-tutorial/IC794133.png "Configure App URL")

4.  在**配置单一登录 InsideView 在**页上，下载您的证书，单击**下载证书**，然后将保存在您的计算机上的证书文件。

    ![配置单一登录](./media/active-directory-saas-insideview-tutorial/IC794134.png "Configure Single SignOn")

5.  在不同的 web 浏览器窗口中，以管理员身份登录到 InsideView 公司网站。

6.  在顶部工具栏中，单击**管理**， **SingleSignOn 设置**，然后单击**添加 SAML**。

    ![SAML 单一登录设置](./media/active-directory-saas-insideview-tutorial/IC794135.png "SAML Single Sign On Settings")

7.  在**添加新的 SAML**部分中，执行以下步骤︰

    ![添加新的 SAML](./media/active-directory-saas-insideview-tutorial/IC794136.png "Add a New SAML")

    1.  在**STS 名称**文本框中，键入您的配置的名称。
    2.  在 Azure 的门户中，在**配置单一登录在 InsideView**对话框页面上，复制的**服务提供商 (SP) 启动终结点**值，，然后将其粘贴到文本框中**SamlP/WS-进 Unsolicated 终结点**。
    3.  创建您已下载的证书的**base 64 编码**的文件。
        
        >[AZURE.TIP]有关详细信息，请参阅[如何将转换到一个文本文件的二进制证书](http://youtu.be/PlgrzUZ-Y1o)

    4.  在记事本中打开您的 base 64 编码的证书，将其内容复制到剪贴板，然后将其粘贴到**STS 证书**文本框
    5.  在**Crm 用户 Id 映射**文本框中，键入**http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**。
    6.  在**Crm 电子邮件映射**文本框中，键入**http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**。
    7.  在**Crm 名字映射**文本框中，键入**http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**。
    8.  在**映射 Crm 姓氏**文本框中，键入**http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname**。
    9.  单击**保存**。

8.  在 Azure 广告门户，选择单一登录配置进行确认，然后单击**完成**关闭**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-insideview-tutorial/IC794137.png "Configure Single SignOn")
##配置用户设置
  
为了使 Azure AD 用户能够登录到 InsideView，他们必须到 InsideView 配置。  
对于 InsideView，资源调配是手动任务。
  
若要获取用户或在 InsideView 中创建的联系人，请联系您的客户成功经理或将电子邮件发送到**support@insideview.com**

>[AZURE.NOTE] 您可以使用任何其他 InsideView 用户帐户创建工具或 Api 提供的 InsideView 设置 Azure AD 用户帐户。

##分配用户
  
若要测试您的配置，您需要将 Azure 的 AD 用户授予您要允许使用通过将他们分配到您应用程序的访问权限。

###要将用户分配到 InsideView 中，执行以下步骤︰

1.  在 Azure 广告门户中，创建一个测试帐户。

2.  在**InsideView**应用程序集成页上，单击**将用户分配**。

    ![分配用户](./media/active-directory-saas-insideview-tutorial/IC794138.png "Assign Users")

3.  选择测试用户，单击**分配**，然后单击**是**以确认您的分配。

    ![是](./media/active-directory-saas-insideview-tutorial/IC767830.png "Yes")
  
如果您想要测试单个登录设置，打开后盖。 关于访问面板的详细信息，请参阅[访问权限面板简介](https://msdn.microsoft.com/library/dn308586)。
测试
