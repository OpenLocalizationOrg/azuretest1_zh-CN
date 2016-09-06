---
ms.openlocfilehash: e6b0534832471041e2cf9d4384d657aecf29ca35
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties pageTitle="教程︰ Azure Active Directory 集成与自适应套件 |Microsoft Azure" description="了解如何使用 Azure Active Directory 自适应套件以启用单一登录、 自动化资源调配，和更多。" services="active-directory" authors="MarkusVi"  documentationCenter="na" manager="stevenpo"/>
<tags ms.service="active-directory" ms.devlang="na" ms.topic="article" ms.tgt_pltfrm="na" ms.workload="identity" ms.date="08/01/2015" ms.author="markvi" />
#教程︰ Azure Active Directory 集成与自适应套件
>[AZURE.TIP]反馈，请单击[此处](http://go.microsoft.com/fwlink/?LinkId=615749)。

本教程的目的是显示 Azure 和自适应的套件的集成。  
在本教程中介绍的方案假定您已经有以下各项︰

-   有效的 Azure 订购
-   自适应套件租户

在完成本教程之后, 已分配到自适应套件 Azure AD 用户将能够单一登录到应用程序中自适应的套件公司网站 （服务提供程序初始化登录），或使用[接入面板简介](https://msdn.microsoft.com/library/dn308586)

在本教程中介绍的方案由以下构造块组成︰

1.  启用自适应套件的应用程序集成
2.  配置单一登录
3.  配置用户设置
4.  分配用户

![方案](./media/active-directory-saas-adaptive-suite-tutorial/IC805637.png "Scenario")
##启用自适应套件的应用程序集成

本部分的目的是概述如何启用自适应套件应用程序集成。

###若要启用应用程序集成为自适应的套件，请执行以下步骤︰

1.  在 Azure 管理门户中，在左侧的导航窗格中，单击**活动目录**。

    ![活动目录](./media/active-directory-saas-adaptive-suite-tutorial/IC700993.png "Active Directory")

2.  从**目录**列表中，选择要为其启用目录集成的目录。

3.  若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序](./media/active-directory-saas-adaptive-suite-tutorial/IC700994.png "Applications")

4.  单击页面底部的**添加**。

    ![添加应用程序](./media/active-directory-saas-adaptive-suite-tutorial/IC749321.png "Add application")

5.  在**您想要执行**对话框中，单击**添加应用程序从库**。

    ![从 gallerry 中添加应用程序](./media/active-directory-saas-adaptive-suite-tutorial/IC749322.png "Add an application from gallerry")

6.  在**搜索框**中，键入**自适应的套件**。

    ![应用程序库](./media/active-directory-saas-adaptive-suite-tutorial/IC805638.png "Application Gallery")

7.  在结果窗格中，选择**自适应的套件**，，然后单击**完成**添加应用程序。

    ![自适应套件](./media/active-directory-saas-adaptive-suite-tutorial/IC805639.png "Adaptive Suite")
##配置单一登录

本部分的目的是概述如何使用户能够使用基于 SAML 协议联合 Azure AD 中使用他们的帐户验证到自适应套件。  
针对自适应套件的单一登录配置需要检索证书的指纹值。  
如果您不熟悉此过程，请参阅[如何检索证书的指纹值](http://youtu.be/YKQF266SAxI)。

###若要配置单一登录，请执行以下步骤︰

1.  在 Azure 广告门户中，在**自适应套件**应用程序集成页上，单击**配置单一登录**来打开**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-adaptive-suite-tutorial/IC805640.png "Configure Single Sign-On")

2.  在**您想怎样来登录到自适应套件的用户**页上**Microsoft Azure AD 单一登录**，选择，然后单击**下一步**。

    ![配置单一登录](./media/active-directory-saas-adaptive-suite-tutorial/IC805641.png "Configure Single Sign-On")

3.  **配置应用程序设置**页上的**回复 URL**文本框中，键入您的 URL 使用以下模式"*https://login.adaptiveinsights.com:443/samlsso/RlJFRVRSSUFMMTI3MTE =*"，然后单击**下一步**。

    >[AZURE.NOTE] 从自适应套件的**SAML SSO 设置**页，您可以获取此值。

    ![配置应用程序设置](./media/active-directory-saas-adaptive-suite-tutorial/IC805642.png "Configure App Settings")

4.  在**配置单一登录方式进行自适应的套件在**页上，下载您的证书，单击**下载证书**，然后将保存在您的计算机的本地证书文件。

    ![配置单一登录](./media/active-directory-saas-adaptive-suite-tutorial/IC805643.png "Configure Single Sign-On")

5.  在不同的 web 浏览器窗口中，为自适应的套件公司网站管理员身份登录。

6.  转到**管理**。

    ![管理员](./media/active-directory-saas-adaptive-suite-tutorial/IC805644.png "Admin")

7.  在**用户和角色**部分中，单击**管理 SAML SSO 设置**。

    ![管理 SAML SSO 设置](./media/active-directory-saas-adaptive-suite-tutorial/IC805645.png "Manage SAML SSO Settings")

8.  在**SAML SSO 设置**页上，请执行以下步骤︰

    ![SAML SSO 设置](./media/active-directory-saas-adaptive-suite-tutorial/IC805646.png "SAML SSO Settings")

    1.  在**身份提供程序名称**文本框中，键入您的配置的名称。
    2.  在 Azure 的门户中，在**配置单一登录方式进行自适应的套件在**对话框页面上，**实体 ID**值，复制，然后将其粘贴到**身份标识提供程序实体 ID**文本框。
    3.  在 Azure 的门户中，在**配置单一登录方式进行自适应的套件在**对话框页面上，将**SAML SSO URL**值，复制，然后将其粘贴到**身份提供 SSO URL**文本框。
    4.  在 Azure 的门户中，在**配置单一登录方式进行自适应的套件在**对话框页上，将**SAML SSO URL**值，复制，然后将其粘贴到**自定义注销 URL**文本框。
    5.  若要上载您已下载的证书，请单击**选择文件**。
    6.  作为**SAML 用户 id**，选择**用户的见解自适应用户名称**。
    7.  作为**SAML 用户 id 位置**，选择**NameID 的主题中的用户 id**。
    8.  作为**SAML NameID 格式**，请选择**电子邮件地址**。
    9.  为**使 SAML**，选择**允许 SAML SSO 和直接自适应见解登录**。
    10. 单击**保存**。

9.  在 Azure 广告门户，选择单一登录配置进行确认，然后单击**完成**关闭**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-adaptive-suite-tutorial/IC805647.png "Configure Single Sign-On")
##配置用户设置

