---
ms.openlocfilehash: 227545f7959078b610af4bcd956b967620ad33e7
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="为 Azure 中订购 Office 365 管理目录"
   description="使用 Azure Active Directory 和管理门户 Office 365 订阅帐户目录进行管理"
   services="active-directory"
   documentationCenter=""
   authors="curtand"
   manager="stevepo"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="08/03/2015"
   ms.author="curtand"/>

#为 Azure 中订购 Office 365 管理目录

本文介绍如何管理 Azure 管理门户中 Office 365 订阅的创建一个目录。 若要完成此步骤是否已订阅的 Azure 而有所不同。 您必须是服务管理员或订阅了 Azure 的联管理员才可以登录到 Azure 管理门户。

如果您还没有 Azure 的订阅，您只需注册通过工作或学校用来登录到 Office 365 帐户。

![](./media/active-directory-manage-o365-subscription/AAD_O365_01.png)

Azure 的相应订阅将找不到，但您可以单击**注册 Azure**并且从 Office 365 提供帐户的相关信息将在注册表单中预填充。 默认情况下，相同的帐户将分配给服务管理员角色。

![](./media/active-directory-manage-o365-subscription/AAD_O365_02.png)

Azure 的订阅后，您可以登录到 Azure 管理门户并访问 Azure 服务。 为了管理验证 Office 365 提供用户位于同一目录中单击 Active Directory 扩展。

如果已经订阅了 Azure，来管理其他目录的过程也是简单明了的。 下面的关系图来帮助说明此过程。

在此示例中，迈克尔 • 史密斯 Contoso.com 具有 Office 365 的订阅。 他已通过他的 Microsoft 帐户，他签约了 Azure 订阅 msmith@hotmail.com。 在这种情况下，他管理着两个目录。

|  订阅 |  365 office  |  Azure |
|  -------------- | ------------- | ------------------------------- |
|  显示名称 |  Contoso  |     默认目录 |
|  域名称  |  contoso.com  | msmithhotmail.onmicrosoft.com |

他想要他登录到 Azure 使用 Microsoft 帐户，因此他可以启用 Azure 的广告功能，如多因素身份验证时管理 Contoso 目录中的用户标识。

![](./media/active-directory-manage-o365-subscription/AAD_O365_03.png)

在这种情况下，两个目录是相互独立。

##若要管理两个独立的目录
为了使他登录到作为 msmith@hotmail.com Azure 时管理这两个目录的迈克尔 • 史密斯，他需要完成以下步骤︰

> [AZURE.NOTE]
> 仅当用户登录到 Microsoft 帐户时，可以完成这些步骤。 如果用户使用工作或学校的帐户登录，**使用现有目录**的选项将不可用因为工作或学校的帐户可以通过身份验证只能由其主目录 (即，工作或学校帐户存储的目录，并且这由工作或学校)。

1.  登录到作为 msmith@hotmail.com Azure 的管理门户。
2.  单击**新建** > **应用程序服务** > **Active Directory** > **目录** > **自定义创建**。
3.  单击使用现有目录并选择**我已准备好要现在注销**复选框。
4.  以 Contoso.onmicrosoft.com (例如，msmith@contoso.com) 的全局管理员身份登录到 Azure 管理门户。
5.  当系统提示**Azure 使用 Contoso 目录？**，单击**继续**。
6.  单击**立即注销**。
7.  登录到管理门户为 msmith@hotmail.com。 Contoso 目录和默认目录显示在 Active Directory 扩展。

完成这些步骤之后，msmith@hotmail.com 是 Contoso 目录中的全局管理员。

##以全局管理员身份管理资源
现在让我们假设 John Doe 需要登录到 Azure 管理门户，并管理与 msmith@hotmail.com Azure 订阅关联的网站和数据库资源。 为此，迈克尔 • 史密斯需要完成以下附加步骤︰

1.  使用 Azure 订阅的服务管理员帐户登录到管理门户 (在此示例中，msmith@hotmail.com)。
2.  将订阅传送到 Contoso 目录︰ 单击**设置** > **订阅**> 选择订购 >**编辑目录**> 选择**Contoso (Contoso.com)**。 作为传输、 任何工作或学校的一部分是订阅的联管理员的帐户将被删除。
3.  作为联管理员订阅添加李向军︰ 单击**设置** > **管理员**> 选择订购 >**添加**> 键入**JohnDoe@Contoso.com**。

##下一步行动
有关订阅和目录之间的关系的详细信息，请参阅[如何订阅是与目录关联](active-directory-how-subscriptions-associated-directory.md)。

测试
