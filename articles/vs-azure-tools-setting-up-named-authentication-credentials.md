---
ms.openlocfilehash: 1c7ffba4a4328b35537c448065e53f4d958d8123
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="设置指定身份验证凭据"
   description="了解到提供凭据，Visual Studio 如何使用到 Azure 的请求发布到 Azure 应用程序从 Visual Studio 或监视现有的云服务进行身份验证。 "
   services="visual-studio-online"
   documentationCenter="na"
   authors="kempb"
   manager="douge"
   editor="tglee" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="09/02/2015"
   ms.author="kempb" />

# 设置指定身份验证凭据

若要发布到 Azure 应用程序从 Visual Studio 或监视现有的云服务，则必须提供可以使用 Visual Studio 到 Azure 的请求进行身份验证的凭据。 从几个方面在 Visual Studio 在签名中提供这些凭据。 例如，从服务器资源管理器中，您可以打开**Azure**节点的快捷菜单并选择**连接到 Azure**。 当您登录时，使用 Azure 帐户相关联的订阅信息也可在 Visual Studio 中，没有什么需要做更多。

Azure 工具还支持一种旧方法提供凭据，使用订阅文件 （.publishsettings 文件）。 本主题介绍在 Azure SDK 2.2 仍支持此方法。

下列各项所需的身份验证到 Azure。

- 您的订购 ID

- 有效的 X.509 v3 证书

>[AZURE.NOTE] X.509 v3 证书密钥的长度必须至少为 2048 位。 Azure 将拒绝任何证书，没有满足这一要求或这不是有效。

Visual Studio 将使用证书数据与您的订阅 ID 作为凭据。 订阅文件 （.publishsettings 文件），其中包含公用密钥的证书中引用相应的凭据。 订阅文件可以包含多个订阅的凭据。

在本主题后面所述，您可以编辑**新建/编辑订阅**对话框中，从预订信息。

如果要自己创建的证书，可以引用文档中的说明[创建并上载 Azure 管理证书](https://msdn.microsoft.com/library/windowsazure/gg551722.aspx)，然后手动将证书传到管理门户。

>[AZURE.NOTE] Visual Studio 的要求来管理您的云服务这些凭据不相同对 Azure 存储服务的请求进行身份验证所需的凭据。

## 修改或导出在 Visual Studio 中的身份验证凭据

您还可以设置、 修改或导出**新建订阅**对话框中，如果您执行下列操作之一将出现在您的身份验证凭据︰

- 在**服务器资源管理器**中打开的**Azure**节点的快捷菜单选择**管理订阅**、 选择**证书**选项卡，选择**新建**或**编辑**按钮。

- 发布从**发布 Azure 应用程序**向导 Azure 的云服务时中**选择您的订阅**列表中，, 选择**管理**然后选择证书选项卡，然后选择**新建**或**编辑**按钮。

以下过程假定**新建订阅**对话框处于打开状态。

### 若要设置在 Visual Studio 中的身份验证凭据

1. 在**选择现有证书**的身份验证列表中，选择证书。

1. 选择**复制完整路径**按钮。证书 （.cer 文件） 的路径复制到剪贴板。

    >[AZURE.IMPORTANT] 从 Visual Studio 将 Azure 应用程序发布，您必须将此证书上载到管理门户。

1. 若要将证书传到 Azure 管理门户︰

    1. 选择 Azure 门户链接。

         [Azure 管理门户](http://go.microsoft.com/fwlink/?LinkID=213885)打开。

    1. 通过使用您的 Microsoft 帐户，登录到 Azure 管理门户网站，然后选择**云服务**按钮。

    1. 选择您感兴趣的云服务。

        打开服务页。

    1. 在**证书**选项卡上选择**上载**按钮。

    1. 粘贴刚才创建的.cer 文件的完整路径，然后输入指定的密码。

测试
