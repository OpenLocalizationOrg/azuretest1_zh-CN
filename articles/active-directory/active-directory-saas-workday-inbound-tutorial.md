---
ms.openlocfilehash: a8d5c11bb6d16a6b06d1a640e07b8af328613d6f
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
  pageTitle="教程︰ 配置入站同步工作日 |Microsoft Azure" 
  description="了解如何使用 Azure Active Directory 作为标识数据源的工作日。" 
  services="active-directory" 
  authors="MarkusVi"  
  documentationCenter="na" 
  manager="msStevenPo"/>
<tags 
  ms.service="active-directory" 
  ms.devlang="na" 
  ms.topic="article" 
  ms.tgt_pltfrm="na" 
  ms.workload="identity" 
  ms.date="08/01/2015" 
  ms.author="markvi" />


#教程︰ 配置入站同步的工作日


>[AZURE.TIP]反馈，请单击[此处](http://go.microsoft.com/fwlink/?LinkId=521880)。
  
本教程的目的是向您显示您需要在工作日和 Azure 广告从工作日的人导入到 Azure 的广告执行的步骤。 

在本教程中介绍的方案假定您已经有以下各项︰

-   有效的 Azure AD 最优订购
-   在工作日中一个租户
  
在本教程中介绍的方案由以下构造块组成︰

1. 启用应用程序集成的工作日 


2. 创建一个集成的系统用户 


3. 创建安全组 


4. 将集成系统用户分配给安全组 


5. 配置安全组选项 


6. 激活安全策略更改 


7. 在 Azure 广告中配置的用户导入 



##启用应用程序集成的工作日
  
本部分的目的是概述如何启用应用程序集成为工作日。

###若要启用应用程序集成为工作日，请执行以下步骤︰

1.  在 Azure 管理门户中，在左侧的导航窗格中，单击**活动目录**。

    ![活动目录](./media/active-directory-saas-workday-inbound-tutorial/IC700993.png "Active Directory")

2.  从**目录**列表中，选择要为其启用目录集成的目录。

3.  若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序](./media/active-directory-saas-workday-inbound-tutorial/IC700994.png "Applications")

4.  单击页面底部的**添加**。

    ![添加应用程序](./media/active-directory-saas-workday-inbound-tutorial/IC749321.png "Add application")

  
5. 在搜索框中，键入**工作日**。

    ![从 gallerry 中添加应用程序](./media/active-directory-saas-workday-inbound-tutorial/IC701021.png "Add an application from gallerry")

6. 在结果窗格中，选择工作日，然后单击完成添加应用程序。

    ![应用程序库](./media/active-directory-saas-workday-inbound-tutorial/IC701022.png "Application gallery")





## 创建一个集成的系统用户

1. 在**工作日工作台**中，输入在搜索框中创建用户，然后单击**创建集成系统用户**。 <br><br>   ![创建用户](./media/active-directory-saas-workday-inbound-tutorial/IC750979.png "Create user")



2. 完成**创建集成系统用户**任务的新的集成系统用户提供用户名和密码。  取消选中要求新密码在下一次登录选项，因为此用户将以编程方式在登录。 <br>
其默认值为 0，这将防止用户的会话超时过早离开会话超时分钟。 <br><br>   ![创建集成系统用户](./media/active-directory-saas-workday-inbound-tutorial/IC750980.png "Create Integration System User")
 




## 创建安全组

在本教程中所述的情况下，您需要创建一个不受约束的集成系统安全组并将用户分配给它。


1. 输入在搜索框中创建安全组，然后单击**创建安全组**。 <br><br>   ![CreateSecurity 组](./media/active-directory-saas-workday-inbound-tutorial/IC750981.png "CreateSecurity Group")
 

2. 完成创建安全组任务。  选择集成系统安全组 — — 不受约束的租用安全组类型下拉列表中，从创建该成员将被显式添加到安全组。 <br><br>   ![CreateSecurity 组](./media/active-directory-saas-workday-inbound-tutorial/IC750982.png "CreateSecurity Group")
 



## 将集成系统用户分配给安全组

1. 在搜索框中输入编辑安全组，然后单击**编辑安全组**。 <br><br>   ![编辑安全组](./media/active-directory-saas-workday-inbound-tutorial/IC750983.png "Edit Security Group")
 
 

2. 搜索并选择新的集成安全组的名称。 <br><br> ![编辑安全组](./media/active-directory-saas-workday-inbound-tutorial/IC750984.png "Edit Security Group")编辑安全组 

3. 新的集成系统用户添加到新的安全组。 <br><br>   ![系统安全组](./media/active-directory-saas-workday-inbound-tutorial/IC750985.png "System Security Group")  




## 配置安全组选项

