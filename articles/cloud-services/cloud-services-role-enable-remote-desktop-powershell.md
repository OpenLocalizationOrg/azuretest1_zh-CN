---
ms.openlocfilehash: 5e9d9fcbd283fed8958212042022616e60f144cc
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
pageTitle="启用远程桌面连接的 Azure 的云服务中的角色" 
description="如何配置 azure 的云服务应用程序，以允许远程桌面连接" 
services="cloud-services" 
documentationCenter="" 
authors="sbtron" 
manager="timlt" 
editor=""/>
<tags 
ms.service="cloud-services" 
ms.workload="tbd" 
ms.tgt_pltfrm="na" 
ms.devlang="na" 
ms.topic="article" 
ms.date="08/31/2015" 
ms.author="saurabh"/>

# 启用远程桌面连接中使用 PowerShell 的 Azure 云服务的角色

>[AZURE.SELECTOR]
- [Azure 门户](cloud-services-role-enable-remote-desktop.md)
- [PowerShell](cloud-services-role-enable-remote-desktop-powershell.md)
- [Visual Studio](https://msdn.microsoft.com/library/gg443832.aspx)


远程桌面可以访问运行于 Azure 角色的桌面。 您可以使用远程桌面连接来诊断并排除问题的应用程序运行时。 

本文介绍如何启用远程桌面使用 PowerShell 您云服务角色。 这篇文章所需的系统必备组件，请参阅[如何安装和配置 Azure PowerShell](powershell-install-configure.md) 。 PowerShell 使用远程桌面扩展方法，因此即使在部署应用程序后，您可以启用远程桌面。 


## 配置远程桌面从 PowerShell

[一组 AzureServiceRemoteDesktopExtension](https://msdn.microsoft.com/library/azure/dn495117.aspx) cmdlet 允许您启用指定的角色上的远程桌面或云服务部署的所有角色。 该 cmdlet 允许您指定的用户名和密码*凭据*参数，它接受一个 PSCredential 对象通过远程桌面用户。

如果您以交互方式使用 PowerShell 可以轻松地设置 PSCredential 对象通过调用[获取凭据](https://technet.microsoft.com/library/hh849815.aspx)cmdlet。 

```
    $remoteusercredentials = Get-Credential
```

这将显示一个对话框，使您可以为远程用户输入的用户名和密码，以安全的方式。 

由于 PowerShell 将主要用于自动化方案也可以安装程序 PSCredential 对象不需要用户交互的方式。 为此首先需要安装一个安全的密码。 在开始使用纯文本密码将其转换为使用[ConvertTo SecureString](https://technet.microsoft.com/library/hh849818.aspx)安全字符串指定。 接下来，您需要将此安全字符串转换为使用[ConvertFrom SecureString](https://technet.microsoft.com/library/hh849814.aspx)加密标准字符串。 现在您可以使用[一组内容](https://technet.microsoft.com/library/ee176959.aspx)保存此加密标准字符串。 创建 PSCredential 对象时您可以读取此文件以安全的方式设置的密码，而无需指定密码提示，或以纯文本格式存储密码。 

使用以下 PowerShell 创建安全密码文件︰  

```
    ConvertTo-SecureString -String "Password123" -AsPlainText -Force | ConvertFrom-SecureString | Set-Content "password.txt"
``` 

创建密码文件 (password.txt) 后您将只使用此文件并不必指定密码以纯文本格式。 如果您需要更新的密码然后可以运行上述 powershell 再次用新密码来生成一个新的 password.txt 文件。 

>[AZURE.IMPORTANT] 设置密码进行确认时，您满足[复杂性要求](https://technet.microsoft.com/library/cc786468.aspx)。 

创建安全密码文件中的凭据对象必须读取该文件的内容并将它们转换为使用[ConvertTo SecureString](https://technet.microsoft.com/library/hh849818.aspx)的安全字符串。 除了凭据[集 AzureServiceRemoteDesktopExtension](https://msdn.microsoft.com/library/azure/dn495117.aspx) cmdlet 也接受*到期*参数，用于指定用户帐户过期的日期时间。 可以设置它指定静态日期和时间或您可以简单地选择从当前日期的几天过期帐户。

此 PowerShell 的示例演示如何在云服务上设置远程桌面扩展︰   

```
    $servicename = "cloudservice"
    $username = "RemoteDesktopUser"
    $securepassword = Get-Content -Path "password.txt" | ConvertTo-SecureString
    $expiry = $(Get-Date).AddDays(1)
    $credential = New-Object System.Management.Automation.PSCredential $username,$securepassword
    Set-AzureServiceRemoteDesktopExtension -ServiceName $servicename -Credential $credential -Expiration $expiry 
```
您还可以选择指定的部署插槽和您想要启用远程桌面的角色。 如果这些参数未指定 cmdlet 将默认使用生产部署插槽并启用远程桌面在生产部署中的所有角色。 

远程桌面扩展程序与部署。 如果您要创建新部署的服务您将需要启用远程桌面在新部署上。 如果您想始终具有在您的部署中启用远程桌面然后应考虑集成到您的部署工作流启用远程桌面 PowerShell 脚本。


## 为角色实例的远程桌面
到云服务的一个特定角色实例，可以到远程桌面使用[Get AzureRemoteDesktopFile](https://msdn.microsoft.com/library/azure/dn495261.aspx) cmdlet。 您可以使用*LocalPath*参数上 cmdlet 下载本地 RDP 文件或您可以使用*启动*参数直接启动远程桌面连接对话框来访问云服务角色实例。

```
    Get-AzureRemoteDesktopFile -ServiceName $servicename -Name "WorkerRole1_IN_0" -Launch
```


## 如果启用了远程桌面扩展服务检查 
[获得 AzureServiceRemoteDesktopExtension](https://msdn.microsoft.com/library/azure/dn495261.aspx) cmdlet 显示是否启用远程桌面服务部署。 该 cmdlet 将返回启用了远程桌面扩展的角色以及远程桌面用户的用户名。 与所生产的默认值，您可以选择指定部署插槽。

```
    Get-AzureServiceRemoteDesktopExtension -ServiceName $servicename
```

## 从服务中删除远程桌面扩展 
如果您已经启用了远程桌面扩展在部署并需要更新远程桌面设置您必须首先删除远程桌面扩展，然后在使用新的设置再次启用它。 例如，如果您要设置为远程用户帐户的新密码，或如果已过期的用户帐户，则需要移除扩展，然后在新密码或过期再次添加它。 这只是上已经启用了远程桌面扩展的现有部署所必需的。 针对新的部署，您可以只调用直接应用扩展。

若要删除从 s 服务部署远程桌面扩展可以使用[删除 AzureServiceRemoteDesktopExtension](https://msdn.microsoft.com/library/azure/dn495280.aspx) cmdlet。 您还可以选择指定的部署插槽和想要删除远程桌面扩展的角色。 

```
Remove-AzureServiceRemoteDesktopExtension -ServiceName $servicename -UninstallConfiguration

```  

>[AZURE.NOTE] *UninstallConfiguration*参数将卸载时应用于此服务的任何扩展配置。 是相关联的所有扩展配置服务配置激活的部署扩展部署必须是与该扩展配置相关联。 调用*UninstallConfiguration*不删除 cmdlet 将解除关联，从而有效地删除扩展从部署扩展配置中的部署。 但是扩展配置仍将保持与该服务相关联。 若要完全移除扩展配置应调用删除 cmdlet 使用*UninstallConfiguration*参数。 



## 其他资源

[如何配置云服务](cloud-services-how-to-configure.md)测试
