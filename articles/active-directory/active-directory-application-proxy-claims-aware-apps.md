---
ms.openlocfilehash: 07d028b4a1123ff2caba535a90217d5c9edddc00
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="使用声明感知应用程序在应用程序代理"
    description="介绍如何使用 Azure AD 应用程序代理获取启动并运行。"
    services="active-directory"
    documentationCenter=""
    authors="rkarlin"
    manager="stevenpo"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/11/2015"
    ms.author="rkarlin"/>



# 使用声明感知应用程序在应用程序代理


声明感知应用程序执行重定向到 STS，其中又以标记交换用户请求凭据，将用户重定向到应用程序之前。 若要启用应用程序代理服务器能够使用这些重定向，以下步骤需要采取以启用应用程序代理使用声明感知应用程序。

> [AZURE.IMPORTANT] 应用程序代理服务器是仅当您升级到特优或 Azure Active directory 的基本版才可用的功能。 有关详细信息，请参见[Azure Active Directory 版本](active-directory-editions.md)。


## 系统必备组件

之前执行此过程，请确保声明感知应用程序重定向到 STS 有您的内部网络之外。

### Azure 的入口配置

1. 发布您的应用程序，根据所述[应用程序代理的发布应用程序](active-directory-application-proxy-publish.md)的说明。
2. 在应用程序的列表中，选择声明感知应用程序并单击**配置**。
3. 如果您选择**直通**作为**预身份验证方法**，确保为**外部 URL**方案选择**HTTPS** 。
4. 如果您选择 Azure Active Directory 作为**预身份验证方法**，请确保选择**无**作为**内部身份验证方法**。

### ADFS 配置

1. 打开**ADFS 管理**。
2. 转到**信赖方信任**并右键单击该应用程序使用应用程序代理，发布并选择**属性**。

![信赖方信任右键单击应用程序名称-screentshot][1]

3. 在**终结点**选项卡上，在**终结点的类型**下选择**WS 联合身份验证**。
4. 在**受信任 URL**，输入您在应用程序代理服务器在**外部 URL**中输入的 URL，然后单击**确定**。

![添加终结点的设置受信任 URL 值的屏幕抓图][2]

<!--image references-->
[1]: ./media/active-directory-application-proxy-claims-aware-apps/AppDropdown.jpg
[2]: ./media/active-directory-application-proxy-claims-aware-apps/AddEndpoint.jpg

测试