在此步骤中，您将授予**获取**和**设置**由下列域安全策略保护的对象上的操作的新安全组权限︰

- 外部帐户设置

- 辅助数据︰ 公用工作报告

- 所有职位的工作人员的数据︰

- 工作人员的数据︰ 当前人员信息

- 在工作人员的个人资料的工作人员数据︰ 职务
 
   

1. 在搜索框中，输入域安全策略，然后单击链接域安全策略的功能区域。  <br><br>   ![域安全策略](./media/active-directory-saas-workday-inbound-tutorial/IC750986.png "Domain Security Policies")  
 

2. 搜索系统，并选择**系统**功能区域。  单击**确定**。  <br><br>   ![域安全策略](./media/active-directory-saas-workday-inbound-tutorial/IC750987.png "Domain Security Policies")  


3. 在系统功能区域的安全策略列表中，展开安全管理，然后选择外部帐户设置的域安全策略。  <br><br>   ![域安全策略](./media/active-directory-saas-workday-inbound-tutorial/IC750988.png "Domain Security Policies")  


4. 单击**编辑权限**，然后在**编辑权限**对话框页到具有集成的**获取**和**设置**权限的安全组的列表添加新的安全组。 <br><br>   ![编辑权限](./media/active-directory-saas-workday-inbound-tutorial/IC750989.png "Edit Permission")  

 

5. 重复第 1 步上面，以返回到屏幕上的选择功能区域中，和这一次，搜索人员、 选择人力资源功能区域，然后单击**确定**。<br><br>   ![域安全策略](./media/active-directory-saas-workday-inbound-tutorial/IC750990.png "Domain Security Policies")  
 

6. 在人力资源功能区域的安全策略列表中，展开工作人员数据︰ 人员，并重复上面的步骤 4 为每个剩余的安全策略︰

     - 辅助数据︰ 公用工作报告

     - 所有职位的工作人员的数据︰

     - 工作人员的数据︰ 当前人员信息

     - 在工作人员的个人资料的工作人员数据︰ 职务


<br><br>   ![域安全策略](./media/active-directory-saas-workday-inbound-tutorial/IC750991.png "Domain Security Policies")  







## 激活安全策略更改


1. 输入激活在搜索框中，然后单击链接，激活挂起的安全策略更改。 <br><br>   ![激活](./media/active-directory-saas-workday-inbound-tutorial/IC750992.png "Activate") 
 

2. 首先激活挂起的安全策略更改任务用于审计目的，输入注释，然后单击**确定**。 <br><br>   ![激活挂起的安全](./media/active-directory-saas-workday-inbound-tutorial/IC750993.png "Activate Pending Security")   
 

3. 通过选中复选框标记确认，完成下一个屏幕上的任务，然后单击**确定**。 <br><br>   ![激活挂起的安全](./media/active-directory-saas-workday-inbound-tutorial/IC750994.png "Activate Pending Security")  





## 在 Azure 广告中配置的用户导入

本部分的目的是概述如何配置 Azure AD 工作日从导入的人。


### 若要配置在 Azure AD 用户导入，请执行以下步骤︰


1. 在**工作日**应用程序集成页上，单击**配置用户导入**以打开**配置设置**对话框。


2.在**设置和管理凭据**页上，执行以下步骤，，然后单击**下一步**: <br><br>   ![设置和管理凭据](./media/active-directory-saas-workday-inbound-tutorial/IC750995.png "Settings and admin credentials")  
 
     2.1. In the Workday admin user name textbox, type the name of the user you have created in the Creating an integration system user section.

     2.2. In the Workday admin password textbox, type the password of the user you have created in the Creating an integration system user section.

     2.3. In the Workday tenant URL textbox, type the URL or your Workday tenant.


3. 在**测试连接**页上，单击**开始测试**以确认连接，，然后单击**下一步**。 <br><br>   ![测试连接](./media/active-directory-saas-workday-inbound-tutorial/IC750996.png "Test connection")  
 

4. 在**资源调配**选项页上，单击**下一步**。 <br><br>   ![自动配置选项](./media/active-directory-saas-workday-inbound-tutorial/IC750997.png "Provisioning options")


5. 在**开始调配**对话框中，单击**完成**。 <br><br>   ![开始设置](./media/active-directory-saas-workday-inbound-tutorial/IC750998.png "Start provisioning")
 

您现在可以转到**用户**部分，检查工作日用户是否已导入。



## 其他资源

* [如何将与 Azure Active Directory 集成在 SaaS 应用程序的教程列表](active-directory-saas-tutorial-list.md)
* [应用程序访问权限和单一登录使用 Azure 活动目录是什么？](active-directory-appssoaccess-whatis.md)

测试
