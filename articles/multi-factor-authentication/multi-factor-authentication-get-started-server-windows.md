---
ms.openlocfilehash: dea471324d24c88543964d641994076681e608b5
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="Windows 身份验证和 Azure 的多因素身份验证服务器" 
    description="这是在部署 Windows 身份验证和 Azure 多因素身份验证服务器将帮助 Azure 的多因素身份验证页。" 
    services="multi-factor-authentication" 
    documentationCenter="" 
    authors="billmath" 
    manager="stevenpo" 
    editor="curtand"/>

<tags 
    ms.service="multi-factor-authentication" 
    ms.workload="identity" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/24/2015" 
    ms.author="billmath"/>

# Windows 身份验证和 Azure 的多因素身份验证服务器

Windows 身份验证部分允许管理员启用并配置一个或多个应用程序的 Windows 身份验证。  以下是一些事项需要注意之前设置 Windows 身份验证。

-  终端服务 Azure 多因素身份验证将在生效之前需要重新启动。
-  如果您不是用户列表中选中需要 Azure 多因素身份验证用户匹配项，您将不能在重新启动之后登录到计算机。
-  信任的 Ip 是依赖于应用程序是否可以提供与身份验证的客户端 IP。 目前终端服务支持。  







>[AZURE.NOTE]到安全 Windows Server 2012 R2 上的终端服务不支持此功能。
 



## 若要保护的应用程序使用 Windows 身份验证，请使用以下过程。

1. 在 Azure 多因素身份验证服务器中单击 Windows 身份验证图标。
![Windows 身份验证](./media/multi-factor-authentication-get-started-server-windows/windowsauth.png)
2. 选中启用 Windows 身份验证复选框。 默认情况下未选中此框。
3. 应用程序选项卡允许管理员配置一个或多个应用程序的 Windows 身份验证。
4. 选择服务器或应用程序 — 指定是否启用服务器/应用程序。 单击确定。
5. 单击添加。. 按钮。
6. 信任的 Ip 选项卡允许您跳过来自特定的 IPs 的 Azure 多因素身份验证的 Windows 会话。 例如，如果员工使用该应用程序从办公室和家庭，您可以决定您不希望其响的 Azure 多因素身份验证，而在办公室的电话。 为此，您会为信任的 Ip 项指定办公网。
7. 单击添加。. 按钮。
8. 如果您想要跳过单个 IP 地址，请选择单个 IP。
9. 如果您想要跳过整个 IP 范围，选择 IP 范围。 示例 10.63.193.1-10.63.193.100。
10. 如果您想指定使用表示法的子网的 Ip 范围，请选择子网。 输入的子网起始 IP 并选择从下拉列表中相应的子网掩码。 
11. 单击确定按钮。