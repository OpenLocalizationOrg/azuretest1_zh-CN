---
ms.openlocfilehash: 23f0ec47bd824900bf020a7428125a23ca695694
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="使用 Windows Server 2012 R2 AD FS Azure MFA 服务器安全云以及内部资源" 
    description="这是介绍如何开始使用 Azure MFA 和 Windows Server 2012 R2 上的 AD FS Azure 的多因素身份验证页。" 
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


# 使用 Windows Server 2012 R2 AD FS Azure 多因素身份验证服务器的安全云以及内部资源

如果您的组织使用 Azure 的广告联合和有内部的资源或云中的想要安全，您可以执行此操作通过使用 Azure 的多因素身份验证服务器并将其配置为使用 AD FS 因此对于高价值终点将触发多因素身份验证。

本文档介绍如何使用 Windows Server 2012 R2 中的 AD FS 使用 Azure 的多因素身份验证服务器。  有关使用 Azure 使用 AD FS 2.0 的多因素身份验证请参见[安全云以及内部资源使用 Azure 使用 AD FS 2.0 的多因素身份验证服务器](multi-factor-authentication-get-started-adfs-adfs2.md)。

## 保护 Windows Server 2012 R2 AD FS Azure 的多因素身份验证服务器

在 Azure 的多因素身份验证服务器安装时您有以下两个选项︰

- 在相同的 AD FS 服务器上本地安装 Azure 的多因素身份验证服务器 
- 在 AD FS 服务器上本地安装 Azure 的多因素身份验证适配器，并在另一台计算机上安装 MFA 服务器

在开始之前，请注意以下信息︰

- 它不是 Azure 的多因素身份验证服务器 AD FS 联合身份验证服务器上安装，但是，AD fs 将多因素身份验证适配器必须安装在运行 AD FS Windows Server 2012 R2 上的要求。 可以将服务器安装在另一台计算机上，只要是受支持的版本，并在 AD FS 联合身份验证服务器上单独安装 AD FS 适配器。 上单独安装该适配器，请参阅下面的说明的过程。

- 在已签名的帐户必须具有在 Active Directory 中创建安全组的权限。

- 多因素身份验证 AD FS 适配器安装向导创建在活动目录中调用 PhoneFactor 管理员安全组，然后将 AD FS 联合身份验证服务服务帐户添加到此组。建议验证在域控制器上，实际上创建 PhoneFactor 管理员组和 AD FS 服务帐户是该组的成员。 如有必要，AD FS 服务帐户对域控制器上 PhoneFactor 管理员组手动添加。
  

### 若要在相同的 AD FS 服务器上本地安装 Azure 的多因素身份验证服务器

1. 下载并安装您的 AD FS 联合身份验证服务器上的 Azure 的多因素身份验证服务器。 有关安装 Azure 多因素身份验证服务器的信息，请参阅[Azure 的多因素身份验证服务器入门](multi-factor-authentication-get-started-server.md)
2. 在 Azure 多因素身份验证服务器用户界面中，选择 AD FS 图标并选择选项，允许用户注册，并允许用户选择方法。
3. 选择任何其他选项。
4. 单击安装 AD FS 适配器。
<center>![云](./media/multi-factor-authentication-get-started-adfs-w2k12/server.png)</center>
5. 如果计算机加入到域并且安全的 AD FS 适配器和多因素身份验证服务之间的通信的 Active Directory 配置不完整，则将显示 Active Directory 步骤。  单击下一步按钮以自动完成此配置或检查跳过自动 Active Directory 配置和配置设置手动复选框，然后单击下一步。
6. 如果计算机未加入到域和本地组配置来保护 AD FS 适配器和多因素身份验证服务之间的通信不完整，则将显示该本地组的步骤。  单击下一步按钮以自动完成此配置或检查自动跳过本地组配置和配置设置手动复选框，然后单击下一步。
7. 这将启动安装向导，请单击下一步允许 Azure 多因素身份验证服务器创建 PhoneFactor 管理员组并将 AD FS 服务帐户添加到 PhoneFactor 管理员组。
<center>![云](./media/multi-factor-authentication-get-started-adfs-w2k12/adapter.png)</center>
8. 在启动安装程序的步骤中，单击下一步。
9. 在多因素身份验证 AD FS 适配器安装程序，单击下一步。
10. 安装完成后，请单击关闭。
11. 既然已经安装适配器，必须使用 AD FS 将进行注册。 打开 Windows PowerShell 并运行以下命令︰ C:\Program Files\Multi 双因素身份验证 Server\Register MultiFactorAuthenticationAdfsAdapter.ps1<center>![云](./media/multi-factor-authentication-get-started-adfs-w2k12/pshell.png)</center>
12. 现在，我们需要编辑在 AD FS 使用我们新注册的适配器的全局身份验证策略。 在 AD FS 管理控制台中，导航到身份验证策略节点，并在多因素身份验证部分下，单击全局设置子部分旁的编辑链接。 在编辑全局身份验证策略窗口中，选择作为额外的身份验证方法，多因素身份验证，然后单击确定。 该适配器注册为 WindowsAzureMultiFactorAuthentication。  您必须重新启动 AD FS 服务登记才能生效。

