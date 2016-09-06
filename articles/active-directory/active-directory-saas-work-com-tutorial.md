---
ms.openlocfilehash: 257c07c544368df50a5b18f37e34625493362e05
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties pageTitle="教程︰ Azure Active Directory 集成与 Work.com |Microsoft Azure" description="了解如何使用 Work.com Azure Active Directory 以启用单一登录、 自动化资源调配，和更多。" services="active-directory" authors="MarkusVi"  documentationCenter="na" manager="stevenpo"/>
<tags ms.service="active-directory" ms.devlang="na" ms.topic="article" ms.tgt_pltfrm="na" ms.workload="identity" ms.date="08/01/2015" ms.author="markvi" />
#教程︰ 与 Work.com 的 Azure Active Directory 集成
  
>[AZURE.TIP] 反馈，请单击[此处](http://go.microsoft.com/fwlink/?LinkId=529836)。
  
本教程的目的是显示 Azure 和 Work.com 的集成。  
在本教程中介绍的方案假定您已经有以下各项︰

-   有效的 Azure 订购
-   Work.com-启用单一登录的订阅
  
在完成本教程之后, AAD 用户对其具有分配的 Work.com 不会访问能一号到 Work.com 公司网站 （服务提供程序初始化登录），或使用[简介访问权限面板](https://msdn.microsoft.com/library/dn308586)应用程序。
  
在本教程中介绍的方案由以下构造块组成︰

1.  启用应用程序集成为 Work.com
2.  配置单一登录
3.  配置用户设置
4.  分配用户

![方案](./media/active-directory-saas-work-com-tutorial/IC794105.png "Scenario")

##启用应用程序集成为 Work.com
  
本部分的目的是概述如何启用应用程序集成为 Work.com。

###若要启用应用程序集成为 Work.com，请执行以下步骤︰

1.  在 Azure 管理门户中，在左侧的导航窗格中，单击**活动目录**。

    ![活动目录](./media/active-directory-saas-work-com-tutorial/IC700993.png "Active Directory")

2.  从**目录**列表中，选择要为其启用目录集成的目录。

3.  若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序](./media/active-directory-saas-work-com-tutorial/IC700994.png "Applications")

4.  单击页面底部的**添加**。

    ![添加应用程序](./media/active-directory-saas-work-com-tutorial/IC749321.png "Add application")

5.  在**您想要执行**对话框中，单击**添加应用程序从库**。

    ![从 gallerry 中添加应用程序](./media/active-directory-saas-work-com-tutorial/IC749322.png "Add an application from gallerry")

6.  在**搜索框**中，键入**Work.com**。

    ![应用程序库](./media/active-directory-saas-work-com-tutorial/IC794106.png "Application Gallery")

7.  在结果窗格中，选择**Work.com**，，然后单击**完成**添加应用程序。

    ![Work.com](./media/active-directory-saas-work-com-tutorial/IC794107.png "Work.com")

##配置单一登录
  
本部分的目的是概述如何使用户能够使用基于 SAML 协议联合 Azure AD 中使用他们的帐户验证到 Work.com。  
作为此过程的一部分，您需要将证书传到 Work.com.com。

>[AZURE.NOTE] 若要配置单一登录，您需要尚未安装的自定义的 Work.com 域名。 您需要定义至少一个域名，测试您的域名，并将其部署到整个组织。

###若要配置单一登录，请执行以下步骤︰

1.  登录到管理员为您 Work.com 租户。

2.  转到**安装程序**。

    ![安装](./media/active-directory-saas-work-com-tutorial/IC794108.png "Setup")

3.  在左侧的导航窗格中，在**管理**部分中，单击**域管理**，以便展开相关的部分，，然后单击**我的域**打开**我的域**页。 

    ![我的域](./media/active-directory-saas-work-com-tutorial/IC767825.png "My Domain")

4.  要确认您的域已设置正确，请确保它是在"**步骤 4 将部署到用户**"并查看您的"**我的域设置**"。

    ![部署到用户的域](./media/active-directory-saas-work-com-tutorial/IC784377.png "Doman Deployed to User")

5.  在不同的 web 浏览器窗口中，登录到 Azure 的门户。

6.  在**Work.com**应用程序集成页上，单击**配置单一登录**来打开**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-work-com-tutorial/IC794109.png "Configure Single Sign-On")

7.  在**您想怎样来登录到 Work.com 的用户**页上**Microsoft Azure AD 单一登录**，选择，然后单击**下一步**。

    ![配置单一登录](./media/active-directory-saas-work-com-tutorial/IC794110.png "Configure Single Sign-On")

8.  在**配置应用程序 URL**页面上的**Work.com 号上的 URL**文本框中，键入用户用于登录到您的 Work.com 应用程序的 URL (例如:" *http://company.my.salesforce.com*")，然后单击**下一步**︰ 

    ![配置应用程序的 URL](./media/active-directory-saas-work-com-tutorial/IC794111.png "Configure App URL")

9.  在**配置单一登录 Work.com 在**页上，下载您的证书，单击**下载证书**，然后将保存在您的计算机的本地证书文件。

    ![配置单一登录](./media/active-directory-saas-work-com-tutorial/IC794112.png "Configure Single Sign-On")

10. 登录到 Work.com 租户。

