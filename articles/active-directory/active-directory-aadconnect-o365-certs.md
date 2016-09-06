---
ms.openlocfilehash: 672fef126163ba7519282629d0aa3d9dfbe71599
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="Office 365 和 Azure AD 用户证书续订指南。" 
    description="本文介绍如何解决问题的电子邮件，通知他们有关续订证书给 Office 365 的用户。" 
    services="active-directory" 
    documentationCenter="" 
    authors="billmath" 
    manager="stevenpo" 
    editor="curtand"/>

<tags 
    ms.service="active-directory" 
    ms.workload="identity" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/24/2015" 
    ms.author="billmath"/>


# Office 365 和 Azure AD 更新联合身份验证证书

如果您收到一封电子邮件或门户通知要求您续订您的证书对于 Office 365，本文旨在帮助您解决此问题并防止其再次发生。  本文假定使用 AD FS 联合身份验证服务器。

## 请检查您是否需要执行任何操作

如果使用 AD FS 2.0 或更高版本，Office 365 和 Azure 广告将自动更新您的证书到期之前将它。  您不需要执行任何手动步骤，或作为计划任务运行的脚本。  为此，两个以下默认 AD FS 配置设置必须有效︰

- AutoCertificateRollover 的 AD FS 属性必须设置为 True，指示，AD FS 将自动生成新的令牌签名和令牌解密证书旧的过期之前。
    - 如果的值为 False，您正在使用自定义的证书设置。  转[此处](https://msdn.microsoft.com/library/azure/JJ933264.aspx#BKMK_NotADFSCert)的全面指南。
- 联合身份验证元数据必须可供公用互联网。
    
    下面介绍了如何检查︰

    - 验证您的 AD FS 安装使用的主要联合身份验证服务器上的 PowerShell 命令窗口中执行以下命令自动证书变换图像︰

    `PS C:\> Get-ADFSProperties`

（请注意，如果您正在使用 AD FS 2.0，您将需要首先运行添加 Pssnapin Microsoft.Adfs.Powershell） 其他。

请检查您联合元数据 （从公司网络中） 公用的 internet 上的计算机从导航到下面的 URL 是公共可访问︰


https:// (your_FS_name) /federationmetadata/2007-06/federationmetadata.xml 

其中`(your_FS_name) `替换为您的组织使用，如 fs.contoso.com 的联合身份验证服务主机名。  如果您能够进行验证这些设置成功，则无需执行任何其他操作。  

示例︰ https://fs.contos.com/federationmetadata/2007-06/federationmetadata.xml

## 如果您的 AutoCertificateRollover 属性设置为 False

AutoCertificateRollover 属性设置为 False 时，如果您正在使用非默认的 AD FS 证书设置。  这最常见的原因是企业管理 AD FS 登记的组织的证书颁发机构颁发的证书。  在这种情况下，您需要进行续订，然后手动更新您的证书。  使用本指南[此处](https://msdn.microsoft.com/library/azure/JJ933264.aspx#BKMK_NotADFSCert)。

## 如果您的元数据不是可公开访问
如果将 AutocertificateRollover 设置为 True，但是是不公开的联合元数据，使用下面的过程以确保在部署和云更新您的证书︰

### 请验证您的 AD FS 系统已生成新的证书。 

- 请验证您登录到主的 AD FS 服务器。
- 检查当前签名证书在 AD FS 通过打开 PowerShell 命令窗口并运行以下命令︰ 

`PS C:\>Get-ADFSCertificate –CertificateType token-signing.` 

（请注意，是否您正在使用 AD FS 2.0，您将需要首先运行添加 Pssnapin Microsoft.Adfs.Powershell）


- 查看命令输出中，列出的任何证书。  如果 AD FS 已生成新的证书，您会看到输出中的两个证书︰ 一个为其 IsPrimary 值是 True，并且 NotAfter 日期是在 5 天内，一个 IsPrimary 的假和 NotAfter 是关于将来的一年。
    
- 如果您仅看到一份证书，并且 NotAfter 日期是在 5 天内，您需要通过执行以下步骤来生成新的证书。

- 若要生成新的证书，请执行以下命令 PowerShell 命令提示符处︰ `PS C:\>Update-ADFSCertificate –CertificateType token-signing`。

- 通过再次运行下面的命令来验证此更新︰ PS c:\>Get ADFSCertificate — CertificateType 令牌签名
- 接下来，要手动更新 Office 365 联合身份验证信任属性，请执行以下步骤。

应现在列出的两个证书，其中有 NotAfter 日期大约一年以后，并为 IsPrimary 的值为 False。


### 手动更新 Office 365 联合身份验证信任属性，请执行以下步骤。

1.  打开 Windows PowerShell Microsoft Azure Active Directory 模块。
2.  运行 $cred = 获取凭据。 当此 cmdlet 将提示您输入凭据时，请键入您的云服务管理员帐户凭据。
3.  运行连接 MsolService – 凭据 $cred。 此 cmdlet 将您连接到云服务。 运行任何其他 cmdlet 由此工具安装之前必须创建连接到云服务的上下文。
4.  如果您不是 AD FS 联合主服务器的计算机上运行这些命令，运行一组 MSOLAdfscontext-计算机<AD FS primary server>，其中<AD FS primary server>是主要的 AD FS 服务器内部 FQDN 名称。 此 cmdlet 创建的上下文中将您连接到 AD FS。 
5.  运行更新 MSOLFederatedDomain – 域名<domain>。 此 cmdlet 更新 AD FS 到云服务的设置，并配置两个之间的信任关系。

>[AZURE.NOTE] 如果您需要支持多个顶级域，如 contoso.com 和 fabrikam.com，您必须与任何 cmdlet 使用 SupportMultipleDomain 开关。 有关详细信息，请参阅支持多个顶层级别域。
最后，确保与[Windows 服务器 5 月 2014年](http://support.microsoft.com/kb/2955164)汇总，否则为这些代理可能无法自我更新与新的证书，从而导致停机更新所有 Web 应用程序代理服务器。

测试
