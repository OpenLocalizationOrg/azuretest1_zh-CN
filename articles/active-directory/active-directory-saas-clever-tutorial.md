---
ms.openlocfilehash: 4d0368e3db25d7e47a059267bbc434c87ec7cdf0
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties pageTitle="教程︰ Azure Active Directory 集成与 Clever |Microsoft Azure" description="了解如何使用 Azure Active Directory Clever 启用单一登录、 自动化资源调配，和更多。" services="active-directory" authors="MarkusVi"  documentationCenter="na" manager="stevenpo"/>
<tags ms.service="active-directory" ms.devlang="na" ms.topic="article" ms.tgt_pltfrm="na" ms.workload="identity" ms.date="08/01/2015" ms.author="markvi" />
#教程︰ 与 Clever 的 Azure Active Directory 集成
>[AZURE.TIP]反馈，请单击[此处](http://go.microsoft.com/fwlink/?LinkId=551005)。

本教程的目的是显示 Azure 和 Clever 集成。 在本教程中介绍的方案假定您已经有以下各项︰

-   有效的 Azure 订购
-   聪明的租户

后学完本教程，您已分配给 Clever Azure AD 用户将能够到聪明的公司网站 （服务提供程序初始化登录），或使用[简介访问权限面板](https://msdn.microsoft.com/library/dn308586)应用程序的单一登录

在本教程中介绍的方案由以下构造块组成︰

1.  启用应用程序集成为 Clever
2.  配置单一登录
3.  配置用户设置
4.  分配用户

![方案](./media/active-directory-saas-clever-tutorial/IC798977.png "Scenario")
##启用应用程序集成为 Clever

本部分的目的是概述如何启用 Clever 用于应用程序集成。

###若要启用 Clever 用于应用程序集成，请执行以下步骤︰

1.  在 Azure 管理门户中，在左侧的导航窗格中，单击**活动目录**。

    ![活动目录](./media/active-directory-saas-clever-tutorial/IC700993.png "Active Directory")

2.  从**目录**列表中，选择要为其启用目录集成的目录。

3.  若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序](./media/active-directory-saas-clever-tutorial/IC700994.png "Applications")

4.  单击页面底部的**添加**。

    ![添加应用程序](./media/active-directory-saas-clever-tutorial/IC749321.png "Add application")

5.  在**您想要执行**对话框中，单击**添加应用程序从库**。

    ![从 gallerry 中添加应用程序](./media/active-directory-saas-clever-tutorial/IC749322.png "Add an application from gallerry")

6.  在**搜索框**中，键入**Clever**。

    ![应用程序库](./media/active-directory-saas-clever-tutorial/IC798978.png "Application Gallery")

7.  在结果窗格中，选择**Clever**，，然后单击**完成**添加应用程序。

    ![聪明](./media/active-directory-saas-clever-tutorial/IC798979.png "Clever")
##配置单一登录

本部分的目的是概述如何使用户能够使用基于 SAML 协议联合 Azure AD 中使用他们的帐户验证到 Clever。  
巧妙地应用程序需要以特定的格式，这就需要你对**saml 令牌属性**配置中添加自定义属性映射 SAML 断言。  
下面的屏幕快照显示此示例。

![属性](./media/active-directory-saas-clever-tutorial/IC798980.png "Attributes")

###若要配置单一登录，请执行以下步骤︰

1.  在 Azure 广告门户中，在**Clever**集成应用程序页，单击**配置单一登录**来打开**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-clever-tutorial/IC784682.png "Configure Single Sign-On")

2.  在**如何您希望用户能够登录到 Clever**页中**Microsoft Azure AD 单一登录**，选择，然后单击**下一步**。

    ![配置单一登录](./media/active-directory-saas-clever-tutorial/IC798981.png "Configure Single Sign-On")

3.  在**配置应用程序 URL**页上，在**聪明的符号在 URL**文本框中，键入您为登录到您聪明的应用程序的用户使用的 URL (例如︰ *https://clever.com/in/azsandbox*)，然后单击**下一步**。

    ![配置应用程序的 URL](./media/active-directory-saas-clever-tutorial/IC798982.png "Configure App URL")

4.  在**配置单一登录 Clever 在**页上，要下载元数据，请单击**下载元数据**，然后保存在您的计算机上的本地元数据文件。

    ![配置单一登录](./media/active-directory-saas-clever-tutorial/IC798983.png "Configure Single Sign-On")

5.  在不同的 web 浏览器窗口中，以管理员身份登录到聪明的公司网站。

6.  在工具栏中，单击**即时登录**。

    ![即时登录](./media/active-directory-saas-clever-tutorial/IC798984.png "Instant Login")

7.  在**即时登录**页面上，请执行以下步骤︰

    ![即时登录](./media/active-directory-saas-clever-tutorial/IC798985.png "Instant Login")

    1.  键入**登录 URL**。  

        >[AZURE.NOTE] **登录 URL**为自定义值。
您可以获取实际值的聪明的支持团队。

    2.  作为**身份识别系统**，选择**ADF**。
    3.  单击**保存**。

8.  在 Azure 广告门户，选择单一登录配置进行确认，然后单击**完成**关闭**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-clever-tutorial/IC798986.png "Configure Single Sign-On")

9.  在顶部菜单中，单击**属性**以打开**SAML 令牌的属性**对话框。

    ![属性](./media/active-directory-saas-clever-tutorial/IC795920.png "Attributes")

10. 若要添加所需的属性映射，请执行以下步骤︰

    ![saml 令牌的属性](./media/active-directory-saas-clever-tutorial/IC795921.png "saml token attributes")

  	|属性名称|属性值|
  	|---|---|
  	|clever.student.credentials.district\_用户名|User.userprincipalname|

    1.  上表中每个数据行，请单击**添加用户属性**。
    2.  在**属性名称**文本框中，键入该行显示的属性名称。
    3.  在**属性值**文本框中，选择的行中显示的属性值。
    4.  单击**完成**。

11. 单击**应用更改**。

##配置用户设置

为了使 Azure AD 用户能够登录到 Clever，他们必须到 Clever 调配。  
对于 Clever，资源调配是需要聪明的支持团队执行的手动任务。

>[AZURE.NOTE] 您可以使用任何其他聪明的用户帐户创建工具或 Api 提供 Clever 来设置 AAD 用户帐户。

##分配用户

若要测试您的配置，您需要将 Azure 的 AD 用户授予您要允许使用通过将他们分配到您应用程序的访问权限。

###若要将用户分配到 Clever，请执行以下步骤︰

1.  在 Azure 广告门户中，创建一个测试帐户。

2.  在**Clever**集成应用程序页，单击**将用户分配**。

    ![分配用户](./media/active-directory-saas-clever-tutorial/IC798987.png "Assign Users")

3.  选择测试用户，单击**分配**，然后单击**是**以确认您的分配。

    ![是](./media/active-directory-saas-clever-tutorial/IC767830.png "Yes")

如果您想要测试单个登录设置，打开后盖。 关于访问面板的详细信息，请参阅[访问权限面板简介](https://msdn.microsoft.com/library/dn308586)。

测试
