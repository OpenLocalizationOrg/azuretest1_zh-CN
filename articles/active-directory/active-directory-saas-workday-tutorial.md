---
ms.openlocfilehash: 2ca933a9a5e009bcdf2dcc6f2409fa835f6862ba
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties pageTitle="教程︰ Azure Active Directory 集成与工作日 |Microsoft Azure" description="了解如何使用 Azure Active Directory 工作日启用单一登录、 自动化资源调配，和更多。" services="active-directory" authors="MarkusVi"  documentationCenter="na" manager="stevenpo"/>
<tags ms.service="active-directory" ms.devlang="na" ms.topic="article" ms.tgt_pltfrm="na" ms.workload="identity" ms.date="08/01/2015" ms.author="markvi" />
#与工作日的教程︰ Azure Active Directory 集成
>[AZURE.TIP]反馈，请单击[此处](http://go.microsoft.com/fwlink/?LinkId=330042)。
  
本教程的目的是显示 Azure 和工作日的集成。 在本教程中介绍的方案假定您已经有以下各项︰

-   有效的 Azure 订购
-   在工作日中一个租户
  
在本教程中介绍的方案由以下构造块组成︰

1.  启用应用程序集成的工作日
2.  配置单一登录
3.  配置用户设置
4.  配置用户设置

![方案](./media/active-directory-saas-workday-tutorial/IC782919.png "Scenario")

##启用应用程序集成的工作日
  
本部分的目的是概述如何启用应用程序集成的队伍。

###若要启用应用程序集成为工作日，请执行以下步骤︰

1.  在 Azure 管理门户中，在左侧的导航窗格中，单击**活动目录**。

    ![活动目录](./media/active-directory-saas-workday-tutorial/IC700993.png "Active Directory")

2.  从**目录**列表中，选择要为其启用目录集成的目录。

3.  若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序](./media/active-directory-saas-workday-tutorial/IC700994.png "Applications")

4.  若要打开**应用程序库**，**添加应用程序**，请单击，然后单击**添加我的组织使用的应用程序**。

    ![您要做什么？](./media/active-directory-saas-workday-tutorial/IC700995.png "What do you want to do?")

5.  在**搜索框**中，键入**工作日**。

    ![工作日](./media/active-directory-saas-workday-tutorial/IC701021.png "Workday")

6.  在结果窗格中，选择**工作日**，，然后单击**完成**添加应用程序。

    ![工作日](./media/active-directory-saas-workday-tutorial/IC701022.png "Workday")

##配置单一登录
  
本部分的目的是概述如何使用户能够验证到工作日使用他们的帐户在 Azure 使用基于 SAML 协议联盟的广告。  
作为此过程的一部分，您将需要创建一个 base 64 编码的证书。  
如果您不熟悉此过程，请参阅[如何将转换到一个文本文件的二进制证书](http://youtu.be/PlgrzUZ-Y1o)。

###若要配置单一登录，请执行以下步骤︰

1.  在**工作日**应用程序集成页上，单击**配置单一登录**来打开**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-workday-tutorial/IC782920.png "Configure single sign-on")

2.  在**您想怎样来登录到工作日的用户**页上**Microsoft Azure AD 单一登录**，选择，然后单击**下一步**。

    ![配置单一登录](./media/active-directory-saas-workday-tutorial/IC782921.png "Configure single sign-on")

