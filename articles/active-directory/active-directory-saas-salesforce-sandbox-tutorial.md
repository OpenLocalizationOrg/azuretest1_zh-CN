---
ms.openlocfilehash: 216ebf1adbf92f78d7ee995d7204ca89d96b11d5
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties pageTitle="教程︰ 与销售队伍沙箱 Azure Active Directory 集成 |Microsoft Azure" description="了解如何使用 Azure Active Directory 队伍沙盒启用单一登录、 自动化资源调配，和更多。" services="active-directory" authors="MarkusVi"  documentationCenter="na" manager="stevenpo"/>
<tags ms.service="active-directory" ms.devlang="na" ms.topic="article" ms.tgt_pltfrm="na" ms.workload="identity" ms.date="08/01/2015" ms.author="markvi" />
#与销售队伍沙箱的教程︰ Azure Active Directory 集成
>[AZURE.TIP]反馈，请单击[此处](http://go.microsoft.com/fwlink/?LinkId=521878)。
  
本教程的目的是显示 Azure 和队伍沙箱的集成。  
沙箱可以用于多种用途，如开发、 测试和培训，而不会影响数据和应用程序销售生产组织中的单独环境中创建您的组织的多个副本的能力。  
有关详细信息，请参阅[沙盒概述](https://help.salesforce.com/HTViewHelpDoc?id=create_test_instance.htm&language=en_US)
  
在本教程中介绍的方案假定您已经有以下各项︰

-   有效的 Azure 订购
-   在 Salesforce.com 沙盒
  
如果您没有有效的沙盒中 Salesforce.com 还，您需要联系销售队伍。
  
在本教程中介绍的方案由以下构造块组成︰

1.  启用用于队伍沙盒应用程序集成
2.  配置单一登录
3.  使您的域
4.  配置用户设置
5.  分配用户

![方案](./media/active-directory-saas-salesforce-sandbox-tutorial/IC769571.png "Scenario")
##启用用于队伍沙盒应用程序集成
  
本部分的目的是概述如何启用的队伍沙盒应用程序集成。

###若要启用对队伍沙盒应用程序集成，请执行以下步骤︰

1.  在 Azure 管理门户中，在左侧的导航窗格中，单击**活动目录**。

    ![活动目录](./media/active-directory-saas-salesforce-sandbox-tutorial/IC700993.png "Active Directory")

2.  从**目录**列表中，选择要为其启用目录集成的目录。

3.  若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序](./media/active-directory-saas-salesforce-sandbox-tutorial/IC700994.png "Applications")

4.  若要打开**应用程序库**，**添加应用程序**，请单击，然后单击**添加我的组织使用的应用程序**。

    ![您要做什么？](./media/active-directory-saas-salesforce-sandbox-tutorial/IC700995.png "What do you want to do?")

5.  在**搜索框**中，键入**销售队伍沙盒**。

    ![应用程序库](./media/active-directory-saas-salesforce-sandbox-tutorial/IC710978.png "Application Gallery")

6.  在结果窗格中，选择**队伍沙盒**，，然后单击**完成**添加应用程序。

    ![销售队伍沙盒](./media/active-directory-saas-salesforce-sandbox-tutorial/IC746474.png "Salesforce Sandbox")
##配置单一登录
  
本部分的目的是概述如何使用户能够使用基于 SAML 协议联合 Azure AD 中使用他们的帐户验证到队伍。

###若要配置单一登录，请执行以下步骤︰

1.  在 Azure 广告门户中，**队伍沙盒**应用程序集成在页上，单击**配置单一登录**来打开**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-salesforce-sandbox-tutorial/IC749323.png "Configure single sign-on")

2.  在**如何您希望用户能够登录到队伍沙盒**页中**Microsoft Azure AD 单一登录**，选择，然后单击**下一步**。

    ![销售队伍沙盒](./media/active-directory-saas-salesforce-sandbox-tutorial/IC746479.png "Salesforce Sandbox")

3.  在**配置应用程序 URL**页上，在**销售队伍沙盒号在 URL**文本框中，键入您的 URL 使用以下模式"http://company.my.salesforce.com"，然后单击**下一步**。

    ![配置应用程序的 URL](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781022.png "Configure App URL")

4.  在**配置单一登录在队伍沙盒**页上，单击**下载证书**，然后将保存在您的计算机上的证书文件。

    ![配置单一登录](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781023.png "Configure Single Sign-On")

5.  在不同的 web 浏览器窗口中，以管理员身份登录到队伍沙盒。

6.  在菜单上，单击**设置**。

    ![安装](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781024.png "Setup")

7.  在左侧导航窗格中，单击**安全控制**，然后单击**单一登录设置**。

    ![单一登录设置](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781025.png "Single Sign-On Settings")

8.  在单一登录设置部分中，执行以下步骤︰

    ![单一登录设置](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781026.png "Single Sign-On Settings")

    1.  选择**启用 SAML**。
    2.  单击**新建**。

