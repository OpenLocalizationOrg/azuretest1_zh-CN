---
ms.openlocfilehash: 885fef3c4c85dfd76bb807c5915e0394f69f6fcd
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties pageTitle="教程︰ Azure Active Directory 集成与 Panopto |Microsoft Azure" description="了解如何使用 Panopto Azure Active Directory 以启用单一登录、 自动化资源调配，和更多。" services="active-directory" authors="MarkusVi"  documentationCenter="na" manager="stevenpo"/>
<tags ms.service="active-directory" ms.devlang="na" ms.topic="article" ms.tgt_pltfrm="na" ms.workload="identity" ms.date="08/01/2015" ms.author="markvi" />
#教程︰ 与 Panopto 的 Azure Active Directory 集成
>[AZURE.TIP]反馈，请单击[此处](http://go.microsoft.com/fwlink/?LinkId=524766)。
  
本教程的目的是显示 Azure 和 Panopto 的集成。  
在本教程中介绍的方案假定您已经有以下各项︰

-   有效的 Azure 订购
-   Panopto 租户
  
后学完本教程，您已分配给 Panopto Azure AD 用户将能够到 Panopto 公司网站 （服务提供程序初始化登录），或使用[简介访问权限面板](https://msdn.microsoft.com/library/dn308586)应用程序的单一登录
  
在本教程中介绍的方案由以下构造块组成︰

1.  启用应用程序集成为 Panopto
2.  配置单一登录
3.  配置用户设置
4.  分配用户

![方案](./media/active-directory-saas-panopto-tutorial/IC777665.png "Scenario")
##启用应用程序集成为 Panopto
  
本部分的目的是概述如何启用应用程序集成为 Panopto。

###若要启用应用程序集成为 Panopto，请执行以下步骤︰

1.  在 Azure 管理门户中，在左侧的导航窗格中，单击**活动目录**。

    ![活动目录](./media/active-directory-saas-panopto-tutorial/IC700993.png "Active Directory")

2.  从**目录**列表中，选择要为其启用目录集成的目录。

3.  若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序](./media/active-directory-saas-panopto-tutorial/IC700994.png "Applications")

4.  单击页面底部的**添加**。

    ![添加应用程序](./media/active-directory-saas-panopto-tutorial/IC749321.png "Add application")

5.  在**您想要执行**对话框中，单击**添加应用程序从库**。

    ![从 gallerry 中添加应用程序](./media/active-directory-saas-panopto-tutorial/IC749322.png "Add an application from gallerry")

6.  在**搜索框**中，键入**Panopto**。

    ![Appkication 库](./media/active-directory-saas-panopto-tutorial/IC777666.png "Appkication Gallery")

7.  在结果窗格中，选择**Panopto**，，然后单击**完成**添加应用程序。

    ![Panopto](./media/active-directory-saas-panopto-tutorial/IC782936.png "Panopto")
##配置单一登录
  
本部分的目的是概述如何使用户能够使用基于 SAML 协议联合 Azure AD 中使用他们的帐户验证到 Panopto。  
作为此过程的一部分，您将需要创建一个 base 64 编码的证书文件。  
如果您不熟悉此过程，请参阅[如何将转换到一个文本文件的二进制证书](http://youtu.be/PlgrzUZ-Y1o)。

###若要配置单一登录，请执行以下步骤︰

1.  在 Azure 广告门户中， **Panopto**应用程序集成在页上，单击**配置单一登录**来打开**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-panopto-tutorial/IC777667.png "Configure single sign-on")

2.  在**您想怎样来登录到 Panopto 的用户**页上**Microsoft Azure AD 单一登录**，选择，然后单击**下一步**。

    ![配置单一登录](./media/active-directory-saas-panopto-tutorial/IC777668.png "Configure single sign-on")

3.  **配置应用程序 URL**页面上的**Panopto 号中 URL**文本框中，键入您的 URL 使用以下模式"*https://\<租户名称\>。Panopto.com*"，然后单击**下一步**。

    ![配置应用程序的 URL](./media/active-directory-saas-panopto-tutorial/IC777528.png "Configure app URL")

4.  在**配置单一登录 Panopto 在**页上，下载您的证书，单击**下载证书**，然后将保存在您的计算机上的证书文件。

    ![配置单一登录](./media/active-directory-saas-panopto-tutorial/IC777669.png "Configure single sign-on")

5.  在不同的 web 浏览器窗口中，以管理员身份登录到 Panopto 公司网站。

6.  在左侧工具栏中，单击**系统**，然后单击**标识提供程序**。

    ![System](./media/active-directory-saas-panopto-tutorial/IC777670.png "System")

7.  单击**添加提供程序**。

    ![身份提供程序](./media/active-directory-saas-panopto-tutorial/IC777671.png "Identity Providers")

8.  在 SAML 提供程序部分中，执行以下步骤︰

    ![SaaS 配置](./media/active-directory-saas-panopto-tutorial/IC777672.png "SaaS configuration")

    1.  从**提供程序类型**列表中，选择**SAML20**
    2.  在**实例名称**文本框中，键入该实例的名称。
    3.  在**好记的描述**文本框中，键入说明。
    4.  在 Azure 的门户中，在**配置单一登录在 Panopto**对话框页上，将**颁发者 URL**值，复制，然后将其粘贴到**颁发者**文本框。
    5.  在 Azure 的门户中，在**配置单一登录在 Panopto**对话框页上，将**SAML SSO URL**值，复制，然后将其粘贴到**反弹的网页 Url**文本框。
    6.  创建您已下载的证书的**base 64 编码**的文件。  

        >[AZURE.TIP] 有关详细信息，请参阅[如何将转换到一个文本文件的二进制证书](http://youtu.be/PlgrzUZ-Y1o)

    7.  在记事本中打开您的 base 64 编码的证书，将其内容复制到剪贴板，然后将其粘贴到**公钥**文本框
    8.  单击**保存**。
        ![保存](./media/active-directory-saas-panopto-tutorial/IC777673.png "Save")

9.  在 Azure 广告门户，选择单一登录配置进行确认，然后单击**完成**关闭**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-panopto-tutorial/IC777674.png "Configure single sign-on")
##配置用户设置
  
没有为您配置用户供应到 Panopto 操作项。  
当分派的用户尝试登录到使用访问权限面板中的 Panopto 时，Panopto 将检查用户是否存在。  
如果没有用户帐户可还，它是自动创建的 Panopto。

>[AZURE.NOTE]您可以使用任何其他 Panopto 用户帐户创建工具或 Api 提供的 Panopto 设置 Azure AD 用户帐户。

##分配用户
  
若要测试您的配置，您需要将 Azure 的 AD 用户授予您要允许使用通过将他们分配到您应用程序的访问权限。

###要将用户分配到 Panopto 中，执行以下步骤︰

1.  在 Azure 广告门户中，创建一个测试帐户。

2.  在**Panopto**应用程序集成页上，单击**将用户分配**。

    ![分配用户](./media/active-directory-saas-panopto-tutorial/IC777675.png "Assign users")

3.  选择测试用户，单击**分配**，然后单击**是**以确认您的分配。

    ![是](./media/active-directory-saas-panopto-tutorial/IC767830.png "Yes")
  
如果您想要测试单个登录设置，打开后盖。 关于访问面板的详细信息，请参阅[访问权限面板简介](https://msdn.microsoft.com/library/dn308586)。
测试
