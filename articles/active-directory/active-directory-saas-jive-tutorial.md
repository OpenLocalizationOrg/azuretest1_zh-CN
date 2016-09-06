---
ms.openlocfilehash: 69146fb7969b86ab6c5c0ad0e193c5c04864d139
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties pageTitle="教程︰ Azure Active Directory 集成与 Jive |Microsoft Azure" description="了解如何使用 Azure Active Directory Jive 启用单一登录、 自动化资源调配，和更多。" services="active-directory" authors="MarkusVi"  documentationCenter="na" manager="stevenpo"/>
<tags ms.service="active-directory" ms.devlang="na" ms.topic="article" ms.tgt_pltfrm="na" ms.workload="identity" ms.date="08/01/2015" ms.author="markvi" />
#教程︰ 与 Jive 的 Azure Active Directory 集成
>[AZURE.TIP]反馈，请单击[此处](http://go.microsoft.com/fwlink/?LinkId=330042)。  
  有关此主题的详细信息，请参阅[管理 Azure Active Directory 应用程序访问增强的最佳做法](http://go.microsoft.com/fwlink/?LinkId=329963)。
  
本教程的目的是显示 Azure 和 Jive 的集成。  
在本教程中介绍的方案假定您已经有以下各项︰

-   有效的 Azure 订购
-   在 Jive 租户
  
在本教程中介绍的方案由以下构造块组成︰

1.  启用应用程序集成的 Jive
2.  配置用户设置

##启用应用程序集成的 Jive
  
本部分的目的是概述如何启用应用程序集成的 Jive。

###若要启用应用程序集成的 Jive，请执行以下步骤︰

1.  在 Azure 管理门户中，在左侧的导航窗格中，单击**活动目录**。

    ![活动目录](./media/active-directory-saas-jive-tutorial/IC700993.png "Active Directory")

2.  从**目录**列表中，选择要为其启用目录集成的目录。

3.  若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序](./media/active-directory-saas-jive-tutorial/IC700994.png "Applications")

4.  若要打开**应用程序库**，**添加应用程序**，请单击，然后单击**添加我的组织使用的应用程序**。

    ![您要做什么？](./media/active-directory-saas-jive-tutorial/IC700995.png "What do you want to do?")

5.  在**搜索框**中，键入**Jive**。

    ![Jive](./media/active-directory-saas-jive-tutorial/IC701001.png "Jive")

6.  在结果窗格中，选择**Jive**，，然后单击**完成**添加应用程序。

    ![Jive](./media/active-directory-saas-jive-tutorial/IC701005.png "Jive")
##配置用户设置
  
本部分的目的是概述如何启用用户供应 Jive 到活动目录的用户帐户。  
作为此过程的一部分，您将需要提供将用户安全令牌，您需要从 Jive.com 请求。
  
下面的屏幕快照显示在 Azure AD 相关对话框的示例︰

![配置用户资源调配](./media/active-directory-saas-jive-tutorial/IC698794.png "Configure User Provisioning")

###若要配置用户设置，请执行以下步骤︰

1.  在 Azure 管理门户中， **Jive**应用程序集成在页上，单击**配置用户设置**以打开**配置用户设置**对话框。

2.  在**输入 Jive 启用自动用户提供的凭据**页提供以下配置设置︰

    1.  在**Jive 管理员用户名**文本框中，键入具有**系统管理员**配置文件中指定的 Jive.com Jive 帐户名。

    2.  在**Jive 管理员密码**文本框中，键入此帐户的密码。

    3.  在**Jive 租户 URL**文本框中，键入 Jive 租户 URL。

        >[AZURE.NOTE] Jive 租户 URL 为您的组织用于登录到 Jive 的 URL。  
        通常情况下，URL 具有以下格式︰ **www。\<组织\>。 jive.com**。

    4.  单击**验证**来验证您的配置。

    5.  单击**下一步**按钮以打开**确认**页。

3.  在**确认**页上，单击以保存配置的复选标记。
  
您现在可以创建一个测试帐户、 等待 10 分钟并验证帐户已被同步到 Jive.com。


测试
