---
ms.openlocfilehash: f1aea59e88f0ee5d2f8a9aafddff78b65bf90d77
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties pageTitle="教程︰ Azure Active Directory 集成与 Zscaler |Microsoft Azure" description="了解如何使用 Zscaler Azure Active Directory 以启用单一登录、 自动化资源调配，和更多。" services="active-directory" authors="MarkusVi"  documentationCenter="na" manager="stevenpo"/>
<tags ms.service="active-directory" ms.devlang="na" ms.topic="article" ms.tgt_pltfrm="na" ms.workload="identity" ms.date="08/01/2015" ms.author="markvi" />
#教程︰ 与 Zscaler 的 Azure Active Directory 集成
>[AZURE.TIP]反馈，请单击[此处](http://go.microsoft.com/fwlink/?LinkId=521842)。
  
本教程的目的是显示 Azure 和 Zscaler 的集成。 在本教程中介绍的方案假定您已经有以下各项︰

-   有效的 Azure 订购
-   在 Zscaler 中承租人
  
在本教程中介绍的方案由以下构造块组成︰

1.  启用应用程序集成为 Zscaler
2.  配置单一登录
3.  配置代理服务器设置
4.  配置用户设置
5.  分配用户

![方案](./media/active-directory-saas-zscaler-tutorial/IC769226.png "Scenario")

##启用应用程序集成为 Zscaler
  
本部分的目的是概述如何启用应用程序集成为 Zscaler。

###若要启用应用程序集成为 Zscaler，请执行以下步骤︰

1.  在 Azure 管理门户中，在左侧的导航窗格中，单击**活动目录**。

    ![活动目录](./media/active-directory-saas-zscaler-tutorial/IC700993.png "Active Directory")

2.  从**目录**列表中，选择要为其启用目录集成的目录。

3.  若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序](./media/active-directory-saas-zscaler-tutorial/IC700994.png "Applications")

4.  单击页面底部的**添加**。

    ![添加应用程序](./media/active-directory-saas-zscaler-tutorial/IC749321.png "Add application")

5.  在**您想要执行**对话框中，单击**添加应用程序从库**。

    ![从 gallerry 中添加应用程序](./media/active-directory-saas-zscaler-tutorial/IC749322.png "Add an application from gallerry")

6.  在**搜索框**中，键入**Zscaler**。

    ![应用程序库](./media/active-directory-saas-zscaler-tutorial/IC769227.png "Application gallery")

7.  在结果窗格中，选择**Zscaler**，，然后单击**完成**添加应用程序。

    ![Zscaler](./media/active-directory-saas-zscaler-tutorial/IC769228.png "Zscaler")

##配置单一登录
  
本部分的目的是概述如何使用户能够使用基于 SAML 协议联合 Azure AD 中使用他们的帐户验证到 Zscaler。  
作为此过程的一部分，您需要将证书传到 Zscaler。

###若要配置单一登录，请执行以下步骤︰

1.  在 Azure 广告门户中， **Zscaler**应用程序集成在页上，单击**配置单一登录**来打开**配置单一登录**对话框。

    ![启用单一登录](./media/active-directory-saas-zscaler-tutorial/IC769229.png "Enable single sign-on")

2.  在**您想怎样来登录到 Zscaler 的用户**页上**Microsoft Azure AD 单一登录**，选择，然后单击**下一步**。

    ![在配置单一登录](./media/active-directory-saas-zscaler-tutorial/IC769230.png "Configure single sign on")

3.  在**配置应用程序 URL**页上，在**Zscaler 号中 URL**文本框中，键入登录 Zscaler，从你的 URL 中，然后单击**下一步**: 

    >[AZURE.NOTE] 请如果您不知道什么是您的登录 URL，与 Zscaler 的支持团队联系。

    ![配置应用程序的 URL](./media/active-directory-saas-zscaler-tutorial/IC769231.png "Configure app URL")

4.  在**配置单一登录 Zscaler 在**页上，请执行以下步骤︰

    ![配置单一登录](./media/active-directory-saas-zscaler-tutorial/IC769232.png "Configure single sign-on")

    1.  单击**下载证书**，然后将保存为本地证书文件**c:\\Zscaler.cer**。
    2.  将**身份验证请求的 URL**复制到剪贴板。

5.  登录到 Zscaler 租户。

6.  在菜单上，单击**管理**。

    ![管理](./media/active-directory-saas-zscaler-tutorial/IC769486.png "Administration")

7.  下**管理管理员和角色**，单击**管理用户和身份验证**。

    ![管理管理员和角色](./media/active-directory-saas-zscaler-tutorial/IC769487.png "Manage Administrators & Roles")

