---
ms.openlocfilehash: 8f362b97d4536f01007f4f340b968db0637befa3
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---

<properties 
    pageTitle="安全应用程序和 Azure 远程应用程序中的资源"
    description="了解如何锁定应用程序和 Azure 远程应用程序中的资源" 
    services="remoteapp" 
    documentationCenter="" 
    authors="lizap" 
    manager="mbaldwin" />

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/12/2015" 
    ms.author="elizapo" />



# 安全应用程序和 Azure 远程应用程序中的资源

Azure 的远程应用程序为用户提供了访问集中管理的 Windows 应用程序，从而使您可以控制您的用户可以做什么和不能做什么。  当用户连接从非托管设备 （如他们的个人 Macbook) 并且想要控制用户访问权限或经验，这是特别有用。
 
例如，如果您使用 Active Directory 进行用户身份验证，并且您要防止用户将数据从一个应用程序复制，您可以配置远程桌面组策略阻止用户到将数据复制中。
 
另一个例子是如果想要阻止 internet 访问集合中特定的应用程序。 您可以为集合创建映像时阻止访问的 Windows 防火墙规则。

## 实现选项

  下面是可用于单独或串联根据需要密钥实现选项︰

1.  如果远程应用程序集合中加入域，可以强制任何[组策略](https://technet.microsoft.com/library/cc725828.aspx)（除了空闲和断开超时策略描述[这里](../azure-subscription-service-limits.md)）。
2.  作为组策略 （如果您的集合不是加入域或您在 AD 中没有适当的权限） 的替代方法，您可以配置[本地策略](https://technet.microsoft.com/library/cc775702.aspx)模板图像。  请注意冲突时，该组策略王牌本地策略。
3.  某些操作系统/应用程序设置是不可配置的策略，通过，但可以通过配置模板图像时使用[注册表编辑器工具](./remoteapp-hybridtrouble.md)的注册表项。
4.  可以使用[Windows 防火墙](http://windows.microsoft.com/en-US/windows-8/Windows-Firewall-from-start-to-finish)来控制网络访问该计算机与运行应用程序的位置。 只需确保不要阻止 Url 和端口，在此处定义。
5.  您可以使用[AppLocker](https://technet.microsoft.com/library/hh831440.aspx)来控制哪些应用程序和文件的用户可以运行。 例如，聪明的用户也可以找出如何运行应用程序，您未发布，但可用的图像用来创建该集合中-AppLocker 可以阻止这。
 
## 详细的信息

- 下面的 RDS 策略很可能是最有用: 
    - [设备和资源重定向](https://technet.microsoft.com/library/ee791794.aspx)
    - [打印机重定向](https://technet.microsoft.com/library/ee791784.aspx)
    - [配置文件](https://technet.microsoft.com/library/ee791865.aspx)。
- 请注意，配置重定向通过 RemoteApp PowerShell 模块 （作为看到[此处](./remoteapp-redirection.md)） 依赖于客户端计算机强制实施策略，因此如果安全性的主要目标将需要强制通过模板图像的本地策略或组策略的策略。
- [Windows Server 2012 R2 的策略](https://technet.microsoft.com/library/hh831791.aspx)。
- [Office 2013 策略](https://technet.microsoft.com/library/cc178969.aspx)（包括[如何自定义 Office 工具栏](https://technet.microsoft.com/library/cc179143.aspx)）。
 