3.  在**配置应用程序 URL**页上，执行下面的步骤，然后单击**下一步**。

    ![配置应用程序的 URL](./media/active-directory-saas-workday-tutorial/IC782957.png "Configure App URL")

    1.  在**符号上 URL**文本框中，键入用户用于登录到工作日的 URL (例如︰ *https://impl.workday.com/\<租户\>/login-saml2.htmld*)
    2.  在**工作日答复 URL**文本框中，键入工作日答复 URL (例如︰ *https://impl.workday.com/\<租户\>/login-saml.htmld*)。

4.  在**配置单一登录在工作日**页中，下载您的证书，单击**下载证书**，然后将保存在您的计算机上的证书文件。

    ![配置单一登录](./media/active-directory-saas-workday-tutorial/IC782922.png "Configure single sign-on")

5.  在不同的 web 浏览器窗口中，以管理员身份登录到您工作日公司网站。

6.  转到**菜单\>工作台**。

    ![工作台](./media/active-directory-saas-workday-tutorial/IC782923.png "Workbench")

7.  转到**帐户管理**。

    ![帐户管理](./media/active-directory-saas-workday-tutorial/IC782924.png "Account Administration")

8.  转到**编辑租户安装 – 安全**。

    ![编辑租户安全](./media/active-directory-saas-workday-tutorial/IC782925.png "Edit Tenant Security")

9.  在**重定向 Url**部分中，执行以下步骤︰

    ![重定向 Url](./media/active-directory-saas-workday-tutorial/IC782958.png "Redirection URLs")

    1.  单击**添加行**。
    2.  在**登录重定向 URL**文本框和**移动重定向 URL**文本框中，键入您 Azure 门户的**配置应用程序 URL**页输入的**工作日租户 URL** 。
    3.  在**环境**文本框中，键入环境名称。  

        >[AZURE.NOTE] 环境属性的值与租户 URL 的值︰
        >
        >-   如果工作日租户的域名 URL 从开始实现 (如︰ *https://impl.workday.com/\<租户\>/login-saml2.htmld*)，**环境**属性必须设置为实现。
        >-   如果以停下手中的开头的域名，您需要联系工作日来获得匹配的**环境**价值。

10. 在**SAML 安装**部分中，执行以下步骤︰

    ![SAML 安装程序](./media/active-directory-saas-workday-tutorial/IC782926.png "SAML Setup")

    1.  选择**启用 SAML 验证**。
    2.  单击**添加行**。

11. 在 SAML 身份提供程序部分中，执行以下步骤︰

    ![SAML 身份提供程序](./media/active-directory-saas-workday-tutorial/IC782927.png "SAML Identity Providers")

    1.  在身份提供程序名称文本框中，键入提供程序的名称 (例如︰ *SPInitiatedSSO*)。
    2.  在 Azure 的门户中，在**配置单一登录在工作日**对话框页的**标识提供程序 ID**值，复制，然后将其粘贴到**颁发者**文本框。
    3.  **公用密钥证书的标识提供程序**，请单击，然后单击**创建**。
        ![创建](./media/active-directory-saas-workday-tutorial/IC782928.png "Create")
    4.  单击**创建 x509 公钥**。
        ![创建](./media/active-directory-saas-workday-tutorial/IC782929.png "Create")
    5.  在**查看 x509 公用密钥**部分中，执行以下步骤︰![视图 x509 公钥](./media/active-directory-saas-workday-tutorial/IC782930.png "View x509 Public Key")
        1.  在**名称**文本框中，键入证书的名称 (例如︰ *PPE\_SP*)。
        2.  在**有效期自**文本框中，键入有效的从您的证书的属性值。
        3.  中的**失效日期**文本框中，键入您的证书的属性值无效。
        
            >[AZURE.NOTE] 您可以从日期和有效日期从下载证书通过双击它以获得有效。 在**详细信息**选项卡列出了日期。

        4.  创建您已下载的证书的**Base 64 编码**的文件。  

            >[AZURE.TIP] 有关详细信息，请参阅[如何将转换到一个文本文件的二进制证书](http://youtu.be/PlgrzUZ-Y1o)

        5.  在记事本中打开您 base 64 编码的证书，然后复制其内容。
        6.  在**证书**文本框中，粘贴剪贴板中的内容。
        7.  单击**确定**。

    6.  执行以下步骤︰ ![SSO 配置](./media/active-directory-saas-workday-tutorial/IC792131.png "SSO configuration")
        1.  在**服务提供商 ID**文本框中，键入**http://www.workday.com**。
        2.  选择**启用 SP 启动 SAML 验证**。
        3.  在 Azure 的门户中，在**配置单一登录在工作日**对话框页面上，**单一登录服务 URL**值，复制，然后将其粘贴到**IdP SSO 服务 URL**文本框。
        4.  选择**不地道 SP 启动身份验证请求**。

    7.  执行以下步骤︰![身份验证请求签名方法](./media/active-directory-saas-workday-tutorial/IC782932.png "Authentication Request Signature Method")
        1.  作为**身份验证请求签名方法**，选择**SHA256**。

    8.  单击**确定**。
        ![确定](./media/active-directory-saas-workday-tutorial/IC782933.png "OK")

12. 在 Azure 广告门户中，**配置单一登录在工作日**在页上，单击**完成**关闭对话框。

    ![配置单一登录](./media/active-directory-saas-workday-tutorial/IC782934.png "Configure single sign-on")

##配置用户设置
  
若要获取工作日中置备一个测试用户，您需要与工作日支持团队联系。  
工作日的支持团队将为您创建该用户。

##分配用户
  
若要测试您的配置，您需要将 Azure 的 AD 用户授予您要允许使用通过将他们分配到您应用程序的访问权限。

###若要将用户分配到工作日，请执行以下步骤︰

1.  在 Azure 广告门户中，创建一个测试帐户。

2.  在**工作日**应用程序集成页上，单击**将用户分配**。

    ![分配用户](./media/active-directory-saas-workday-tutorial/IC782935.png "Assign Users")

3.  选择测试用户，单击**分配**，然后单击**是**以确认您的分配。

    ![是](./media/active-directory-saas-workday-tutorial/IC767830.png "Yes")
  
如果您想要测试单个登录设置，打开后盖。 关于访问面板的详细信息，请参阅[访问权限面板简介](https://msdn.microsoft.com/library/dn308586)。
测试
