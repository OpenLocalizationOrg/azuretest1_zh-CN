---
ms.openlocfilehash: a4b0e5377852ad39cb2fa8feab4bc8dd33225182
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="安全云以及内部资源使用 AD FS 2.0 Azure 多因素身份验证服务器" 
    description="这是介绍如何开始使用 Azure MFA 和 AD FS 2.0 Azure 的多因素身份验证页。" 
    services="multi-factor-authentication" 
    documentationCenter="" 
    authors="billmath" 
    manager="stevenpo" 
    editor="curtland"/>

<tags 
    ms.service="multi-factor-authentication" 
    ms.workload="identity" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/24/2015" 
    ms.author="billmath"/>
# 安全云以及内部资源使用 AD FS 2.0 Azure 多因素身份验证服务器

如果组织联合，Azure Active Directory 并具有内部的资源或者云中的想要安全，您可以执行此操作通过使用 Azure 的多因素身份验证服务器并将其配置为使用 AD FS 因此对于高价值终点将触发多因素身份验证。

本文档介绍如何使用 AD FS 2.0 使用 Azure 的多因素身份验证服务器。  在 Azure 多因素身份验证中使用 Windows 服务于 2012 R2 AD FS 的信息请参阅[安全云以及内部资源使用与 Windows Server 2012 R2 AD FS Azure 多因素身份验证服务器](multi-factor-authentication-get-started-adfs-w2k12.md)。


## AD FS 2.0 代理
要保护与代理服务器的 AD FS 2.0，ADFS 在代理服务器上安装 Azure 的多因素身份验证服务器和配置服务器每以下步骤。 

### 若要通过代理服务器安全保障 AD FS 2.0