8.  在**选择您的组织的身份验证选项**部分中，执行以下步骤︰

    ![选择的身份验证选项](./media/active-directory-saas-zscaler-tutorial/IC769488.png "Choose Authentication Options")

    1.  选择**身份验证使用 SAML 单一登录方式进行登录**。
    2.  单击**配置 SAML 单一登录参数**。

9.  **配置 SAML 单一登录参数**对话框页上，执行下面的步骤，然后单击**完成**:

    ![将上载证书](./media/active-directory-saas-zscaler-tutorial/IC769489.png "Upload certificate")

    1.  在**进行身份验证的用户发送到 SAML 门户的 URL**文本框中，粘贴从 Azure 的门户网站的**身份验证请求 URL**字段的值。
    2.  在**属性包含登录名**文本框中，键入**NameID**。
    3.  在**上载 SSL 公用证书**字段中，传从 Azure 的门户网站下载该证书。
    4.  选择**启用 SAML 自动资源调配**。

10. 在**配置用户身份验证**对话框页面上，请执行以下步骤︰

    ![配置用户身份验证](./media/active-directory-saas-zscaler-tutorial/IC769490.png "Configure User Authentication")

    1.  单击**保存**。
    2.  单击**立即激活**。

11. 在 Azure 广告门户，选择单一登录配置进行确认，然后单击**完成**关闭**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-zscaler-tutorial/IC769491.png "Configure single sign-on")

##配置代理服务器设置

###要在 Internet Explorer 中配置代理服务器设置

1.  启动**Internet Explorer**。

2.  从**工具**菜单上打开**Internet 选项**对话框中选择**Internet 选项**。

    ![Internet 选项](./media/active-directory-saas-zscaler-tutorial/IC769492.png "Internet Options")

3.  单击**连接**选项卡。

    ![连接](./media/active-directory-saas-zscaler-tutorial/IC769493.png "Connections")

4.  单击**局域网设置**以打开**局域网设置**对话框。

5.  在代理服务器部分中，执行以下步骤︰

    ![代理服务器](./media/active-directory-saas-zscaler-tutorial/IC769494.png "Proxy server")

    1.  选择为 LAN 使用代理服务器。
    2.  在地址文本框中，键入**gateway.zscalertwo.net**。
    3.  在端口文本框中，键入**80**。
    4.  选择**对于本地地址不使用代理服务器**。
    5.  单击**确定**以关闭**局域网 (LAN) 设置**对话框。

6.  单击**确定**以关闭**Internet 选项**对话框。

##配置用户设置
  
为了使 Azure AD 用户能够登录到 Zscaler，他们必须到 Zscaler 配置。  
对于 Zscaler，资源调配是手动任务。

###若要配置用户设置，请执行以下步骤︰

1.  登录到**Zscaler**租户。

2.  单击**管理**。

    ![管理](./media/active-directory-saas-zscaler-tutorial/IC781035.png "Administration")

3.  单击**用户管理**。

    ![用户管理](./media/active-directory-saas-zscaler-tutorial/IC781036.png "User Management")

4.  在**用户**选项卡上，单击**添加**。

    ![Add](./media/active-directory-saas-zscaler-tutorial/IC781037.png "Add")

5.  在添加用户部分中，执行以下步骤︰

    ![添加用户](./media/active-directory-saas-zscaler-tutorial/IC781038.png "Add User")

    1.  键入**用户 Id**、**显示用户名**、**密码**、**确认密码**，然后选择**组**和您希望提供一个有效的 AAD 帐户**部门**。
    2.  单击**保存**。

>[AZURE.NOTE] 您可以使用任何其他 Zscaler 用户帐户创建的工具或 Api 来设置 AAD 用户帐户由 Zscaler 提供。

##分配用户
  
若要测试您的配置，您需要将 Azure 的 AD 用户授予您要允许使用通过将他们分配到您应用程序的访问权限。

###要将用户分配到 Zscaler 中，执行以下步骤︰

1.  在 Azure 广告门户中，创建一个测试帐户。

2.  在**Zscaler**应用程序集成页上，单击**将用户分配**。

    ![分配用户](./media/active-directory-saas-zscaler-tutorial/IC769495.png "Assign users")

3.  选择测试用户，单击**分配**，然后单击**是**以确认您的分配。

    ![是](./media/active-directory-saas-zscaler-tutorial/IC767830.png "Yes")
  
如果您想要测试单个登录设置，打开后盖。 关于访问面板的详细信息，请参阅[访问权限面板简介](https://msdn.microsoft.com/library/dn308586)。

测试
