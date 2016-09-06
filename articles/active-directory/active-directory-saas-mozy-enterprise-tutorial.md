---
ms.openlocfilehash: 59eb47a4cfd0ba342026ea134b4155566f3a1130
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties pageTitle="教程︰ Azure Active Directory 集成与 Mozy 企业 |Microsoft Azure" description="了解如何使用 Azure Active Directory Mozy 企业启用单一登录、 自动化资源调配，和更多。" services="active-directory" authors="MarkusVi"  documentationCenter="na" manager="stevenpo"/>
<tags ms.service="active-directory" ms.devlang="na" ms.topic="article" ms.tgt_pltfrm="na" ms.workload="identity" ms.date="08/01/2015" ms.author="markvi" />
#教程︰ 使用 Mozy 企业的 Azure Active Directory 集成
>[AZURE.TIP]反馈，请单击[此处](http://go.microsoft.com/fwlink/?LinkId=524186)。
  
本教程的目的是显示 Azure 和 Mozy 企业的集成。  
在本教程中介绍的方案假定您已经有以下各项︰

-   有效的 Azure 订购
-   Mozy 企业租户
  
后学完本教程，您已分配给 Mozy 企业 Azure AD 用户将能够到 Mozy 企业公司网站 （服务提供程序初始化登录），或使用[简介访问权限面板](https://msdn.microsoft.com/library/dn308586)应用程序的单一登录
  
在本教程中介绍的方案由以下构造块组成︰

1.  启用 Mozy 的企业应用程序集成
2.  配置单一登录
3.  配置用户设置
4.  分配用户

![方案](./media/active-directory-saas-mozy-enterprise-tutorial/IC777308.png "Scenario")
##启用 Mozy 的企业应用程序集成
  
本部分的目的是概述如何启用 Mozy 的企业应用程序集成。

###若要启用 Mozy 的企业应用程序集成，请执行以下步骤︰

1.  在 Azure 管理门户中，在左侧的导航窗格中，单击**活动目录**。

    ![活动目录](./media/active-directory-saas-mozy-enterprise-tutorial/IC700993.png "Active Directory")

2.  从**目录**列表中，选择要为其启用目录集成的目录。

3.  若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序](./media/active-directory-saas-mozy-enterprise-tutorial/IC700994.png "Applications")

4.  单击页面底部的**添加**。

    ![添加应用程序](./media/active-directory-saas-mozy-enterprise-tutorial/IC749321.png "Add application")

5.  在**您想要执行**对话框中，单击**添加应用程序从库**。

    ![从 gallerry 中添加应用程序](./media/active-directory-saas-mozy-enterprise-tutorial/IC749322.png "Add an application from gallerry")

6.  在**搜索框**中，键入**mozy 企业**。

    ![应用程序库](./media/active-directory-saas-mozy-enterprise-tutorial/IC777309.png "Application Gallery")

7.  在结果窗格中，选择**Mozy 企业**，，然后单击**完成**添加应用程序。

    ![Mozy 企业](./media/active-directory-saas-mozy-enterprise-tutorial/IC777310.png "Mozy Enterprise")
##配置单一登录
  
本部分的目的是概述如何使用户能够使用基于 SAML 协议联合 Azure AD 中使用他们的帐户验证到 Mozy 企业。  
作为此过程的一部分，您都需要将 base 64 编码的证书传到 Mozy 企业租户。  
如果您不熟悉此过程，请参阅[如何将转换到一个文本文件的二进制证书](http://youtu.be/PlgrzUZ-Y1o)

###若要配置单一登录，请执行以下步骤︰

1.  在 Azure 广告门户中， **Mozy 企业**应用程序集成在页上，单击**配置单一登录**来打开**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-mozy-enterprise-tutorial/IC771709.png "Configure single sign-on")

2.  **您希望如何登录到 Mozy 企业用户**在页上**Microsoft Azure AD 单一登录**，选择，然后单击**下一步**。

    ![配置单一登录](./media/active-directory-saas-mozy-enterprise-tutorial/IC777311.png "Configure single sign-on")

3.  在**配置应用程序 URL**页面上**Mozy 企业号在 URL**文本框中，键入您的 URL 使用以下模式"*https://\<租户名称\>。Mozyenterprise.com*"，然后单击**下一步**。

    ![配置应用程序的 URL](./media/active-directory-saas-mozy-enterprise-tutorial/IC777312.png "Configure app URL")

4.  在**配置单一登录 Mozy 企业在**页上，下载您的证书，单击**下载证书**，然后将保存在您的计算机上的证书文件。

    ![配置单一登录](./media/active-directory-saas-mozy-enterprise-tutorial/IC777313.png "Configure single sign-on")

5.  在不同的 web 浏览器窗口中，以管理员身份登录到您 Mozy 企业公司网站。

6.  在**配置**部分中，单击**身份验证策略**。

    ![身份验证策略](./media/active-directory-saas-mozy-enterprise-tutorial/IC777314.png "Authentication policy")

7.  在**身份验证策略**部分中，执行以下步骤︰

    ![身份验证策略](./media/active-directory-saas-mozy-enterprise-tutorial/IC777315.png "Authentication policy")

    1.  作为**提供程序**，请选择**目录服务**。
    2.  选择**使用 LDAP 推**。
    3.  单击**SAML 身份验证**选项卡。
    4.  在 Azure 的门户中，在**配置单一登录 Mozy 企业在**对话框页上，将**身份验证请求 URL**值，复制，然后将其粘贴到**身份验证 URL**文本框。
    5.  在 Azure 的门户中，在**配置单一登录 Mozy 企业在**对话框页面上，**标识提供程序 ID**值，复制，然后将其粘贴到文本框中**SAML 终结点**。
    6.  创建您已下载的证书的**Base 64 编码**的文件。  

        >[AZURE.TIP]有关详细信息，请参阅[如何将转换到一个文本文件的二进制证书](http://youtu.be/PlgrzUZ-Y1o)

    7.  在记事本中打开您的 base 64 编码的证书，将其内容复制到剪贴板， **SAML 证书**文本框中粘贴整个证书。
    8.  选择**启用 SSO 管理员登录他们的网络凭据**。
    9.  单击**保存更改**。

8.  在 Azure 的门户中，在**配置单一登录 Mozy 企业在**对话框页面上，选择单一登录配置进行确认，，然后单击**完成**。

    ![配置单一登录](./media/active-directory-saas-mozy-enterprise-tutorial/IC777316.png "Configure single sign-on")
##配置用户设置
  
为了使 Azure AD 用户能够登录到 Mozy 企业，他们必须到 Mozy 企业调配。  
对于 Mozy 企业资源调配是手动任务。

###若要设置用户帐户，请执行以下步骤︰

1.  登录到**Mozy 企业**租户。

2.  单击**用户**，然后单击**添加新用户**。

    ![用户](./media/active-directory-saas-mozy-enterprise-tutorial/IC777317.png "Users")

    >[AZURE.NOTE]只有**Mozy**选择**身份验证策略**下的提供程序仅显示**添加新用户**选项。 如果配置 SAML 验证然后用户会自动添加在首次登录通过单一登录上。

3.  在新建用户对话框中，请执行以下步骤︰

    ![添加用户](./media/active-directory-saas-mozy-enterprise-tutorial/IC777318.png "Add Users")

    1.  从**选择组**列表中，选择一个组。
    2.  **哪种类型的用户**列表中，选择一种类型。
    3.  在**用户名**文本框中，键入 Azure AD 用户的名称。
    4.  在**电子邮件**文本框中，键入 Azure AD 用户的电子邮件地址。
    5.  选择**发送用户指令电子邮件**。
    6.  单击**添加用户**。

    >[AZURE.NOTE]创建新用户后, 一封电子邮件将发送到 Azure AD 用户，包含一个链接，以确认该帐户，然后将变为活动状态。

>[AZURE.NOTE]您可以使用任何其他 Mozy 企业用户帐户创建工具或 Api 由 Mozy 企业来设置 AAD 用户帐户。

##分配用户
 
若要测试您的配置，您需要将 Azure 的 AD 用户授予您要允许使用通过将他们分配到您应用程序的访问权限。

###要为 Mozy 企业用户，请执行以下步骤︰

1.  在 Azure 广告门户中，创建一个测试帐户。

2.  **Mozy 企业**应用程序集成在页上，单击**将用户分配**。

    ![分配用户](./media/active-directory-saas-mozy-enterprise-tutorial/IC777319.png "Assign users")

3.  选择测试用户，单击**分配**，然后单击**是**以确认您的分配。

    ![是](./media/active-directory-saas-mozy-enterprise-tutorial/IC767830.png "Yes")
  
如果您想要测试单个登录设置，打开后盖。 关于访问面板的详细信息，请参阅[访问权限面板简介](https://msdn.microsoft.com/library/dn308586)。
测试
