---
ms.openlocfilehash: ed8db57caceba9ca3409b3d84f383bb1debfc4a4
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="入门教程 MFA 服务器移动应用程序 Web 服务" 
    description="Azure 的多因素身份验证应用程序提供了一个额外的带外身份验证选项。  它允许 MFA 服务器使用推式通知给用户。" 
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

# 入门教程 MFA 服务器移动应用程序 Web 服务

Azure 的多因素身份验证应用程序提供了一个额外的带外身份验证选项。 而不是将自动的电话呼叫或 SMS 用户在登录过程中，Azure 多因素身份验证将推送通知到 Azure 的多因素身份验证应用程序用户的智能手机或 tablet 上。 用户只需点击"身份验证"（或输入 PIN 并点击"身份验证"） 在应用程序日志。 

为了使用 Azure 多因素身份验证的应用程序，下面是必需的以便应用程序可以成功与移动应用程序 Web 服务进行通信︰ 

- 请有关硬件和软件要求，参阅硬件和软件要求
- 您必须使用 v6.0 或更高版本的 Azure 的多因素身份验证服务器
- 移动应用程序 Web 服务必须安装在运行 Microsoft® Internet Information Services (IIS) 面向 Internet 的 web 服务器上 6.x 或 IIS 7.x
- 在使用 IIS 时 6.x，确保安装、 注册和设置为允许 ASP.NET v2.0.50727
- 所需的角色服务时使用 IIS 7.x 版包括 ASP.NET 和 IIS 6 元数据库兼容性
- 移动 Web 服务应用程序必须能够通过公用 URL 访问
- 移动 Web 服务应用程序必须可以安全使用 SSL 证书。
- 必须在 IIS 中安装 Azure 多因素身份验证 Web 服务 SDK 6.x 或 IIS 服务器上的安装 7.x 的 Azure 的多因素身份验证服务器
- 使用 SSL 证书，Azure 多因素身份验证 Web 服务 SDK 必须加以保护。
- 移动 Web 服务应用程序必须能够通过 SSL 连接到 Azure 多因素身份验证 Web 服务 SDK
- 移动 Web 服务应用程序必须能够对 Azure 多因素身份验证 Web 服务 SDK 使用的是名为"Azure 多因素身份验证管理员"安全组的成员的服务帐户凭据进行身份验证。 此服务帐户和组在 Active Directory 中如果存在 Azure 的多因素身份验证服务器加入域的服务器上运行。 此服务帐户和组在本地存在 Azure 多因素身份验证服务器上如果不加入域。


Azure 多因素身份验证服务器之外的服务器上安装的用户门户，需要以下三个步骤︰

1. 安装 web 服务 SDK
2. 安装移动应用程序 web 服务
3. 在 Azure 的多因素身份验证服务器配置的移动应用程序设置
4. 为最终用户激活 Azure 的多因素身份验证应用程序

## 安装 web 服务 SDK

如果 Azure 的多因素身份验证服务器上尚未安装 Azure 多因素身份验证 Web 服务 SDK，转到该服务器，并打开 Azure 的多因素身份验证服务器。 单击 Web 服务 SDK 图标，然后单击安装 Web 服务 SDK... 按钮，然后按照显示的说明进行操作。 使用 SSL 证书，必须保护 Web 服务 SDK。 自行签署式证书没什么关系，为此目的，但它具有导入用户门户 web 服务器上的本地计算机帐户的"受信任的根证书颁发机构"存储区，以便初始化 SSL 连接时，它将信任该证书。 

<center>![安装](./media/multi-factor-authentication-get-started-server-webservice/sdk.png)</center>

## 安装移动应用程序 web 服务
在安装之前移动应用程序的 web 服务，请注意以下︰

- 在面向 Internet 的服务器上已经安装了 Azure 多因素身份验证的用户门户，如果用户名、 密码和 Web 服务 SDK URL 可以复制用户门户网站的 web.config 文件中。 
- 最好先打开面向 Internet 的 web 服务器上的 web 浏览器并定位到的 URL 输入到 web.config 文件中的 Web 服务 sdk。 如果浏览器可以成功地访问 web 服务，则应提示您提供凭据。 输入的用户名和密码的 web.config 文件中输入了它在文件中显示的完全一样。 确保显示没有证书警告或错误。
- 如果反向代理服务器或防火墙坐在移动 Web 服务应用程序的 web 服务器之前，执行 SSL 卸载，可以编辑移动 Web 服务应用程序的 web.config 文件中，并添加到下面的项<appSettings>部分，以便移动应用程序 Web 服务可以使用 http，而不是 https。 但是 SSL 是从移动应用程序仍然需要到防火墙/反向代理服务器。 <add key="SSL_REQUIRED" value="false"/> 

