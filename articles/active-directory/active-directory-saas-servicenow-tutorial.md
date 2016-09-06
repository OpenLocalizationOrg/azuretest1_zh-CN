---
ms.openlocfilehash: ee1923ed8a5349611eea7b8e0fcb6c9d90c1bf41
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties pageTitle="教程︰ Azure Active Directory 集成与 ServiceNow |Microsoft Azure" description="了解如何使用 ServiceNow Azure Active Directory 以启用单一登录、 自动化资源调配，和更多。" services="active-directory" authors="MarkusVi"  documentationCenter="na" manager="stevenpo"/>
<tags ms.service="active-directory" ms.devlang="na" ms.topic="article" ms.tgt_pltfrm="na" ms.workload="identity" ms.date="08/01/2015" ms.author="markvi" />
#教程︰ 与 ServiceNow 的 Azure Active Directory 集成
>[AZURE.TIP]反馈，请单击[此处](http://go.microsoft.com/fwlink/?LinkId=521880)。
  
本教程的目的是显示 Azure 和 ServiceNow 的集成。  
在本教程中介绍的方案假定您已经有以下各项︰

-   有效的 Azure 订购
-   在 ServiceNow 中承租人
  
后学完本教程，您已分配给 ServiceNow Azure AD 用户将能够到 ServiceNow 公司网站 （服务提供程序初始化登录），或使用[简介访问权限面板](active-directory-saas-access-panel-introduction.md)应用程序的单一登录
  
在本教程中介绍的方案由以下构造块组成︰

1.  启用应用程序集成为 ServiceNow
2.  配置单一登录
3.  配置用户设置
4.  分配用户

![方案](./media/active-directory-saas-servicenow-tutorial/IC769496.png "Scenario")
##启用应用程序集成为 ServiceNow
  
本部分的目的是概述如何启用应用程序集成为 ServiceNow。

###若要启用应用程序集成为 ServiceNow，请执行以下步骤︰

1.  在 Azure 管理门户中，在左侧的导航窗格中，单击**活动目录**。

    ![活动目录](./media/active-directory-saas-servicenow-tutorial/IC700993.png "Active Directory")

2.  从**目录**列表中，选择要为其启用目录集成的目录。

3.  若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序](./media/active-directory-saas-servicenow-tutorial/IC700994.png "Applications")

4.  单击页面底部的**添加**。

    ![添加应用程序](./media/active-directory-saas-servicenow-tutorial/IC749321.png "Add application")

5.  在**您想要执行**对话框中，单击**添加应用程序从库**。

    ![从 gallerry 中添加应用程序](./media/active-directory-saas-servicenow-tutorial/IC749322.png "Add an application from gallerry")

6.  在**搜索框**中，键入**ServiceNow**。

    ![应用程序库](./media/active-directory-saas-servicenow-tutorial/IC701016.png "Application gallery")

7.  在结果窗格中，选择**ServiceNow**，，然后单击**完成**添加应用程序。

    ![ServiceNow](./media/active-directory-saas-servicenow-tutorial/IC701017.png "ServiceNow")
##配置单一登录
  
本部分的目的是概述如何使用户能够使用基于 SAML 协议联合 Azure AD 中使用他们的帐户验证到 ServiceNow。

作为此过程的一部分，您都需要将 base 64 编码的证书上传到您的商业租户的收存箱。 如果您不熟悉此过程，请参阅[如何将转换到一个文本文件的二进制证书](http://youtu.be/PlgrzUZ-Y1o)。

###若要配置单一登录，请执行以下步骤︰

1.  在 Azure 广告门户中， **ServiceNow**应用程序集成在页上，单击**配置单一登录**来打开**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-servicenow-tutorial/IC749323.png "Configure single sign-on")

2.  在**您想怎样来登录到 ServiceNow 的用户**页上**Microsoft Azure AD 单一登录**，选择，然后单击**下一步**。

    ![配置单一登录](./media/active-directory-saas-servicenow-tutorial/IC749324.png "Configure single sign-on")

3.  **配置应用程序 URL**页面上的**ServiceNow 号中 URL**文本框中，键入您的 URL 使用以下模式"*https://<InstanceName>。 servicenow.com*"，然后单击**下一步**。

    ![配置应用程序的 URL](./media/active-directory-saas-servicenow-tutorial/IC769497.png "Configure app URL")

4.  在**配置单一登录 ServiceNow 在**页上，单击**下载证书**，保存在您的计算机的本地证书文件，然后单击**下一步**。

    ![配置单一登录](./media/active-directory-saas-servicenow-tutorial/IC749325.png "Configure single sign-on")

5.  在您 ServiceNow 租户，在左侧导航栏上单击**属性**以打开**SAML 2.0 单一登录属性**页。


6. 在**SAML 2.0 单一登录属性**页上，请执行以下步骤︰

     6.1。 为**启用外部身份验证**，请选择**是**。


     6.2。 在**这将发出 SAML2 安全令牌包含用户信息的标识提供程序 URL**文本框中，键入**https://sts.windows.net/<your 租户 GUID > /**。


     6.3。 在**标识提供程序的 AuthnRequest 服务的基 URL**文本框中，键入**https://login.windows.net/<your 租户 GUID > / saml2**。


     6.4。 在**标识提供程序的 SingleLogoutRequest 服务的基 URL**文本框中，键入**https://login.windows.net/<your 租户 GUID > / saml2**。


     6.5。 在**标识提供程序的 SingleLogoutRequest 服务的协议绑定**文本框中，键入**urn: oasis︰ 姓名︰ tc: SAML:2.0:bindings:HTTP-重定向**。

     6.6。 作为**符号 LogoutRequest**，选择**是**。

     6.7。 在**当 SAML 2.0 单一登录将失败，因为未经过身份验证的会话，或者这是第一次登录，重定向到此 URL**文本框中，键入**https://login.windows.net/<your 租户 GUID > / saml2**。

  