<center>![云](./media/multi-factor-authentication-get-started-adfs-w2k12/global.png)</center>

此时，多因素身份验证服务器被设置为使用 AD FS 的其他身份验证提供程序。

## 安装使用 Web 服务 SDK AD FS 适配器独立
1. 多因素身份验证服务器运行的服务器上安装 Web 服务 SDK。
2. 复制 MultiFactorAuthenticationAdfsAdapterSetup64.msi，登记-MultiFactorAuthenticationAdfsAdapter.ps1，注销-MultiFactorAuthenticationAdfsAdapter.ps1 和 \Program Files\Multi 的 MultiFactorAuthenticationAdfsAdapter.config 文件-因子身份验证服务器目录更改为您要在安装 AD FS 适配器的服务器。
3. 运行 MultiFactorAuthenticationAdfsAdapterSetup64.msi。
4. 在多因素身份验证 AD FS 适配器安装程序，单击下一步以执行安装。
5. 安装完成后单击关闭按钮。
6. 通过以下方法编辑 MultiFactorAuthenticationAdfsAdapter.config 文件︰

MultiFactorAuthenticationAdfsAdapter.config 步骤| 子步骤
:------------- | :------------- |
UseWebServiceSdk 节点设置为 true。||
将 WebServiceSdkUrl 设置为 SDK 多因素身份验证 Web 服务的 URL。||
选项 1-使用用户名和密码配置 Web 服务 SDK。|<ol><li>将 WebServiceSdkUsername 设置为 PhoneFactor 管理员安全组的成员帐户。  使用<domain>\<用户名 > 格式。<li>将 WebServiceSdkPassword 设置为适当的帐户密码。</li></ol>
选项 2-使用客户端证书配置 Web 服务 SDK。|<ol><li>从服务器运行 Web 服务 SDK 证书颁发机构获取客户端证书。</li><li>客户端证书导入到运行 Web 服务 SDK 的服务器上的本地计算机个人证书存储中。  注意︰ 请确保在受信任的根证书是证书颁发机构的公钥证书。</li><li>将客户端证书的公钥和私钥的密钥导出到一个.pfx 文件。</li><li>将公钥以 base-64 格式导出到.cer 文件。</li><li>在服务器管理器中，验证安装了 Web 服务器 (IIS) \Web Server\Security\Client 证书映射身份验证功能。</li><li>如果未安装，请选择要添加这项功能的添加角色和功能。</li><li>在 IIS 管理器中，双击配置编辑器包含 Web 服务 SDK 虚拟目录的网站中。  注意︰ 它是非常重要，为此在网站级别并不是虚拟目录级别。</li><li>导航到 system.webServer/security/authentication/iisClientCertificateMappingAuthentication 部分。</li><li>设置为启用。</li><li>将 oneToOneCertificateMappingsEnabled 设置为 true。</li><li>单击...oneToOneMappings 按钮。</li><li>单击添加链接。</li><li>打开前面导出的 base 64.cer 文件。  删除---开始证书---、---最终证书---和任何换行。  将复制生成的字符串。</li><li>在上一步中复制的字符串设置证书。</li><li>设置为启用。</li><li>设置为 PhoneFactor 管理员安全组的成员的帐户的用户名。  使用<domain>\<用户名 > 格式。</li><li>将密码设置为适当的帐户密码。</li><li>关闭集合编辑器中。</li><li>单击应用链接。</li><li>导航到 Web 服务 SDK 的虚拟目录。</li><li>双击身份验证。</li><li>验证已启用 ASP.NET 模拟和基本身份验证，并禁用所有其他项目。</li><li>重新定位到 Web 服务 SDK 的虚拟目录。</li><li>双击 SSL 设置。</li><li>设置为接受客户端证书，然后单击应用。</li><li>将复制到服务器运行 AD FS 适配器先前导出的.pfx 文件。</li><li>.Pfx 文件导入到本地计算机个人证书存储中。</li><li>从右键单击菜单中选择管理私钥并授予以登录到 Active Directory 联合身份验证服务的服务帐户的读取权限。</li><li>打开客户端证书，并从详细信息选项卡中复制指纹。</li><li>在 MultiFactorAuthenticationAdfsAdapter.config 文件中，将 WebServiceSdkCertificateThumbprint 设置到上一步中复制的字符串。</li></ol>
编辑寄存器 MultiFactorAuthenticationAdfsAdapter.ps1 脚本添加-ConfigurationFilePath<path>到末尾的登记 AdfsAuthenticationProvider 命令的位置<path>到 MultiFactorAuthenticationAdfsAdapter.config 文件的完整路径。|


现在，运行 \Program Files\Multi-因子身份验证 Server\Register MultiFactorAuthenticationAdfsAdapter.ps1 脚本继续注册适配器。  该适配器注册为 WindowsAzureMultiFactorAuthentication。  您必须重新启动 AD FS 服务登记才能生效。 




























 

 


 

 


 





 


 

























































































 


 

 






 