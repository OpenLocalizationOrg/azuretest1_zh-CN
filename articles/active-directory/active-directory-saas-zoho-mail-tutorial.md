---
ms.openlocfilehash: 0195837d010f1c8932f803d19dbc9e40ce8ce012
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties pageTitle="教程︰ Azure Active Directory 集成使用 Zoho 邮件 |Microsoft Azure" description="了解如何使用 Azure Active Directory Zoho 邮件启用单一登录、 自动化资源调配，和更多。" services="active-directory" authors="MarkusVi"  documentationCenter="na" manager="stevenpo"/>
<tags ms.service="active-directory" ms.devlang="na" ms.topic="article" ms.tgt_pltfrm="na" ms.workload="identity" ms.date="08/01/2015" ms.author="markvi" />
#教程︰ 使用 Zoho 邮件的 Azure Active Directory 集成
>[AZURE.TIP]反馈，请单击[此处](http://go.microsoft.com/fwlink/?LinkId=528578)。
  
本教程的目的是显示 Azure 和 Zoho 邮件的集成。  
在本教程中介绍的方案假定您已经有以下各项︰

-   有效的 Azure 订购
-   Zoho 邮件租户
  
完成本教程后, 分配到 Zoho 邮件 Azure AD 用户将能够到 Zoho 邮件公司网站 （服务提供程序初始化登录），或使用[简介访问权限面板](https://msdn.microsoft.com/library/dn308586)应用程序的单一登录
  
在本教程中介绍的方案由以下构造块组成︰

1.  启用对 Zoho 邮件的应用程序集成
2.  配置单一登录
3.  配置用户设置
4.  分配用户

![方案](./media/active-directory-saas-zoho-mail-tutorial/IC789600.png "Scenario")

##启用对 Zoho 邮件的应用程序集成
  
本部分的目的是概述如何启用 Zoho 邮件的应用程序集成。

###若要启用 Zoho 邮件的应用程序集成，请执行以下步骤︰

1.  在 Azure 管理门户中，在左侧的导航窗格中，单击**活动目录**。

    ![活动目录](./media/active-directory-saas-zoho-mail-tutorial/IC700993.png "Active Directory")

2.  从**目录**列表中，选择要为其启用目录集成的目录。

3.  若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序](./media/active-directory-saas-zoho-mail-tutorial/IC700994.png "Applications")

4.  单击页面底部的**添加**。

    ![添加应用程序](./media/active-directory-saas-zoho-mail-tutorial/IC749321.png "Add application")

5.  在**您想要执行**对话框中，单击**添加应用程序从库**。

    ![从 gallerry 中添加应用程序](./media/active-directory-saas-zoho-mail-tutorial/IC749322.png "Add an application from gallerry")

6.  在**搜索框**中，键入**Zoho 邮件**。

    ![应用程序库](./media/active-directory-saas-zoho-mail-tutorial/IC789601.png "Application Gallery")

7.  在结果窗格中，选择**Zoho 邮件**，然后单击**完成**添加应用程序。

    ![Zoho 邮件](./media/active-directory-saas-zoho-mail-tutorial/IC789602.png "Zoho Mail")

##配置单一登录
  
本部分的目的是概述如何使用户能够使用基于 SAML 协议联合 Azure AD 中使用他们的帐户验证到 Zoho 邮件。  
作为此过程的一部分，您将需要创建一个 base 64 编码的证书文件。  
如果您不熟悉此过程，请参阅[如何将转换到一个文本文件的二进制证书](http://youtu.be/PlgrzUZ-Y1o)。

###若要配置单一登录，请执行以下步骤︰

1.  在 Azure 广告门户中， **Zoho 邮件**应用程序集成在页上，单击**配置单一登录**来打开**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-zoho-mail-tutorial/IC789603.png "Configure Single Sign-On")

2.  在**您想怎样用户能够登录到 Zoho 邮件**页面上**Microsoft Azure AD 单一登录**，选择，然后单击**下一步**。

    ![配置单一登录](./media/active-directory-saas-zoho-mail-tutorial/IC789604.png "Configure Single Sign-On")

3.  在**配置应用程序 URL**页上，在**将邮件符号 Zoho 在 URL**文本框中，键入您的 URL 使用以下模式"*http://company.ZohoMail.com*"，然后单击**下一步**。

    ![配置应用程序的 URL](./media/active-directory-saas-zoho-mail-tutorial/IC789605.png "Configure App URL")

4.  在**配置单一登录在 Zoho 邮件**页面上，单击**下载证书**，然后将保存在您的计算机上的证书文件。

    ![配置单一登录](./media/active-directory-saas-zoho-mail-tutorial/IC789606.png "Configure Single Sign-On")

5.  在不同的 web 浏览器窗口中，以管理员身份登录到 Zoho 邮件公司网站。

6.  请转到**控制面板**。

    ![控制面板](./media/active-directory-saas-zoho-mail-tutorial/IC789607.png "Control Panel")

7.  单击**SAML 身份验证**选项卡。

    ![SAML 验证](./media/active-directory-saas-zoho-mail-tutorial/IC789608.png "SAML Authentication")

8.  在**SAML 验证详细信息**部分中，执行以下步骤︰

    ![SAML 验证详细信息](./media/active-directory-saas-zoho-mail-tutorial/IC789609.png "SAML Authentication Details")

    1.  在 Azure 的门户中，在**配置单一登录在 Zoho 邮件**对话框页复制的**远程登录 URL**值，，然后将其粘贴到**登录 URL**文本框。
    2.  在 Azure 的门户中，在**配置单一登录在 Zoho 邮件**对话框页复制**远程注销 URL**值，，然后将其粘贴到**注销 URL**文本框。
    3.  在 Azure 的门户中，在**配置单一登录在 Zoho 邮件**对话框页复制**更改密码 URL**值，，然后将其粘贴到文本框中**更改密码 URL** 。
    4.  创建您已下载的证书的**base 64 编码**的文件。  

        >[AZURE.TIP] 有关详细信息，请参阅[如何将转换到一个文本文件的二进制证书](http://youtu.be/PlgrzUZ-Y1o)

    5.  在记事本中打开您的 base 64 编码的证书，将其内容复制到剪贴板，然后将其粘贴到**PublicKey**文本框。
    6.  作为**算法**，选择**RSA**。
    7.  单击**确定**。

9.  在 Azure 广告门户，选择单一登录配置进行确认，然后单击**完成**关闭**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-zoho-mail-tutorial/IC789610.png "Configure Single Sign-On")

##配置用户设置
  
为了使 Azure AD 用户能够登录到 Zoho 邮件，他们必须到 Zoho 邮件设置。  
在 Zoho 邮件资源调配是手动任务。

###若要设置用户帐户，请执行以下步骤︰

1.  以管理员身份登录到**Zoho 邮件**公司网站。

2.  转到**控制面板\>邮件和文档**。

3.  转到**用户详细信息\>添加用户**。

    ![添加用户](./media/active-directory-saas-zoho-mail-tutorial/IC789611.png "Add User")

4.  在**添加用户**对话框中，请执行以下步骤︰

    ![添加用户](./media/active-directory-saas-zoho-mail-tutorial/IC789612.png "Add User")

    1.  键入的**名字**、**姓氏**、**电子邮件 ID**、 要到相关文本框设置有效的 Azure Active Directory 帐户的**密码**。
    2.  单击**确定**。  

        >[AZURE.NOTE] Azure Active Directory 帐户持有者会收到一封电子邮件，以确认该帐户，然后将变为活动状态的链接。

>[AZURE.NOTE] 您可以使用任何其他 Zoho 邮件用户帐户创建工具或 Api 提供的 Zoho 邮件来设置 AAD 用户帐户。

##分配用户
  
若要测试您的配置，您需要将 Azure 的 AD 用户授予您要允许使用通过将他们分配到您应用程序的访问权限。

###若要将用户分配到 Zoho 邮件，请执行以下步骤︰

1.  在 Azure 广告门户中，创建一个测试帐户。

2.  在**Zoho 邮件**应用程序集成页上，单击**将用户分配**。

    ![分配用户](./media/active-directory-saas-zoho-mail-tutorial/IC789613.png "Assign Users")

3.  选择测试用户，单击**分配**，然后单击**是**以确认您的分配。

    ![是](./media/active-directory-saas-zoho-mail-tutorial/IC767830.png "Yes")
  
如果您想要测试单个登录设置，打开后盖。 关于访问面板的详细信息，请参阅[访问权限面板简介](https://msdn.microsoft.com/library/dn308586)。
测试