7. 在**服务提供程序 （服务现在） 属性**部分中，执行以下步骤︰

     7.1。 在**服务现在的 URL 实例主页**文本框中，键入您的 ServiceNow 实例主页的 URL。 ServiceNow 实例主页的 URL 是**ServiceNow 租户 URL**和**/navpage.do**的串联︰ **https://<InstanceName>.service-now.com/navpage.do** <br><br>   ![服务现在实例主页](./media/active-directory-saas-servicenow-tutorial/IC700342.png "Service-now instance homepage")


     7.2。 在**实体标识或颁发者**文本框中，键入您组织的 URL。

     7.3。 在**接受 SAML2 标记的观众 uri**文本框中，键入您组织的 URL。

     7.4。 在**用户表字段以匹配主题的 NameID 元素在 SAMLResponse**文本框中，键入**电子邮件**。

     7.5。 **NameID 策略用于返回主题的 NameID SAMLResponse 在**文本框中键入**urn: oasis︰ 姓名︰ tc: SAML:1.1:nameid 的格式︰ 未指定**。

     7.6 取消选中**创建 AuthnRequest 语句中的 AuthnContextClass 请求**。

     7.7 在**AuthnContextClassRef 方法中我们 SAML 2.0 AuthnRequest 为身份标识提供程序将包含**文本框中，键入**http://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password**。



8. 在**高级设置**部分中，执行以下步骤︰

     8.1。 在**"notBefore"约束之前或之后"notOnOrAfter"约束考虑仍然有效的秒数**文本框中，键入**60**。


9. 要保存配置，请单击**保存**。

10. 在左侧导航栏上，单击**证书**以打开**证书**页。



11. 若要上载您的证书，在证书页上，执行以下步骤︰

     11.1。 单击**新建**。

     11.2。 在**名称**文本框中，键入**SAML 2.0**。

     11.3。 选择**活动**。

     11.4。 作为**格式**，请选择**PEM**。

     11.5。 从下载证书创建 Base 64 编码的文件。  >[AZURE.NOTE]更多详细信息，请参阅[如何将转换到一个文本文件的二进制证书](http://youtu.be/PlgrzUZ-Y1o)。

     11.6。 在**记事本**，Base 64 编码的文件，将其打开，然后将此文件的内容复制到剪贴板。

     11.7。 将剪贴板中的内容粘贴到**PEM 证书**文本框。

     11.8。 单击**提交**。



12. 在 Azure 广告门户，选择单一登录配置进行确认，然后单击完成关闭配置单一登录对话框。 <br><br> ![配置单一登录](./media/active-directory-saas-servicenow-tutorial/IC749326.png "Configure single sign-on")




##配置用户设置
  
本部分的目的是概述如何启用 Active Directory 用户帐户添加到 ServiceNow 的用户供应。


### 若要配置用户设置，请执行以下步骤︰

1. 在 Azure 管理门户中， **ServiceNow**应用程序集成在页上，单击**配置用户设置**。 <br><br> ![用户资源调配](./media/active-directory-saas-servicenow-tutorial/IC769498.png "User provisioning")


2. 在**输入 ServiceNow 启用自动用户提供的凭据**页提供以下配置设置︰ 配置用户的资源调配 

     2.1。 在**ServiceNow 实例名称**文本框中，键入 ServiceNow 实例名称。

     2.2。 在**ServiceNow 管理员用户名**文本框中，键入 ServiceNow 管理员帐户的名称。

     2.3。 在**ServiceNow 管理员密码**文本框中，键入此帐户的密码。

     2.4。 单击**验证**来验证您的配置。

     2.5。 单击**下一步**按钮以打开**后续步骤**页。

     2.6。 如果您想要设置为此应用程序的所有用户，请选择"都**自动资源都调配到此应用程序目录中的所有用户帐户**"。 <br><br> ![下一步行动](./media/active-directory-saas-servicenow-tutorial/IC698804.png "Next Steps")

     2.7。 在**接下来的步骤**页面上，单击**完成**以保存配置。











##分配用户
  
若要测试您的配置，您需要将 Azure 的 AD 用户授予您要允许使用通过将他们分配到您应用程序的访问权限。

###要将用户分配到 ServiceNow 中，执行以下步骤︰

1.  在 Azure 广告门户中，创建一个测试帐户。

2.  在**ServiceNow**应用程序集成页上，单击**将用户分配**。

    ![分配用户](./media/active-directory-saas-servicenow-tutorial/IC769499.png "Assign users")

3.  选择测试用户，单击**分配**，然后单击**是**以确认您的分配。

    ![是](./media/active-directory-saas-servicenow-tutorial/IC767830.png "Yes")
  
如果您想要测试单个登录设置，打开后盖。 关于访问面板的详细信息，请参阅[访问权限面板简介](active-directory-saas-access-panel-introduction.md)。


## 其他资源

* [如何将与 Azure Active Directory 集成在 SaaS 应用程序的教程列表](active-directory-saas-tutorial-list.md)
* [应用程序访问权限和单一登录使用 Azure 活动目录是什么？](active-directory-appssoaccess-whatis.md)测试
