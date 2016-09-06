---
ms.openlocfilehash: c454725c0772105767c97620380c15ec317644be
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="将 Microsoft Azure 管理 API 证书上载到门户网站" 
    description="了解如何上载 Microsoft Azure 中的管理员管理 API 证书" 
    services="cloud-services" 
    documentationCenter=".net" 
    authors="Thraka" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="na" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/20/2015"
    ms.author="adegeo"/>


# 上载的 Azure 管理 API 管理证书

管理证书，可以使用提供的 Azure 服务管理 API 进行身份验证。 许多程序和工具 （如 Visual Studio 或 Azure SDK） 将使用这些证明来自动化配置和部署各种 Azure 服务。 这些真的无关以云服务。 

>[AZURE.WARNING] 小心！ 这些类型的证书允许他们可以管理与其关联的订阅通过身份验证的任何人。 

如果您需要它，Azure 证书 （包括创建自签名的证书） 的详细信息是[可用](cloud-services/cloud-services-certs-create.md#what-are-management-certificates)的。

您还可以使用[Azure Active Directory](http://azure.microsoft.com/documentation/services/active-directory/)进行身份验证的客户端代码进行自动化。

## 上载管理证书

一旦您有一个管理证书创建的 （仅使用公钥.cer 文件） 可以将其上载到门户网站。 门户中可用的证书时，匹配 certficiate （私钥） 的任何人都可以通过管理 API 连接和访问的资源相关联的订阅。

1. 登录到[Azure 的管理门户](http://manage.windowsazure.com)。
2. 单击左侧的入口 （您可能需要向下滚动） 的**设置**。 
    
    ![Settings](./media/azure-api-management-certs/settings.png)

3. 单击**管理证书**tab.1

    ![Settings](./media/azure-api-management-certs/certificates-tab.png)
    
4. 请单击**上载**按钮。

    ![Settings](./media/azure-api-management-certs/upload.png)
    
5. 填写对话框信息，然后单击完成的**复选标记**。

    ![Settings](./media/azure-api-management-certs/upload-dialog.png)

## 下一步行动

现在，您已经与订阅管理证书，可以 （安装本地匹配的证书后） 以编程方式连接到[服务管理 REST API，](https://msdn.microsoft.com/library/azure/ee460799.aspx)并自动化也是与此订阅关联的各种 Azure 资源。 测试

写访问权限测试
