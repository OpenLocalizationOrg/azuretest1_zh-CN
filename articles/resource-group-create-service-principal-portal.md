---
ms.openlocfilehash: a3bbca58e647026ecb8c64ac1d93b911274441a2
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="创建新的 Azure 服务主体使用 Azure 门户"
   description="描述如何创建新的 Azure 服务主体，可以使用基于角色的访问控制在 Azure 资源管理器来管理对资源的访问。"
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="wpickett"
   editor=""/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/24/2015"
   ms.author="tomfitz"/>

# 创建新的 Azure 服务主体使用 Azure 门户

## 概述
服务主体是自动化的过程，应用程序或访问其他资源所需要的服务。 使用 Azure 资源管理器，可以授予访问权限的服务主体，并以允许的管理操作执行上存在预订中或作为租户的资源进行身份验证。 

本主题演示如何创建新服务主体使用 Azure 的门户。 目前，您必须使用 Microsoft Azure 门户创建新的服务主体。 此功能将被添加到 Azure 预览门户，在以后的版本中。

## 概念
1. Azure Active Directory (AAD) 的云身份和访问管理服务生成。 有关更多详细信息，请参阅︰[什么是 Azure 的活动目录](./active-directory-whatis/)
2. 服务主体的某个目录中应用程序的实例。
3. AD 应用程序-中标识应用程序到 AAD 的 AAD 的目录记录。 有关更多详细信息，请参阅[Azure 广告基础知识的身份验证](https://msdn.microsoft.com/library/azure/874839d9-6de6-43aa-9a5c-613b0c93247e#BKMK_Auth)。


## 创建活动目录的应用程序
1. 登录到 Azure 帐户通过[经典的门户](https://manage.windowsazure.com/)。

2. 从左窗格中选择**活动目录**。

   ![请选择有效的目录][1]

3. 选择要用于创建新的应用程序的目录。

   ![选择目录][2]

3. 要查看应用程序在您的目录中，请单击**应用程序**。

   ![查看应用程序][11]

4. 如果尚未创建应用程序在该目录中之前，您应该看到类似于下面的图像。 单击**添加应用程序**

   ![添加应用程序][6]

   或者，单击底部窗格中的**添加**。

   ![添加][12]

5. 选择您想要创建的应用程序的类型。 在本教程中，我们不会从库中使用应用程序。

   ![新的应用程序][10]

6. 填充的应用程序名称并选择想要使用的应用程序的类型。 因为我们想要使用此应用程序的服务主体进行身份验证使用 Azure 资源管理器中，我们将选择创建一个**WEB 应用程序和/或 WEB API** ，然后单击下一步按钮。

   ![应用程序名称][9]

7. 填写您的应用程序的属性。 对于**符号上的 URL**，提供网站描述您的应用程序的 URI。 未验证的 web 站点存在。 对于**应用程序 ID URI**，可提供标识您的应用程序的 URI。 未经验证的唯一性或终结点的存在。 单击**完成**以创建您 AAD 的应用程序。

   ![应用程序属性][4]

## 创建服务主体密码
门户网站现在应该有选择的应用程序。

1. 单击**配置**选项卡来配置应用程序的密码。

   ![配置应用程序][3]

2. 向下滚动到**密钥**部分中，选择您的密码才能有效多长时间。

   ![密钥][7]

3. 选择**保存**以创建您的密钥。

   ![保存][13]

   显示保存的项，您可以将其复制。

   ![保存密钥][8]

4. 您现在可以使用您密钥作为主体服务进行身份验证。 除了您的**密钥**登录，您将需要您的**客户端 ID** 。 转到**客户机 ID** ，并将其复制。
  
   ![客户机 id][5]


您的应用程序现在已准备就绪，在您的组织创建服务主体。 在登录为主体服务的时候一定要使用︰

* **客户机 ID** -为您的用户名称。
* **键**-作为您的密码。

## 下一步行动

- 若要了解如何指定安全策略，请参阅[管理和审核对资源的访问](azure-portal/resource-group-rbac.md)  
- 允许服务主体访问资源的步骤，请参阅[身份验证服务主体与 Azure 资源管理器](./resource-group-authenticate-service-principal.md)  
- 基于角色的访问控制的概述，请参阅[Microsoft Azure 门户中基于角色的访问控制](role-based-access-control-configure.md)
- 实施安全使用 Azure 资源管理器中的指导，请参阅[为 Azure 资源管理器中的安全注意事项](best-practices-resource-manager-security.md)


<!-- Images. -->
[1]: ./media/resource-group-create-service-principal-portal/active-directory.png
[2]: ./media/resource-group-create-service-principal-portal/active-directory-details.png
[3]: ./media/resource-group-create-service-principal-portal/application-configure.png
[4]: ./media/resource-group-create-service-principal-portal/app-properties.png
[5]: ./media/resource-group-create-service-principal-portal/client-id.png
[6]: ./media/resource-group-create-service-principal-portal/create-application.png
[7]: ./media/resource-group-create-service-principal-portal/create-key.png
[8]: ./media/resource-group-create-service-principal-portal/save-key.png
[9]: ./media/resource-group-create-service-principal-portal/tell-us-about-your-application.png
[10]: ./media/resource-group-create-service-principal-portal/what-do-you-want-to-do.png
[11]: ./media/resource-group-create-service-principal-portal/view-applications.png
[12]: ./media/resource-group-create-service-principal-portal/add-icon.png
[13]: ./media/resource-group-create-service-principal-portal/save-icon.png

测试
