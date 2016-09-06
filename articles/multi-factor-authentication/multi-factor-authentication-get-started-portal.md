---
ms.openlocfilehash: 9a0b9db9a8931d3e8e61b1eb31d58048665855fd
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="Azure 多因素身份验证服务器部署用户门户" 
    description="这是介绍如何开始使用 Azure MFA 和用户门户 Azure 的多因素身份验证页。" 
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

# Azure 多因素身份验证服务器部署用户门户

用户门户允许管理员安装和配置 Azure 多因素身份验证的用户门户。 用户门户是 IIS 网站，该网站允许用户注册 Azure 多因素身份验证，并对帐户进行维护。 用户可能会更改他们的电话号码、 更改 PIN，或在其下一次注册时避开 Azure 多因素身份验证。 

用户将登录到用户门户使用其正常的用户名和密码并将完成 Azure 多因素身份验证呼叫或回答无法完成其身份验证的安全问题。 如果允许用户注册，用户将配置其电话号码和 PIN 登录用户门户网站第一次。 

可能设置并添加新用户并更新现有用户的权限授予用户门户管理员。 

<center>![安装](./media/multi-factor-authentication-get-started-portal/install.png)</center>

## 部署在 Azure 的多因素身份验证服务器所在的服务器上的用户门户

下列必备组件安装所需的用户门户在 Azure 的多因素身份验证服务器所在的服务器上︰

- IIS 必须安装包括 asp.net 和 IIS 6 元基本兼容性 （IIS 7 或更高版本） 
- 已登录的用户必须具有管理员权限的计算机和域，如果适用。  这是因为该帐户需要创建 Active Directory 安全组的权限。

### 若要为 Azure 的多因素身份验证服务器部署用户门户

