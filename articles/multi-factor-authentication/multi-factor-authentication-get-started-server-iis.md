---
ms.openlocfilehash: 53d8cb5d659fc6ce6ba74670057fb5178d5a53cd
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="IIS 身份验证和 Azure 的多因素身份验证服务器" 
    description="这是部署 IIS 身份验证和 Azure 多因素身份验证服务器将协助 Azure 的多因素身份验证页。" 
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

# IIS 身份验证

Azure 的多因素身份验证服务器的 IIS 身份验证部分允许您启用和配置 IIS 集成的身份验证与 Microsoft IIS web 应用程序。 Azure 的多因素身份验证服务器安装的插件，它可以筛选到 IIS web 服务器中要添加 Azure 多因素身份验证所做的请求。 插件 IIS 提供的基于表单的身份验证和集成 Windows HTTP 身份验证的支持。 受信任的 IPs 还到免税的内部 IP 地址从两因素身份验证配置。 


![IIS 身份验证](./media/multi-factor-authentication-get-started-server-iis/iis.png)


## 对 Azure 的多因素身份验证服务器使用基于窗体的 IIS 身份验证

要保护 IIS web 应用程序使用基于窗体的身份验证，在 IIS web 服务器上安装 Azure 的多因素身份验证服务器和配置服务器每下面的过程。

1. 在 Azure 多因素身份验证服务器内单击左侧菜单中的 IIS 身份验证图标。
2. 单击基于窗体的选项卡。
3. 单击添加。. 按钮。
4. 检测用户名、 密码和域变量自动，Auto-Configure Form-Based 网站对话框内输入登录 URL (例如 https://localhost/contoso/auth/login.aspx) 并单击确定。
5. 如果所有用户已经执行或将导入到服务器和受多因素身份验证，请选中需要多因素身份验证的用户匹配。 如果大量用户不还已导入到服务器和/或将会受多因素身份验证，请取消选中框。
6. 如果页变量无法自动检测到，请单击手动指定... Auto-Configure Form-Based 网站对话框中的按钮。
7. 在 Add Form-Based 网站对话框中，将 URL 输入到登录页提交 URL 字段中，输入应用程序名称 （可选）。 应用程序名称在 Azure 多因素身份验证报告中出现，并可能显示 SMS 或移动应用程序的身份验证消息内。 提交 url，请参阅帮助文件获得详细信息。 
8. 选择正确的请求格式。 这是大多数 web 应用程序设置为"开机自检或获得"。
9. 输入变量的用户名、 密码变量和局域变量 （如果它显示在登录页）。 您可能需要导航到 web 浏览器中的登录页，页上右击并选择"查看源文件"，若要查找的页面中输入框的名称。
10. 如果所有用户已经执行或将导入到服务器和受多因素身份验证，请选中需要 Azure 多因素身份验证的用户匹配。 如果大量用户不还已导入到服务器和/或将会受多因素身份验证，请取消选中框。 此功能，请参阅帮助文件的其他信息。
11.  单击高级按钮。. 按钮以查看高级的设置，可以选择一个自定义拒绝页文件，包括要缓存的一段时间使用 cookie 的网站成功的身份验证，并选择是否主要针对 Windows 域、 LDAP 目录或 RADIUS 服务器凭据进行身份验证。 完成后单击确定按钮以返回到 Add Form-Based 网站对话框中。 有关高级设置，请参阅帮助文件获得详细信息。
12. 单击确定按钮。
13. 一旦被检测到或输入 URL 和页变量，将基于窗体的面板中显示的网站数据。
14. 请参阅启用 IIS 插件 Azure 多因素身份验证服务器部分直接下完成 IIS 身份验证配置。 

## 集成的 Windows 身份验证使用 Azure 的多因素身份验证服务器

要保护 IIS web 应用程序使用集成的 Windows HTTP 身份验证，在 IIS web 服务器上安装 Azure 的多因素身份验证服务器和配置服务器每下面的过程。 

1. 在 Azure 多因素身份验证服务器内单击左侧菜单中的 IIS 身份验证图标。
2. 单击 HTTP 选项卡。 单击基于窗体的选项卡。
3. 单击添加。. 按钮。
4. 在添加基 URL 的对话中，在基 URL 字段中输入 URL 的 HTTP 进行身份验证的网站执行 (如 http://localhost/owa)，输入应用程序名称 （可选）。 应用程序名称在 Azure 多因素身份验证报告中出现，并可能显示 SMS 或移动应用程序的身份验证消息内。
5. 空闲超时和最大值会话时间如果调整默认值是不够的。
6. 如果所有用户已经执行或将导入到服务器和受多因素身份验证，请选中需要多因素身份验证的用户匹配。 如果大量用户不还已导入到服务器和/或将会受多因素身份验证，请取消选中框。 此功能，请参阅帮助文件的其他信息。 
7. 如果需要，请检查 cookie 缓存。
8. 单击确定按钮。
9. 请参阅[启用 IIS 插件的 Azure 多因素身份验证服务器](#enable-iis-plug-ins-for-azure-multi-factor-authentication-server)部分直接下完成 IIS 身份验证配置。 


## Azure 的多因素身份验证服务器启用 IIS 插件

一旦您已配置了基于表单的或 HTTP 身份验证 Url 和设置，您必须在 IIS 中选择应加载并启用 Azure 多因素身份验证 IIS 插件的位置。 请执行以下步骤︰

1. 如果在 IIS 6 上运行，请单击 ISAPI 选项卡，然后选择 web 应用程序运行下 （例如默认 Web 站点） 以启用 Azure 多因素身份验证 ISAPI 筛选器插件对该网站的网站。
2. 如果运行在 IIS 7 或更高版本，单击本机模块选项卡，然后选择服务器、 有关网站或应用程序启用插件所需的级别在 IIS。
3. 单击屏幕顶部的启用 IIS 身份验证框。 现在，azure 的多因素身份验证保证安全所选的 IIS 应用程序。 请确保用户具有已导入服务器。 如果想要到白名单内部 IP 地址，因此两因素身份验证不需要登录到该网站从这些位置时，请参阅下面的部分信任的 Ip。 


## 信任的 Ip

信任的 Ip 允许用户跳过 Azure 多因素身份验证的网站请求来自特定 IP 地址或子网。 例如，可能要免除用户从 Azure 多因素身份验证登录从办公室时。 为此，您会为信任的 Ip 项指定办公网。 若要配置信任的 Ip，请使用以下过程︰

1. 在 IIS 身份验证部分中，单击信任的 Ip 选项卡。 
2. 单击添加。. 按钮。
3. 出现添加可信的 Ip 对话框时，选择单个 IP、 IP 范围或子网单选按钮。
4. nter 的 IP 地址、 IP 地址范围或子网，应列入白名单。 如果输入一个子网，选择适当的子网掩码，然后单击确定按钮。 现在已添加白名单。
