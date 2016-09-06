---
ms.openlocfilehash: 26917ccec9279888574fc925dc539f00effa57db
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="在 Azure RemoteApp 模板图像是什么？"
    description="了解有关附带 Azure RemoteApp 的模板图像。"
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/02/2015" 
    ms.author="elizapo" />

# 在 Azure RemoteApp 模板图像是什么？

Azure RemoteApp 订阅包括三个模板图像︰


- Windows Server 2012
- Microsoft Office 365 ProPlus （所需的 Office 365 订阅）
- Microsoft Office 2013年专业加号 （仅用于试验）

> [AZURE.IMPORTANT]Azure RemoteApp 订阅授予您访问在图像中，除了 Office 365 ProPlus，这就需要单独订购，和办公室 2013，不能在生产中使用的软件。 这意味着可以使用您的用户共享的程序或模板图像的应用程序。 例如，如果您创建一个集合，它使用 Windows Server 2012 R2 图像，您可以发布系统中心端点保护的用户通过远程应用程序访问。
> 
> 检查详细信息的[远程应用程序授权的详细信息](remoteapp-licensing.md)。 并[使用 Azure RemoteApp 办公室](remoteapp-o365.md)办公室的授权信息。

阅读有关每个图像所包含的内容的详细信息。

## Windows Server 2012 R2 （"香草映像"）
此图像基于 Microsoft Windows Server 2012 R2 数据中心操作系统，并具有下列角色和功能安装要符合 Azure RemoteApp 模板图像的要求︰


- .NET framework 4.5，3.5.1、 3.5
- 桌面体验
- 墨迹和手写服务
- 媒体基础
- 远程桌面会话主机
- Windows PowerShell 4.0
- Windows PowerShell ISE
- WoW64 支持

此图像也有安装下列应用程序︰

- Adobe 的 Flash 播放器
- Microsoft Silverlight
- Microsoft System Center 2012 端点保护
- Microsoft Windows Media Player


## Microsoft Office 365 ProPlus （订阅所需）
Office 365 是最常请求的应用程序，因此我们创建了您要使用的"自定义"图像。

该图像是香草图像的扩展，并具有 Microsoft Office 365 ProPlus 除了 Windows Server 2012 R2 图像中描述的组件安装的下列组件︰


- Access
- Excel
- Lync
- OneNote
- OneDrive 为公司的
- Outlook
- PowerPoint
- 单词
- Microsoft Office 校对工具

图像还包含 Visio Pro 和专业项目。

并且以下应用程序，以及︰

- SQL 本机客户端
- ODBC 驱动程序
- SQL Server 数据挖掘客户端
- MasterDataServices 客户端
- Microsoft 发布服务器
- PowerQuery
- PowerMap


Office 365 ProPlus 应用程序的全部功能是仅适用于具有 Office 365 ProPlus 计划的用户。 Office 365 中的更多详细信息的订阅计划请参阅[Office 365 提供服务计划](http://technet.microsoft.com/library/office-365-plan-options.aspx)。 还有问题吗？ 检查出[Office 365 + 远程应用程序](remoteapp-o365.md)的信息。 此外检查出新的文章，[如何使用 Azure RemoteApp Office 365 订阅](remoteapp-officesubscription.md)。

请注意，您需要单独的许可证 Office 365 ProPlus、 Visio Pro 和专业项目它们各有自己的许可证。

## Microsoft Office 2013年专业加号 （仅用于试验）
在免费的试用期间，您可以测试与 Office 2013 图像服务。

此图像是香草图像的扩展，除了 Windows Server 2012 R2 图像中描述的组件安装的 Microsoft Office 2013年专业加号的下列组件︰


- Access
- Excel
- Lync
- OneNote
- OneDrive 为公司的
- Outlook
- PowerPoint
- Project
- Visio
- 单词
- Microsoft Office 校对工具

> [AZURE.IMPORTANT]**法律信息︰**此图像不包含 Microsoft Office 许可证和*不能用于生产*。 图像设计只用于试用版 Office 2013 专业加号。 如果您想要为生产 Azure 远程应用程序在使用 Office 应用程序，您需要使用 Office 365 ProPlus 图像。 有关授权机构的详细信息，请参阅[使用 Azure RemoteApp 的 Office 365 提供](remoteapp-o365.md)
 