1. 在 Azure 多因素身份验证服务器内单击左侧菜单中的 IIS 身份验证图标。
2. 单击基于窗体的选项卡。
3. 单击添加。. 按钮。
<center>![安装](./media/multi-factor-authentication-get-started-adfs-adfs2/setup1.png)</center>
4. 检测用户名、 密码和域变量自动，Auto-Configure Form-Based 网站对话框内输入登录 URL (例如 https://sso.contoso.com/adfs/ls) 并单击确定。
5. 如果所有用户已经执行或将导入到服务器和受多因素身份验证，请选中需要 Azure 多因素身份验证的用户匹配。 如果大量用户不还已导入到服务器和/或将会受多因素身份验证，请取消选中框。 此功能，请参阅帮助文件的其他信息。
6. 如果页变量无法自动检测到，请单击手动指定... Auto-Configure Form-Based 网站对话框中的按钮。
7. Add Form-Based 网站对话框中，在 ADFS 登录页提交 URL 字段 (例如 https://sso.contoso.com/adfs/ls) 中输入的 URL，并输入应用程序名称 （可选）。 应用程序名称在 Azure 多因素身份验证报告中出现，并可能显示 SMS 或移动应用程序的身份验证消息内。 提交 url，请参阅帮助文件获得详细信息。
8. 设置为"过帐或获得"请求格式。
9. 输入的用户名变量 (ctl00$ ContentPlaceHolder1$ UsernameTextBox) 和密码变量 (ctl00$ ContentPlaceHolder1$ PasswordTextBox)。 如果您的基于窗体的登录页显示一个域的文本框，输入域变量也。 您可能需要导航到 web 浏览器中的登录页，在页上右击并选择"查看源"，以查找在登录页中输入框的名称。
10. 如果所有用户已经执行或将导入到服务器和受多因素身份验证，请选中需要 Azure 多因素身份验证的用户匹配。 如果大量用户不还已导入到服务器和/或将会受多因素身份验证，请取消选中框。
<center>![安装](./media/multi-factor-authentication-get-started-adfs-adfs2/manual.png)</center>
11. 单击高级按钮。. 按钮以查看高级的设置，其中包括能够选择一个自定义拒绝页文件，缓存一段时间使用 cookie 成功身份验证的网站，选择主要的凭据进行身份验证的方式。
12. 由于 ADFS 代理服务器不可能加入到域，您可能会使用 LDAP 连接到您的用户导入预身份验证的域控制器。 在 Advanced Form-Based 网站对话框中，单击主身份验证选项卡并选择预身份验证的身份验证类型"LDAP 绑定"。
13. 完成后，单击确定按钮以返回到 Add Form-Based 网站对话框中。 有关高级设置，请参阅帮助文件获得详细信息。
14. 单击确定按钮关闭该对话框。
15. 一旦被检测到或输入 URL 和页变量，将基于窗体的面板中显示的网站数据。
16. 单击本机模块选项卡，然后选择服务器、 ADF 代理运行下 （如"默认 Web 站点"） 的网站或 ADFS 代理应用程序 (例如"ls""adfs"下) 以启用 IIS 插件所需的级别。
17. 单击屏幕顶部的启用 IIS 身份验证框。
18. 现在已启用了 IIS 身份验证。 但是，以便执行预身份验证向您活动目录 (AD) 通过 LDAP 必须配置为域控制器的 LDAP 连接。 若要执行此操作，请单击目录集成图标。
19. 在设置选项卡上，选择使用特定 LDAP 配置单选按钮。
<center>![安装](./media/multi-factor-authentication-get-started-adfs-adfs2/ldap1.png)</center>
20. 单击编辑命令。. 按钮。
21. 在编辑 LDAP 配置对话框中连接到 AD 域控制器所需的信息填充的字段。 下表中包含字段的说明。 注︰ 在 Azure 多因素身份验证服务器帮助文件还包含此信息。
22. 通过单击测试按钮测试的 LDAP 连接。
<center>![安装](./media/multi-factor-authentication-get-started-adfs-adfs2/ldap2.png)</center>
23. 如果 LDAP 连接测试成功，请单击确定按钮。
24. 接下来，单击公司设置图标，然后选择用户名解析选项卡。
25. 选择匹配的用户名单选按钮使用 LDAP 唯一标识符属性。
26. 如果用户将在 ADFS 代理登录窗体"域 \ 用户名"格式输入他们的用户名，服务器将需要能够创建 LDAP 查询时去除从用户名的域。 它可以通过注册表设置来完成。
27. 打开注册表编辑器中，在 64 位服务器上转置此变量/软件/Wow6432Node/正网络/PhoneFactor 对。 如果在 32 位服务器上，需要从路径"Wow6432Node"。 创建新的 DWORD 注册表项名为"UsernameCxz_stripPrefixDomain"，并将值设置为 1。 现在，azure 的多因素身份验证保证安全 ADFS 代理。 请确保该用户已导入从 Active Directory 服务器。 如果想要到白名单内部 IP 地址，因此两因素身份验证不需要登录到该网站从这些位置时，请参阅下面的部分信任的 Ip。

<center>![安装](./media/multi-factor-authentication-get-started-adfs-adfs2/reg.png)</center>

## 代理服务器不直接 AD FS 2.0

为了确保 AD FS 安全不使用 AD FS 代理时，AD FS 服务器上安装 Azure 的多因素身份验证服务器和配置服务器每以下步骤。 

### 保护无代理服务器的 AD FS 2.0
1. 在 Azure 多因素身份验证服务器内单击左侧菜单中的 IIS 身份验证图标。
2. 单击 HTTP 选项卡。
3. 单击添加。. 按钮。
4. 在添加基 URL 的对话中，基 URL 字段中输入的 URL 的 HTTP 身份验证是在 ADFS 网站执行 (如 https://sso.domain.com/adfs/ls/auth/integrated)，输入应用程序名称 （可选）。 应用程序名称在 Azure 多因素身份验证报告中出现，并可能显示 SMS 或移动应用程序的身份验证消息内。
5. 如果需要，请调整空闲超时和最长会话时间。
6. 如果所有用户已经执行或将导入到服务器和受多因素身份验证，请选中需要 Azure 多因素身份验证的用户匹配。 如果大量用户不还已导入到服务器和/或将会受多因素身份验证，请取消选中框。 此功能，请参阅帮助文件的其他信息。
7. 如果需要，请检查 cookie 缓存。
<center>![安装](./media/multi-factor-authentication-get-started-adfs-adfs2/noproxy.png)</center>
8. 单击确定按钮。
9. 单击本机模块选项卡，然后选择服务器、 ADF 运行下 （如"默认 Web 站点"） 的网站或 ADF 应用程序 (例如"ls""adfs"下) 以启用 IIS 插件所需的级别。
10. 单击屏幕顶部的启用 IIS 身份验证框。 Azure 的多因素身份验证保证 ADFS 现在安全。 请确保该用户已导入从 Active Directory 服务器。 如果想要到白名单内部 IP 地址，因此两因素身份验证不需要登录到该网站从这些位置时，请参阅下面的部分信任的 Ip。


## 信任的 Ip
信任的 Ip 允许用户跳过 Azure 多因素身份验证的网站请求来自特定 IP 地址或子网。 例如，可能要免除用户从 Azure 多因素身份验证登录从办公室时。 为此，您会为信任的 Ip 项指定办公网。 

### 若要配置受信任的 Ip


1. 在 IIS 身份验证部分中，单击信任的 Ip 选项卡。
1. 单击添加。. 按钮。
1. 出现添加可信的 Ip 对话框时，选择单个 IP、 IP 范围或子网单选按钮。
1. 输入 IP 地址、 IP 地址范围或子网，应列入白名单。 如果输入一个子网，选择适当的子网掩码，然后单击确定按钮。 现在已添加到信任的 IP。


<center>![安装](./media/multi-factor-authentication-get-started-adfs-adfs2/trusted.png)</center>

 