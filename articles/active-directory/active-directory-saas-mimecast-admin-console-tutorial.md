---
ms.openlocfilehash: 21f7130fb012fb87896405ec26b447dd3ad2ee22
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties pageTitle="教程︰ 使用 Mimecast 管理控制台的 Azure Active Directory 集成 |Microsoft Azure" description="了解如何使用 Azure Active Directory Mimecast 管理控制台启用单一登录、 自动化资源调配，和更多。" services="active-directory" authors="MarkusVi"  documentationCenter="na" manager="stevenpo"/>
<tags ms.service="active-directory" ms.devlang="na" ms.topic="article" ms.tgt_pltfrm="na" ms.workload="identity" ms.date="08/01/2015" ms.author="markvi" />
#教程︰ 使用 Mimecast 管理控制台的 Azure Active Directory 集成
>[AZURE.TIP]反馈，请单击[此处](http://go.microsoft.com/fwlink/?LinkId=529833)。
  
本教程的目的是显示 Azure 和 Mimecast 管理控制台集成。  
在本教程中介绍的方案假定您已经有以下各项︰

-   有效的 Azure 订购
-   Mimecast 管理控制台-启用单一登录的订阅
  
后学完本教程，您已分配给 Mimecast 管理控制台的 Azure AD 用户将能够到单一登录到管理控制台 Mimecast 公司网站 （服务提供程序初始化登录），或使用[简介访问权限面板](https://msdn.microsoft.com/library/dn308586)应用程序
  
在本教程中介绍的方案由以下构造块组成︰

1.  启用对 Mimecast 管理控制台的应用程序集成
2.  配置单一登录
3.  配置用户设置
4.  分配用户

![方案](./media/active-directory-saas-mimecast-admin-console-tutorial/IC795008.png "Scenario")
##启用对 Mimecast 管理控制台的应用程序集成
  
本部分的目的是概述如何启用 Mimecast 管理控制台应用程序的集成。

###若要启用对 Mimecast 管理控制台的应用程序集成，请执行以下步骤︰

1.  在 Azure 管理门户中，在左侧的导航窗格中，单击**活动目录**。

    ![活动目录](./media/active-directory-saas-mimecast-admin-console-tutorial/IC700993.png "Active Directory")

2.  从**目录**列表中，选择要为其启用目录集成的目录。

3.  若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序](./media/active-directory-saas-mimecast-admin-console-tutorial/IC700994.png "Applications")

4.  单击页面底部的**添加**。

    ![添加应用程序](./media/active-directory-saas-mimecast-admin-console-tutorial/IC749321.png "Add application")

5.  在**您想要执行**对话框中，单击**添加应用程序从库**。

    ![从 gallerry 中添加应用程序](./media/active-directory-saas-mimecast-admin-console-tutorial/IC749322.png "Add an application from gallerry")

6.  在**搜索框**中，键入**Mimecast 管理控制台**。

    ![应用程序库](./media/active-directory-saas-mimecast-admin-console-tutorial/IC795009.png "Application Gallery")

7.  在结果窗格中， **Mimecast 管理控制台**中，选择，然后单击**完成**添加应用程序。

    ![Mimecast 管理控制台](./media/active-directory-saas-mimecast-admin-console-tutorial/IC795010.png "Mimecast Admin Console")
##配置单一登录
  
本部分的目的是概述如何使用户能够使用基于 SAML 协议联合 Azure AD 中使用他们的帐户验证到 Mimecast 管理控制台。  
作为此过程的一部分，您将需要创建一个 base 64 编码的证书文件。  
如果您不熟悉此过程，请参阅[如何将转换到一个文本文件的二进制证书](http://youtu.be/PlgrzUZ-Y1o)。

###若要配置单一登录，请执行以下步骤︰

1.  在 Azure 广告门户中， **Mimecast 管理控制台**应用程序集成在页上，单击**配置单一登录**来打开**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-mimecast-admin-console-tutorial/IC795011.png "Configure Single Sign-On")

2.  在**您想怎样来登录到 Mimecast 管理控制台的用户**页上**Microsoft Azure AD 单一登录**，选择，然后单击**下一步**。

    ![配置单一登录](./media/active-directory-saas-mimecast-admin-console-tutorial/IC795012.png "Configure Single Sign-On")

3.  在**配置应用程序 URL**页面上的**Mimecast 管理控制台登录在 URL**文本框中，键入用户用于登录到 Mimecast 管理控制台应用程序的 URL (例如:"https://webmail-uk.mimecast.com"或"https://webmail-us.mimecast.com")，然后单击**下一步**。

    >[AZURE.NOTE] 在 URL 上的符号是特定区域。

    ![配置应用程序的 URL](./media/active-directory-saas-mimecast-admin-console-tutorial/IC795013.png "Configure App URL")

4.  在**配置单一登录 Mimecast 管理控制台在**页上，下载您的证书，单击**下载证书**，然后将保存在您的计算机的本地证书文件。

    ![配置单一登录](./media/active-directory-saas-mimecast-admin-console-tutorial/IC795014.png "Configure Single Sign-On")

5.  在不同的 web 浏览器窗口中，以管理员身份登录到 Mimecast 管理控制台。

6.  转到**服务\>应用程序**。

    ![服务](./media/active-directory-saas-mimecast-admin-console-tutorial/IC794998.png "Services")

7.  单击**身份验证配置文件**。

    ![身份验证配置文件](./media/active-directory-saas-mimecast-admin-console-tutorial/IC794999.png "Authentication Profiles")

8.  单击**新的身份验证配置文件**。

    ![新的身份验证配置文件](./media/active-directory-saas-mimecast-admin-console-tutorial/IC795000.png "New Authentication Profiles")

9.  在**身份验证的配置文件**部分中，执行以下步骤︰

    ![身份验证配置文件](./media/active-directory-saas-mimecast-admin-console-tutorial/IC795015.png "Authentication Profile")

    1.  在**说明**文本框中，键入您的配置的名称。
    2.  选择**强制执行 Mimecast 管理控制台的 SAML 验证**。
    3.  作为**提供程序**，选择**Azure 活动目录**。
    4.  在 Azure 的门户中，在**配置单一登录 Mimecast 管理控制台**对话框页上，将**颁发者 URL**值，复制，然后将其粘贴到该**颁发者 URL**文本框。
    5.  在 Azure 的门户中，在**配置单一登录 Mimecast 管理控制台**对话框页面上的**远程登录 URL**值，复制，然后将其粘贴到**登录 URL**文本框。
    6.  在 Azure 的门户中，在**配置单一登录 Mimecast 管理控制台**对话框页面上，复制的**远程登录 URL**值，，然后将其粘贴到**注销 URL**文本框。  

        >[AZURE.NOTE]登录 URL 值和注销 URL 值是 Mimecast 管理控制台相同。

    7.  创建您已下载的证书的**base 64 编码**的文件。  

        >[AZURE.TIP]有关详细信息，请参阅[如何将转换到一个文本文件的二进制证书](http://youtu.be/PlgrzUZ-Y1o)。

    8.  打开记事本中的 base 64 编码的证书删除的第一行 ("*--*") 和最后一个行 ("*--*")，将其余的内容复制到剪贴板，然后将其粘贴到**标识提供程序证书 （元数据）**文本框。
    9.  选择**允许单一登录**。
    10. 单击**保存**。

10. 在 Azure 广告门户，选择单一登录配置进行确认，然后单击**完成**关闭**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-mimecast-admin-console-tutorial/IC795016.png "Configure Single Sign-On")
##配置用户设置
  
为了使 Azure AD 用户能够登录到 Mimecast 管理控制台，必须到 Mimecast 管理控制台配置它们。  
Mimecast 管理控制台中，在资源调配是手动任务。
  
您需要注册域，然后才能创建用户。

###若要配置用户设置，请执行以下步骤︰

1.  以管理员身份登录到您的**Mimecast 管理控制台**。

2.  转到**目录\>内部**。

    ![目录](./media/active-directory-saas-mimecast-admin-console-tutorial/IC795003.png "Directories")

3.  单击**注册新域**。

    ![注册新域](./media/active-directory-saas-mimecast-admin-console-tutorial/IC795004.png "Register New Domain")

4.  在创建新域之后，请单击**新的地址**。

    ![新的地址](./media/active-directory-saas-mimecast-admin-console-tutorial/IC795005.png "New Address")

5.  在新的地址对话框中，请执行以下步骤︰

    ![保存](./media/active-directory-saas-mimecast-admin-console-tutorial/IC795006.png "Save")

    1.  键入您要提供到相关文本框有效 AAD 帐户的**电子邮件地址**、**全局名称**、**密码**和**确认密码**属性。
    2.  单击**保存**。

>[AZURE.NOTE]您可以使用 Mimecast 管理控制台用户帐户创建工具或 Api 由 Mimecast 管理控制台来设置 AAD 用户帐户。

##分配用户
  
若要测试您的配置，您需要将 Azure 的 AD 用户授予您要允许使用通过将他们分配到您应用程序的访问权限。

###若要将用户分配到 Mimecast 管理控制台，请执行以下步骤︰

1.  在 Azure 广告门户中，创建一个测试帐户。

2.  在**Mimecast 管理控制台**应用程序集成页上，单击**将用户分配**。

    ![分配用户](./media/active-directory-saas-mimecast-admin-console-tutorial/IC795017.png "Assign Users")

3.  选择测试用户，单击**分配**，然后单击**是**以确认您的分配。

    ![是](./media/active-directory-saas-mimecast-admin-console-tutorial/IC767830.png "Yes")
  
如果您想要测试单个登录设置，打开后盖。 关于访问面板的详细信息，请参阅[访问权限面板简介](https://msdn.microsoft.com/library/dn308586)。
测试