为了使 Azure AD 用户能够登录到自适应的套件，他们必须到自适应套件设置。  
在自适应套件，资源调配是手动任务。

###若要配置用户设置，请执行以下步骤︰

1.  以管理员身份登录到您的**自适应套件**公司网站。

2.  转到**管理**。

    ![管理员](./media/active-directory-saas-adaptive-suite-tutorial/IC805644.png "Admin")

3.  在**用户和角色**部分中，单击**添加用户**。

    ![添加用户](./media/active-directory-saas-adaptive-suite-tutorial/IC805648.png "Add User")

4.  在**新建用户**部分中，执行以下步骤︰

    ![提交](./media/active-directory-saas-adaptive-suite-tutorial/IC805649.png "Submit")

    1.  键入**名称**、**登录**、**电子邮件**、 您要到相关文本框提供的有效 Azure Active Directory 用户的**密码**。
    2.  选择一个**角色**。
    3.  单击**提交**。

>[AZURE.NOTE] 您可以使用任何其他自适应套件用户帐户创建工具或 Api 提供的自适应套件来设置 AAD 用户帐户。

##分配用户

若要测试您的配置，您需要将 Azure 的 AD 用户授予您要允许使用通过将他们分配到您应用程序的访问权限。

###若要将用户分配到自适应的套件，请执行以下步骤︰

1.  在 Azure 广告门户中，创建一个测试帐户。

2.  在**自适应套件**应用程序集成页上，单击**将用户分配**。

    ![分配用户](./media/active-directory-saas-adaptive-suite-tutorial/IC805650.png "Assign Users")

3.  选择测试用户，单击**分配**，然后单击**是**以确认您的分配。

    ![是](./media/active-directory-saas-adaptive-suite-tutorial/IC767830.png "Yes")

如果您想要测试单个登录设置，打开后盖。 关于访问面板的详细信息，请参阅[访问权限面板简介](https://msdn.microsoft.com/library/dn308586)。

测试
