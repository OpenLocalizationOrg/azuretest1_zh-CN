---
ms.openlocfilehash: 38aba95af27ab3b59aa0ec6a34ce185c9b61ed69
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="PhoneFactor 代理升级到 Azure 的多因素身份验证服务器" 
    description="本文档介绍如何开始使用 Azure MFA 服务器以及如何从旧的 phonefactor 代理升级。" 
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

# PhoneFactor 代理升级到 Azure 的多因素身份验证服务器

升级 PhoneFactor 代理 v5.x 或旧到 Azure 多因素身份验证服务器要求在多因素身份验证服务器之前卸载 PhoneFactor 代理和相关的组件和其附属的组件可安装。 

## 要将 PhoneFactor 代理升级到 Azure 多因素身份验证服务器
<ol>
<li>首先，备份 PhoneFactor 数据文件。 默认的安装位置为 C:\Program Files\PhoneFactor\Data\Phonefactor.pfdata。


<li>如果已安装用户门户︰</li>
<ol>
<li>导航到安装文件夹和备份的 web.config 文件。 默认的安装位置是 C:\inetpub\wwwroot\PhoneFactor。</li>


<li>向门户网站添加了自定义主题，重新安装自定义文件夹的 C:\inetpub\wwwroot\PhoneFactor\App_Themes 目录下。</li>


<li>卸载用户门户通过 PhoneFactor 代理 （仅当安装在同一服务器上为 PhoneFactor 代理可用） 或通过 Windows 程序和功能。</li></ol>




<li>如果安装了移动应用程序 Web 服务︰
<ol>
<li>转到安装文件夹和备份的 web.config 文件。 默认的安装位置是 C:\inetpub\wwwroot\PhoneFactorPhoneAppWebService。</li>
<li>卸载 Windows 通过移动应用程序 Web 服务程序和功能。</li></ol>

<li>如果安装了 Web 服务 SDK，将通过 PhoneFactor 代理或通过 Windows 程序和功能将其卸载。

<li>通过 Windows PhoneFactor 卸载程序和功能。

<li>安装多因素身份验证服务器。 请注意，安装路径拾取从以前的 PhoneFactor 代理安装的注册表以便它应该安装在相同的位置 (例如 C:\Program Files\PhoneFactor)。 新的安装会有不同的默认安装路径 （如 C:\Program Files\Multi 双因素身份验证服务器）。 因此，您的用户和设置仍然应该有新的多因素身份验证服务器安装后，应安装过程中，升级留下的以前的 PhoneFactor 代理的数据文件。

<li>如果出现提示，请激活多因素身份验证服务器，并确保它被分配给正确的复制组。

<li>如果以前安装的 Web 服务 SDK 安装新的 Web 服务 SDK 通过多因素身份验证服务器的用户界面。 请注意，默认虚拟目录名称而不是"PhoneFactorWebServiceSdk""MultiFactorAuthWebServiceSdk"。 如果您想要使用以前的名称，则必须在安装过程中更改虚拟目录的名称。 否则，如果您允许安装以使用新的默认名称，您将需要更改 URL 中的任何应用程序引用 Web 服务 SDK 用户门户网站和移动应用程序 Web 服务指向正确的位置等。

<li>如果用户门户以前 PhoneFactor 代理服务器上安装，安装新的多因素身份验证用户门户通过多因素身份验证服务器的用户界面。 请注意，默认虚拟目录名称而不是"PhoneFactor""MultiFactorAuth"。 如果您想要使用以前的名称，则必须在安装过程中更改虚拟目录的名称。 否则为如果您允许安装以使用新的默认名称，则应该单击中多因素身份验证服务器的用户门户图标和更新用户门户 URL 中的设置选项卡上。 

<li>如果从 PhoneFactor 代理不同的服务器上以前安装的用户门户和/或移动 Web 服务应用程序︰
<ol>
<li>转到安装位置 (例如 C:\Program Files\PhoneFactor)，并复制到其他服务器上相应的 installer(s)。 有 32 位和 64 位安装程序的用户门户网站和移动应用程序 Web 服务。 它们被分别称为 MultiFactorAuthenticationUserPortalSetupXX.msi 和 MultiFactorAuthenticationMobileAppWebServiceSetupXX.msi。</li>
<li>要在 web 服务器上安装的用户门户，打开作为管理员命令提示符并运行 MultiFactorAuthenticationUserPortalSetupXX.msi。 请注意，默认虚拟目录名称而不是"PhoneFactor""MultiFactorAuth"。 如果您想要使用以前的名称，则必须在安装过程中更改虚拟目录的名称。 否则为如果您允许安装以使用新的默认名称，则应该单击中多因素身份验证服务器的用户门户图标和更新用户门户 URL 中的设置选项卡上。 现有的用户需要被告知新的 URL。</li>
<li>转到用户门户安装位置 (如 C:\inetpub\wwwroot\MultiFactorAuth)，然后编辑 web.config 文件。 在 appSettings 和 applicationSettings 部分中原始 web.config 文件备份之前升级到新的 web.config 文件中复制的值。 如果新的默认虚拟目录名称已保留安装 Web 服务 SDK 时，，更改的 applicationSettings 部分中的 URL，以指向正确的位置。 如果以前的 web.config 文件中更改其他的默认值，则将这些相同的更改应用于新的 web.config 文件。</li>
<li>要在 web 服务器上安装移动应用程序 Web 服务，请打开作为管理员命令提示符并运行 MultiFactorAuthenticationMobileAppWebServiceSetupXX.msi。 请注意，默认虚拟目录名称而不是"PhoneFactorPhoneAppWebService""MultiFactorAuthMobileAppWebService"。 如果您想要使用以前的名称，则必须在安装过程中更改虚拟目录的名称。 您可能需要选择一个较短的名称，以方便最终用户能够键入他们的移动设备上。 否则为如果您允许安装以使用新的默认名称，则应该单击中多因素身份验证服务器的移动应用程序图标和更新移动应用程序的 Web 服务 URL。</li>
<li>转到移动应用程序 Web 服务安装位置 (如 C:\inetpub\wwwroot\MultiFactorAuthMobileAppWebService)，然后编辑 web.config 文件。 在 appSettings 和 applicationSettings 部分中原始 web.config 文件备份之前升级到新的 web.config 文件中复制的值。 如果新的默认虚拟目录名称已保留安装 Web 服务 SDK 时，，更改的 applicationSettings 部分中的 URL，以指向正确的位置。 如果以前的 web.config 文件中更改其他的默认值，则将这些相同的更改应用于新的 web.config 文件。</li></ol>


 


 