1. Azure 的多因素身份验证服务器中︰ 单击用户门户图标左侧的菜单中，单击安装用户门户按钮。 
1. 单击下一步。
1. 单击下一步。
1. 如果计算机加入到域，并且为保护用户门户和 Azure 多因素身份验证服务之间的通信安全的 Active Directory 配置不完整，则将显示 Active Directory 步骤。 单击下一步按钮以自动完成此配置。
1. 单击下一步。
1. 单击下一步。
1. 单击关闭。
1. 从任何计算机上打开 web 浏览器并定位到装有用户门户网站的 URL (例如 https://www.publicwebsite.com/MultiFactorAuth)。 确保显示没有证书警告或错误。

<center>![安装](./media/multi-factor-authentication-get-started-portal/portal.png)</center>

## 部署在单独的服务器上的 Azure 的多因素身份验证服务器用户门户

为了使用 Azure 多因素身份验证的应用程序，以下是必需的以便该应用程序会成功地与用户门户︰ 

请有关硬件和软件要求，参阅硬件和软件要求︰

- 您必须使用 v6.0 或更高版本的 Azure 的多因素身份验证服务器。
- 用户门户网站必须安装在运行 Microsoft® Internet Information Services (IIS) 面向 Internet 的 web 服务器上 6.x IIS 7.x 版或更高版本。
- 在使用 IIS 时 6.x，确保安装、 注册和设置为允许 ASP.NET v2.0.50727。
- 在使用 IIS 时必需的角色服务 7.x 版或更高版本包含 ASP.NET 和 IIS 6 元数据库兼容性。
- 用户门户网站应使用 SSL 证书的安全。
- 必须在 IIS 中安装 Azure 多因素身份验证 Web 服务 SDK 6.x，IIS 7.x 版或更高版本在 Azure 的多因素身份验证服务器安装在服务器上。
- 使用 SSL 证书，Azure 多因素身份验证 Web 服务 SDK 必须加以保护。
- 用户门户网站必须能够通过 SSL 连接到 Azure 多因素身份验证 Web 服务 SDK。
- 用户门户网站必须能够对 Azure 多因素身份验证 Web 服务 SDK 使用的是名为"PhoneFactor 管理员"安全组的成员的服务帐户凭据进行身份验证。 此服务帐户和组在 Active Directory 中如果存在 Azure 的多因素身份验证服务器加入域的服务器上运行。 此服务帐户和组在本地存在 Azure 的多因素身份验证服务器上如果不加入域。

Azure 多因素身份验证服务器之外的服务器上安装的用户门户，需要以下三个步骤︰

1. 安装 web 服务 SDK
2. 安装用户门户
3. 在 Azure 的多因素身份验证服务器上配置用户门户设置


### 安装 web 服务 SDK

如果 Azure 的多因素身份验证服务器上尚未安装 Azure 多因素身份验证 Web 服务 SDK，转到该服务器，并打开 Azure 的多因素身份验证服务器。 单击 Web 服务 SDK 图标，然后单击安装 Web 服务 SDK... 按钮，然后按照显示的说明进行操作。 使用 SSL 证书，必须保护 Web 服务 SDK。 自行签署式证书没什么关系，为此目的，但它具有导入用户门户 web 服务器上的本地计算机帐户的"受信任的根证书颁发机构"存储区，以便初始化 SSL 连接时，它将信任该证书。 

<center>![安装](./media/multi-factor-authentication-get-started-portal/sdk.png)</center>

### 安装用户门户

之前安装在单独的服务器上的用户门户，请注意以下事项︰

- 最好先打开面向 Internet 的 web 服务器上的 web 浏览器并定位到的 URL 输入到 web.config 文件中的 Web 服务 sdk。 如果浏览器可以成功地访问 web 服务，则应提示您提供凭据。 输入的用户名和密码的 web.config 文件中输入了它在文件中显示的完全一样。 确保显示没有证书警告或错误。
- 如果反向代理服务器或防火墙坐用户门户 web 服务器并执行 SSL 卸载，可以编辑用户门户 web.config 文件并添加到下面的项<appSettings>节，以便用户门户可以使用 http，而不是 https。 <add key="SSL_REQUIRED" value="false"/>

#### 若要安装用户门户

1. 在 Azure 多因素身份验证服务器上打开 Windows 资源管理器中，导航到 Azure 的多因素身份验证服务器所在的文件夹 （如 C:\Program Files\Multi 双因素身份验证服务器）。 选择 MultiFactorAuthenticationUserPortalSetup 安装文件作为适用于服务器，用户门户将安装在 32 位或 64 位版本。 将安装文件复制到的面向 Internet 的服务器。
2. 在面向 Internet 的 web 服务器上，必须以管理员权限运行安装程序文件。 若要执行此操作的最简单方法是打开作为管理员命令提示符下，导航到复制安装文件的位置的位置。
3. 运行 MultiFactorAuthenticationUserPortalSetup64 的安装文件，如果需要更改的网站和虚拟目录名称。
4. 完成安装的用户门户后, 浏览到 C:\inetpub\wwwroot\MultiFactorAuth （或相应的目录，根据虚拟目录名称） 和编辑 web.config 文件。
5. 找到的 USE_WEB_SERVICE_SDK 键并从假的值更改为 true。 找到的 WEB_SERVICE_SDK_AUTHENTICATION_USERNAME 和 WEB_SERVICE_SDK_AUTHENTICATION_PASSWORD 键并将值设置为用户名和密码的服务帐户的成员的 PhoneFactor 管理员安全组 （请参见上面的要求一节）。 请务必在行结尾引号之间输入的用户名和密码 (值 =""/ >)。 建议使用限定的用户名 （例如，域 \ 用户名或按）
6. 找到的 pfup_pfwssdk_PfWsSdk 设置并更改到 Azure 多因素身份验证服务器 (例如 https://computer1.domain.local/MultiFactorAuthWebServiceSdk/PfWsSdk.asmx) 上运行的 Web 服务 sdk 的 URL 的值从"http://localhost:4898/PfWsSdk.asmx"。 因为此连接使用 SSL，则必须引用 Web 服务 SDK 中的服务器名称而不是 IP 地址，因为 SSL 证书将服务器名称发出和使用的 URL 必须与证书上的名称匹配。 如果服务器名称无法解析为 IP 地址从面向 Internet 的服务器，到 Azure 的多因素身份验证服务器名称映射到 IP 地址的服务器上的主机文件中添加一个条目。 后进行了更改，则保存 web.config 文件。
7. 如果用户门户安装在下 （例如默认 Web 站点） 的网站尚未公开签名证书的绑定，在服务器上安装证书如果还没有安装，打开 IIS 管理器并将证书绑定到该网站。
8. 从任何计算机上打开 web 浏览器并定位到装有用户门户网站的 URL (例如 https://www.publicwebsite.com/MultiFactorAuth)。 确保显示没有证书警告或错误。



## 在 Azure 多因素身份验证服务器上配置用户门户设置
安装门户时，需要将 Azure 多因素身份验证服务器配置为与门户网站合作。

Azure 的多因素身份验证服务器提供用户门户的几个选项。  下表列出了这些选项和解释的他们的用途是什么。

用户门户设置|说明|
:------------- | :------------- | 
用户门户 URL| 允许您输入所在该门户的 URL。
主的身份验证| 允许您指定要登录到门户网站时使用的身份验证类型。  Windows、 半径或 LDAP 身份验证。
允许用户登录|允许用户为用户门户登录页面上输入的用户名和密码。  如果不选择此选项，将灰色框。
允许用户注册|允许用户在多因素身份验证中登记采用它们将提示用户输入附加信息，如电话号码设置屏幕。  提示对于备份电话允许用户指定的备用电话号码。  提示对于第三方誓言令牌允许用户指定一个第三方誓言令牌。
允许用户启动一次性旁路| 这样用户就可以发起零星旁路。  如果用户设置启动后这将影响到下一次用户登录。  提示输入旁路秒为用户提供了列表框使用户可以更改的默认值为 300 秒。  否则，一次性绕过才好为 300 秒。
允许用户选择方法| 允许用户指定他们的主要联系方式。  这可以是电话、 短信、 移动应用程序或誓言的标记。
允许用户选择语言|  允许用户更改所使用的电话、 短信、 移动应用程序或誓言的标记语言。
允许用户激活移动应用程序| 允许用户生成激活代码以完成与服务器一起使用的移动应用程序激活过程。  您还可以设置它们可以激活此功能的设备的数量。  介于 1 和 10。
使用安全问题进行回退|允许您在多因素身份验证失败的情况下使用安全问题。  您可以指定的数目必须成功回答的安全问题。
允许用户将相关联的第三方誓言标记| 允许用户指定一个第三方誓言令牌。
誓言令牌用于回退|多因素身份验证不成功的事件中允许使用誓言令牌。  您还可以以分钟为单位指定的会话超时值。
启用日志记录|启用用户门户网站上的记录。  日志文件位于以下位置︰ C:\Program Files\Multi 双因素身份验证 Server\Logs。

大多数的这些设置可以看到一旦启用这些用户并为用户门户的用户信号。

![用户门户设置](./media/multi-factor-authentication-get-started-portal/portalsettings.png)



### 若要在 Azure 多因素身份验证服务器上配置用户门户设置




1. 在 Azure 的多因素身份验证服务器，单击用户门户图标。 在设置选项卡上，用户门户用户门户 URL 文本框中输入的 URL。 将它们导入到 Azure 的多因素身份验证服务器已启用电子邮件功能时将发送给用户的电子邮件中插入此 URL。
2. 选择您想要在用户门户网站中使用的设置。 例如，如果用户有权控制其身份验证方法，确保他们可以选择的方法，允许用户选择方法处于选中状态。
3. 单击帮助了解的任何显示设置右上角的帮助链接。

<center>![安装](./media/multi-factor-authentication-get-started-portal/config.png)</center>


## 管理员选项卡
只是，此选项卡允许您添加具有管理权限的用户。  当添加管理员，您可以微调他们收到的权限。  这种方式，您可以确保仅向管理员授予所需的权限。  只需单击添加按钮，然后选择用户和他们的权限，然后单击添加。

![用户门户管理员](./media/multi-factor-authentication-get-started-portal/admin.png)


## 安全问题
此选项卡可以指定用户将需要提供的答案，如果选择备用选项使用安全问题时的安全问题。  Azure 多元 Authenticaton 服务器附带了默认值的问题，您可以使用。  您还可以更改顺序或添加您自己的问题。  当添加您自己的问题，您可以指定要将这些问题也出现在语言。

![用户门户安全性问题](./media/multi-factor-authentication-get-started-portal/secquestion.png)


## 传入的会话

## SAML
允许您设置用户门户来接受来自使用 SAML 标识提供程序声明。  您可以指定超时会话，指定验证证书和注销重定向 URL。

![SAML](./media/multi-factor-authentication-get-started-portal/saml.png)

## 信任的 Ip
此选项卡可以指定单个 IP 地址或 IP 地址范围，以便用户从这些 IP 地址的一个签名，如果避开多因素身份验证可以添加。 

![用户门户受信任的 Ip](./media/multi-factor-authentication-get-started-portal/trusted.png)

## 自助服务用户注册
如果您希望您的用户登录和注册必须选择允许用户登录到，并允许用户注册选项。 请记住，您选择的设置会影响用户的登录体验。

例如，当用户登录到的用户门户，并单击登录按钮，就会再进入 Azure 多因素身份验证用户安装程序页。  这取决于您如何配置 Azure 多因素身份验证，用户可以选择的身份验证方法。  

如果他们选择语音调用的身份验证方法，或已被预先配置为使用该方法，则页将提示用户输入他们的主电话号码和扩展，有可能的话。  他们还可能允许输入备份电话号码。  

![用户门户受信任的 Ip](./media/multi-factor-authentication-get-started-portal/backupphone.png)

如果用户需要验证身份时使用 PIN，页还将提示他们输入 PIN。  之后输入其电话号码和 pin 码 （如果适用），用户可单击调用我现在为验证按钮。  Azure 的多因素身份验证将电话联络对执行身份验证用户的主要电话号码。  用户必须应答电话呼叫和输入他们的 PIN （如果适用），然后按 # 将自注册过程的下一步。   

如果用户选择的 SMS 文本身份验证方法或已被预先配置为使用该方法时，页将提示用户输入他们的移动电话号码。  如果用户需要验证身份时使用 PIN，页还将提示他们输入 PIN。  之后输入其电话号码和 PIN （如果适用），用户可单击的文本我现在为验证按钮。  Azure 的多因素身份验证将执行到用户的手机 SMS 进行身份验证。  用户必须接收 SMS 包含一次密码 (OTP) 和与该 OTP 再加上他们的 PIN 回复，如果适用的话） 能够自我注册过程的下一步。 

![用户门户 SMS](./media/multi-factor-authentication-get-started-portal/text.png)   

如果用户选择的移动应用程序的身份验证方法或已被预先配置为使用该方法时，页将提示用户在其设备上安装 Azure 多因素身份验证的应用程序，并生成激活代码。  在安装之后 Azure 多因素身份验证的应用程序，用户可单击生成激活代码按钮。    

>[AZURE.NOTE]若要使用 Azure 多因素身份验证的应用程序，用户必须启用推式通知为其设备。 

激活码和以及条码图片的 URL，则页面会显示。  如果用户需要验证身份时使用 PIN，页还将提示他们输入 PIN。  用户进入 Azure 多因素身份验证应用程序的激活码和 URL 或使用条码扫描器扫描条码图片并单击激活按钮。    

完成激活后，用户可单击立即验证我按钮。  Azure 的多因素身份验证将对执行身份验证用户的移动应用程序。  用户必须输入他们的 PIN （如果适用），并按其移动应用程序，能够自我注册过程的下一步的身份验证按钮。  


如果管理员已配置了 Azure 多因素身份验证服务器收集安全问题和答案，用户然后进入安全问题页。  用户必须选择四个安全问题，并为他们挑选出的问题提供答案。    

![用户门户安全性问题](./media/multi-factor-authentication-get-started-portal/secq.png)  

现已完成用户自我注册和用户登录到的用户门户。  用户可以重新登录到用户门户在将来任何时候改变它们的电话号码、 针、 身份验证方法和安全问题，如果允许他们的管理员。

 