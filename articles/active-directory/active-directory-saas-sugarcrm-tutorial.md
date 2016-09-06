---
ms.openlocfilehash: d4fcd8aa99c7c7877c01b65ee1dc27d3704f2b3d
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties pageTitle="教程︰ 与 SugarCRM 的 Azure Active Directory 集成集成 |Microsoft Azure" description="了解如何使用 Azure Active Directory SugarCRM 启用单一登录、 自动化资源调配，和更多。" services="active-directory" authors="MarkusVi"  documentationCenter="na" manager="stevenpo"/>
<tags ms.service="active-directory" ms.devlang="na" ms.topic="article" ms.tgt_pltfrm="na" ms.workload="identity" ms.date="08/01/2015" ms.author="markvi" />
#教程︰ 与 SugarCRM 的 Azure Active Directory 集成集成
>[AZURE.TIP]反馈，请单击[此处](http://go.microsoft.com/fwlink/?LinkId=534768)。
  
本教程的目的是显示 Azure，糖 CRM 的集成。  
在本教程中介绍的方案假定您已经有以下各项︰

-   有效的 Azure 订购
-   糖 CRM-启用单一登录的订阅
  
后学完本教程，您已分配给糖 CRM Azure AD 用户将能够到糖 CRM 公司网站 （服务提供程序初始化登录），或使用[简介访问权限面板](https://msdn.microsoft.com/library/dn308586)应用程序的单一登录
  
在本教程中介绍的方案由以下构造块组成︰

1.  启用糖 CRM 的应用程序集成
2.  配置单一登录
3.  配置用户设置
4.  分配用户

![方案](./media/active-directory-saas-sugarcrm-tutorial/IC795881.png "Scenario")

##启用糖 CRM 的应用程序集成
  
本部分的目的是概述如何启用糖 CRM 的应用程序集成。

###若要启用糖 CRM 的应用程序集成，请执行以下步骤︰

1.  在 Azure 管理门户中，在左侧的导航窗格中，单击**活动目录**。

    ![活动目录](./media/active-directory-saas-sugarcrm-tutorial/IC700993.png "Active Directory")

2.  从**目录**列表中，选择要为其启用目录集成的目录。

3.  若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序](./media/active-directory-saas-sugarcrm-tutorial/IC700994.png "Applications")

4.  单击页面底部的**添加**。

    ![添加应用程序](./media/active-directory-saas-sugarcrm-tutorial/IC749321.png "Add application")

5.  在**您想要执行**对话框中，单击**添加应用程序从库**。

    ![从 gallerry 中添加应用程序](./media/active-directory-saas-sugarcrm-tutorial/IC749322.png "Add an application from gallerry")

6.  在**搜索框**中，键入**糖 CRM**。

    ![应用程序库](./media/active-directory-saas-sugarcrm-tutorial/IC795882.png "Application Gallery")

7.  在结果窗格中，选择**糖 CRM**，然后单击**完成**添加应用程序。

    ![CRM 的糖](./media/active-directory-saas-sugarcrm-tutorial/IC795883.png "Sugar CRM")

##配置单一登录
  
本部分的目的是概述如何使用户能够使用基于 SAML 协议联合 Azure AD 中使用他们的帐户验证到糖 CRM。  
作为此过程的一部分，您都需要将 base 64 编码的证书传到糖 CRM 租户。  
如果您不熟悉此过程，请参阅[如何将转换到一个文本文件的二进制证书](http://youtu.be/PlgrzUZ-Y1o)

###若要配置单一登录，请执行以下步骤︰

1.  在 Azure 广告门户中，**糖 CRM**应用程序集成在页上，单击**配置单一登录**来打开**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-sugarcrm-tutorial/IC795884.png "Configure Single Sign-On")

2.  在**您想怎样来登录到糖 CRM 的用户**页上**Microsoft Azure AD 单一登录**，选择，然后单击**下一步**。

    ![配置单一登录](./media/active-directory-saas-sugarcrm-tutorial/IC795885.png "Configure Single Sign-On")

3.  **配置应用程序 URL**页面上的**糖 CRM 号在 URL**文本框中，键入您为登录到糖 CRM 应用程序的用户使用的 URL (例如:"*http://company.sugarondemand.com*"，然后单击**下一步**。

    ![配置应用程序的 URL](./media/active-directory-saas-sugarcrm-tutorial/IC795886.png "Configure App URL")

4.  在**配置单一登录糖 CRM 在**页上，下载您的证书，单击**下载证书**，然后将保存在您的计算机上的证书文件。

    ![配置单一登录](./media/active-directory-saas-sugarcrm-tutorial/IC796918.png "Configure Single Sign-On")

5.  在不同的 web 浏览器窗口中，以管理员身份登录到糖 CRM 公司网站。

6.  转到**管理**。

    ![管理员](./media/active-directory-saas-sugarcrm-tutorial/IC795888.png "Admin")

7.  在**管理**部分中，单击**密码管理**。

    ![管理](./media/active-directory-saas-sugarcrm-tutorial/IC795889.png "Administration")

8.  选择**启用 SAML 验证**。

    ![管理](./media/active-directory-saas-sugarcrm-tutorial/IC795890.png "Administration")

9.  在**SAML 验证**部分中，执行以下步骤︰

    ![SAML 验证](./media/active-directory-saas-sugarcrm-tutorial/IC795891.png "SAML Authentication")

    1.  在 Azure 的门户中，在**配置单一登录在糖 CRM**对话框页复制的**远程登录 URL**值，，然后将其粘贴到**登录 URL**文本框。
    2.  在 Azure 的门户中，在**配置单一登录在糖 CRM**对话框页复制的**远程登录 URL**值，，然后将其粘贴到**SLO URL**文本框。
    3.  创建您已下载的证书的**Base 64 编码**的文件。

        >[AZURE.TIP] 有关详细信息，请参阅[如何将转换到一个文本文件的二进制证书](http://youtu.be/PlgrzUZ-Y1o)

    4.  在记事本中打开您的 base 64 编码的证书，将其内容复制到剪贴板，然后将整个证书粘贴到文本框**的 X.509 证书**。
    5.  单击**保存**。

10. 在 Azure 的门户中，在**配置单一登录在糖 CRM**对话框页面上，选择单一登录配置进行确认，，然后单击**完成**。

    ![配置单一登录](./media/active-directory-saas-sugarcrm-tutorial/IC796919.png "Configure Single Sign-On")

##配置用户设置
  
为了使 Azure AD 用户能够登录到糖 CRM，他们必须对糖 CRM 配置。  
对于糖 CRM 资源调配是手动任务。

###若要设置用户帐户，请执行以下步骤︰

1.  以管理员身份登录到您的**糖 CRM**公司网站。

2.  转到**管理**。

    ![管理员](./media/active-directory-saas-sugarcrm-tutorial/IC795888.png "Admin")

3.  在**管理**部分中，单击**用户管理**。

    ![管理](./media/active-directory-saas-sugarcrm-tutorial/IC795893.png "Administration")

4.  转到**的用户\>创建新用户**。

    ![创建新用户](./media/active-directory-saas-sugarcrm-tutorial/IC795894.png "Create New User")

5.  在**用户配置文件**选项卡，请执行以下步骤︰

    ![新用户](./media/active-directory-saas-sugarcrm-tutorial/IC795895.png "New User")

    1.  相关的文本框中键入用户名称、 有效的 Azure Active Directory 用户的姓氏和电子邮件地址。

6.  作为**状态**，选择**活动**。

7.  在密码选项卡，请执行以下步骤︰

    ![新用户](./media/active-directory-saas-sugarcrm-tutorial/IC795896.png "New User")

    1.  相关的文本框中键入的密码。
    2.  单击**保存**。

>[AZURE.NOTE] 您可以使用任何其他糖 CRM 用户帐户创建工具或 Api 来设置 AAD 用户帐户提供糖 CRM。

##分配用户
  
若要测试您的配置，您需要将 Azure 的 AD 用户授予您要允许使用通过将他们分配到您应用程序的访问权限。

###若要将用户分配到糖 CRM 中，请执行以下步骤︰

1.  在 Azure 广告门户中，创建一个测试帐户。

2.  在**糖 CRM**应用程序集成页上，单击**将用户分配**。

    ![分配用户](./media/active-directory-saas-sugarcrm-tutorial/IC795897.png "Assign Users")

3.  选择测试用户，单击**分配**，然后单击**是**以确认您的分配。

    ![是](./media/active-directory-saas-sugarcrm-tutorial/IC767830.png "Yes")
  
如果您想要测试单个登录设置，打开后盖。 关于访问面板的详细信息，请参阅[访问权限面板简介](https://msdn.microsoft.com/library/dn308586)。
测试