11. 转到**安装程序**。

    ![安装](./media/active-directory-saas-work-com-tutorial/IC794108.png "Setup")

12. 展开**安全控件**菜单，然后单击**单一登录设置**。

    ![单一登录设置](./media/active-directory-saas-work-com-tutorial/IC794113.png "Single Sign-On Settings")

13. 在**单一登录设置**对话框页面上，请执行以下步骤︰

    ![SAML 启用](./media/active-directory-saas-work-com-tutorial/IC781026.png "SAML Enabled")

    1.  选择**启用 SAML**。
    2.  单击**新建**。

14. 在**SAML 单一登录设置**部分中，执行以下步骤︰

    ![SAML 单一登录设置](./media/active-directory-saas-work-com-tutorial/IC794114.png "SAML Single Sign-On Setting")

    1.  在**名称**文本框中，键入您的配置的名称。  

        >[AZURE.NOTE] 提供的值**名称**使用**API 名称**文本框自动填充。

    2.  在 Azure 的门户中，在**配置单一登录在 Work.com**对话框页上，将**颁发者 URL**值，复制，然后将其粘贴到**颁发者**文本框。
    3.  若要上载下载的证书，请单击**浏览**。
    4.  在**实体 Id**文本框中，键入**https://salesforce-work.com**。
    5.  作为**SAML 标识类型**，选择**断言包含从用户对象的联合身份验证 ID**。
    6.  作为**SAML 标识位置**，选择**标识是主题语句的 NameIdentfier 元素中**。
    7.  在 Azure 的门户中，在**配置单一登录在 Work.com**对话框页复制的**远程登录 URL**值，，然后将其粘贴到**标识提供程序的登录 URL**文本框。
    8.  在 Azure 的门户中，在**配置单一登录在 Work.com**对话框页复制**远程注销 URL**值，，然后将其粘贴到**标识提供程序注销 URL**文本框。
    9.  作为**服务提供程序初始化请求绑定**，选择**HTTP Post**。
    10. 单击**保存**。

15. 在 Work.com 门户网站，在左侧的导航窗格中，单击**域管理**，以便展开相关的部分，，然后单击**我的域**打开**我的域**页。 

    ![我的域](./media/active-directory-saas-work-com-tutorial/IC794115.png "My Domain")

16. 在**我的域**页面上，在**登录页署名**部分中，单击**编辑**。

    ![品牌的登录页](./media/active-directory-saas-work-com-tutorial/IC767826.png "Login Page Branding")

17. 在**登录页署名**页上，在**身份验证服务**部分中，将显示**SAML SSO 设置**的名称。 选择它，并单击**保存**。

    ![品牌的登录页](./media/active-directory-saas-work-com-tutorial/IC784366.png "Login Page Branding")

18. 在 Azure 广告门户，选择单一登录配置进行确认，然后单击**完成**关闭**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-work-com-tutorial/IC794116.png "Configure Single Sign-On")

##配置用户设置
  
对于 Azure Active Directory 用户能够登录，他们必须到 Work.com 设置。  
对于 Work.com，资源调配是手动任务。

###若要配置用户设置，请执行以下步骤︰

1.  以管理员身份登录到您的 Work.com 公司网站。

2.  转到**安装程序**。

    ![安装](./media/active-directory-saas-work-com-tutorial/IC794108.png "Setup")

3.  转到**管理用户\>用户**。

    ![管理用户](./media/active-directory-saas-work-com-tutorial/IC784369.png "Manage Users")

4.  单击**新用户**。

    ![所有用户](./media/active-directory-saas-work-com-tutorial/IC794117.png "All Users")

5.  在编辑用户部分中，执行以下步骤︰

    ![用户编辑](./media/active-directory-saas-work-com-tutorial/IC794118.png "User Edit")

    1.  键入**最后一个名称**、**别名**、**电子邮件**、**用户名**和**昵称**要到相关文本框设置有效 Azure Active Directory 帐户属性。
    2.  选择**角色**、**用户许可证**和**配置文件**。
    3.  单击**保存**。  

        >[AZURE.NOTE] Azure Active Directory 帐户持有人将获得包括确认该帐户之前将变为活动状态的链接的电子邮件。

>[AZURE.NOTE] 您可以使用任何其他 Work.com 用户帐户创建工具或 Api 来设置 AAD 用户帐户由 Work.com 提供。

##分配用户
  
若要测试您的配置，您需要将 Azure 的 AD 用户授予您要允许使用通过将他们分配到您应用程序的访问权限。

###要将用户分配到 Work.com 中，执行以下步骤︰

1.  在 Azure 广告门户中，创建一个测试帐户。

2.  在 Work.com 应用程序集成页上，单击**将用户分配**。

    ![分配用户](./media/active-directory-saas-work-com-tutorial/IC794119.png "Assign Users")

3.  选择测试用户，单击**分配**，然后单击**是**以确认您的分配。

    ![是](./media/active-directory-saas-work-com-tutorial/IC767830.png "Yes")
  
您现在应等待 10 分钟，并验证该帐户已被同步到 Work.com.com。
  
如果您想要测试单个登录设置，打开后盖。 关于访问面板的详细信息，请参阅[访问权限面板简介](https://msdn.microsoft.com/library/dn308586)。
测试