### 若要安装移动应用程序 web 服务

<ol>
<li>在 Azure 多因素身份验证服务器上打开 Windows 资源管理器中，导航到 Azure 的多因素身份验证服务器所在的文件夹 （例如 C:\Program Files\Azure 多因素身份验证）。 选择 Azure 多元 AuthenticationPhoneAppWebServiceSetup 安装文件根据服务器的移动 Web 服务应用程序将被安装在 32 位或 64 位版本。 将安装文件复制到的面向 Internet 的服务器。</li> 

<li>在面向 Internet 的 web 服务器上，必须以管理员权限运行安装程序文件。 若要执行此操作的最简单方法是打开作为管理员命令提示符下，导航到复制安装文件的位置的位置。</li>  

<li>运行多因子 AuthenticationMobileAppWebServiceSetup 的安装文件，如果需要，请更改该站点并将虚拟目录更改为一个短的名称，如"PA"。 由于用户必须移动应用程序的 Web 服务 URL 输入到移动设备，在激活过程中建议简短的虚拟目录名称。</li> 

<li>完成安装的 Azure 多元 AuthenticationMobileAppWebServiceSetup 后, 浏览到 C:\inetpub\wwwroot\PA （或相应的目录，根据虚拟目录名称） 和编辑 web.config 文件。</li>  

<li>找到的 WEB_SERVICE_SDK_AUTHENTICATION_USERNAME 和 WEB_SERVICE_SDK_AUTHENTICATION_PASSWORD 键并将值设置为用户名和密码的服务帐户的成员的 PhoneFactor 管理员安全组 （请参见上面的要求一节）。 这可能是被用作标识的 Azure 多因素身份验证的用户门户如果先前安装了的同一帐户。 请务必在行结尾引号之间输入的用户名和密码 (值 =""/ >)。 建议使用限定的用户名 （例如，域 \ 用户名或按）。</li>  

<li>找到的 pfMobile 应用程序 Web Service_pfwssdk_PfWsSdk 设置并更改到 Azure 多因素身份验证服务器服务器 (如 https://computer1.domain.local/MultiFactorAuthWebServiceSdk/PfWsSdk.asmx) 上运行的 Web 服务 sdk 的 URL 的值从"http://localhost:4898/PfWsSdk.asmx"。 因为此连接使用 SSL，则必须引用 Web 服务 SDK 中的服务器名称而不是 IP 地址，因为 SSL 证书将服务器名称发出和使用的 URL 必须与证书上的名称匹配。 如果服务器名称无法解析为 IP 地址从面向 Internet 的服务器，到 Azure 多因素身份验证服务器名称映射到 IP 地址的服务器上的主机文件中添加一个条目。 后进行了更改，则保存 web.config 文件。</li>  

<li>如果移动 Web 服务应用程序已安装在下 （例如默认 Web 站点） 的网站尚未公开签名证书的绑定，在服务器上安装证书如果还没有安装，则打开 IIS 管理器并将证书绑定到该网站。</li>  

<li>从任何计算机上打开 web 浏览器并导航到安装移动 Web 服务应用程序的位置的 URL (例如 https://www.publicwebsite.com/PA)。 确保显示没有证书警告或错误。</li> 

### 在 Azure 的多因素身份验证服务器配置的移动应用程序设置
现在，安装了移动应用程序 web 服务，您需要 Azure 多因素身份验证服务器配置为使用该门户。

#### 在 Azure 的多因素身份验证服务器配置的移动应用程序设置

1. 在 Azure 的多因素身份验证服务器，单击用户门户图标。 如果允许用户控制其身份验证方法，请在设置选项卡上的允许用户选择方法，检查移动应用程序。 不启用此功能后，最终用户将需要您帮助台以完成激活移动应用程序，请与联系。
2. 检查允许用户激活移动应用程序。
3. 选中的复选框允许用户注册。
4. 单击移动应用程序图标。
5. 输入与安装 Azure 多元 AuthenticationMobileAppWebServiceSetup 之时所创建的虚拟目录使用的 URL。 可以在提供的空格中输入帐户名。 该公司的名称会显示在移动应用程序。 如果留空，则将显示在 Azure 管理门户中创建多因素身份验证提供程序的名称。 



<center>![安装](./media/multi-factor-authentication-get-started-server-webservice/mobile.png)</center>
 