9.  在 SAML 单一登录设置部分中，执行以下步骤︰

    ![SAML 单一登录设置](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781027.png "SAML Single Sign-On Settings")

    1.  在名称文本框中，键入配置的名称 (例如︰ *SPSSOWAAD\_测试*)。
    2.  在 Azure 的门户中，在**配置单一登录在队伍沙箱**的对话页上，将**颁发者 URL**值，复制，然后将其粘贴到**颁发者**文本框。
    3.  在**实体 id**文本框中，键入**https://test.salesforce.com**。
    4.  单击**浏览**上载下载的证书。
    5.  作为**SAML 标识类型**，选择**断言包含从用户对象的联合身份验证 ID**。
    6.  作为**SAML 标识位置**，选择**标识是主题语句的 NameIdentifier 元素中**。
    7.  在 Azure 的门户中，在**配置单一登录在队伍沙箱**的对话页上的**远程登录 URL**值，复制，然后将其粘贴到**标识提供程序的登录 URL**文本框。
    8.  在 Azure 的门户中，在**配置单一登录在队伍沙箱**的对话页上，**远程注销 URL**值，复制，然后将其粘贴到**标识提供程序注销 URL**文本框。
    9.  作为**服务提供程序初始化请求绑定**，选择**HTTP POST**。
    10. 单击**保存**。

10. 在 Azure 广告门户，选择单一登录配置进行确认，然后单击**完成**关闭**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781028.png "Configure Single Sign-On")
##使您的域
  
本节假定您已经创建了一个域。  
有关详细信息，请参阅[定义的域名称](https://help.salesforce.com/HTViewHelpDoc?id=domain_name_define.htm&language=en_US)。

###若要启用您的域，请执行以下步骤︰

1.  在左侧的导航窗格中，单击**域管理**，然后单击**我域。**

    ![我的域](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781029.png "My Domain")

    >[AZURE.NOTE]请确保您的域已正确配置。

2.  在**登录页设置**部分中，单击**编辑**、 作为**身份验证服务**，从上一节中，选择 SAML 单一登录设置的名称然后，最后单击**保存**。

    ![我的域](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781030.png "My Domain")
  
只要您有配置域时，用户应使用登录到队伍沙箱的域 URL。  
若要获取 URL 的值，请单击在上一节创建的 SSO 配置文件。
##配置用户设置
  
本部分的目的是概述如何启用用户供应队伍沙盒活动目录的用户帐户。

###若要配置用户设置，请执行以下步骤︰

1.  在队伍门户中，在顶部导航栏中，选择您的姓名以展开用户菜单︰

    ![我的设置](./media/active-directory-saas-salesforce-sandbox-tutorial/IC698773.png "My Settings")

2.  从用户菜单，选择**我的设置**以打开**我的设置**页面。

3.  在左窗格中，单击**个人**以展开该个人的部分，然后单击**重置我安全令牌**:

    ![我的设置](./media/active-directory-saas-salesforce-sandbox-tutorial/IC698774.png "My Settings")

4.  在**重置我安全令牌**页上，单击**重置安全令牌**请求的电子邮件包含您 Salesforce.com 的安全令牌。

    ![新标记](./media/active-directory-saas-salesforce-sandbox-tutorial/IC698776.png "New Token")

5.  检查您的电子邮件收件箱 Salesforce.com 发的电子邮件，与"**salesforce.com.com 安全确认**"作为主题。

6.  查看此电子邮件并将复制的安全令牌值。

7.  在 Azure 管理门户中，**队伍沙盒**应用程序集成在页上，单击**配置用户设置**以打开**配置用户设置**对话框。

    ![配置用户资源调配](./media/active-directory-saas-salesforce-sandbox-tutorial/IC769573.png "Configure user provisioning")

8.  在**输入队伍沙盒启用自动用户提供的凭据**页中，提供以下配置设置︰

    ![销售队伍沙盒](./media/active-directory-saas-salesforce-sandbox-tutorial/IC746476.png "Salesforce Sandbox")

    1.  在**队伍沙盒管理员用户名**文本框中，键入具有**系统管理员**配置文件中分配的 Salesforce.com 的队伍沙盒帐户名称。

    2.  在**队伍沙盒管理员密码**文本框中，键入此帐户的密码。

    3.  在**用户安全令牌**的文本框中，粘贴的安全令牌值。

    4.  单击**验证**来验证您的配置。

    5.  单击**下一步**按钮以打开**确认**页。

9.  在**确认**页中，单击**完成**以保存配置。
##分配用户
  
若要测试您的配置，您需要将 Azure 的 AD 用户授予您要允许使用通过将他们分配到您应用程序的访问权限。

###若要将用户分配给队伍沙箱中，请执行以下步骤︰

1.  在 Azure 广告门户中，创建一个测试帐户。

2.  在**队伍沙盒**应用程序集成页上，单击**将用户分配**。

    ![分配用户](./media/active-directory-saas-salesforce-sandbox-tutorial/IC769574.png "Assign users")

3.  选择测试用户，单击**分配**，然后单击**是**以确认您的分配。

    ![是](./media/active-directory-saas-salesforce-sandbox-tutorial/IC767830.png "Yes")
  
现在应该等待 10 分钟，并验证该帐户已被同步到队伍沙盒。
  
如果您想要测试单个登录设置，打开后盖。 关于访问面板的详细信息，请参阅[访问权限面板简介](https://msdn.microsoft.com/library/dn308586)。
测试
