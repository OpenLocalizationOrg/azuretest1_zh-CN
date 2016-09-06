---
ms.openlocfilehash: 54e0dd6a6f67a936d7b2991e8dc43ce668bd1d20
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="如何将项目升级到当前版本的 Azure 的工具"
   description="了解如何在 Visual Studio 中的 Azure 项目升级到当前版本的 Azure 的工具"
   services="visual-studio-online"
   documentationCenter="na"
   authors="kempb"
   manager="douge"
   editor="tglee" />
<tags 
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/13/2015"
   ms.author="kempb" />

# 如何升级到 Azure 工具的当前版本的 Visual Studio 的项目

##概述

通过使用 Azure 工具创建的任何项目安装当前版本的 Azure 工具 （或以前的版本比 1.6 更新） 后，松开之前 1.6 （2011 年 11 月） 将自动升级时立即打开它们。 如果通过使用这些工具的 1.6 （2011 年 11 月） 版本创建项目，您仍然必须安装该版本，可以在较早的版本中打开这些项目，稍后决定是否升级它们。

## 您在升级时，如何更改您的项目

自动升级项目或您指定您想要升级它，如果您的项目进行修改以便与当前版本的某些程序集，并也更改某些属性，如本节中所述。 如果您的项目需要能够使用工具的新版本兼容的其他更改，您必须手动进行这些更改。

- 更新 web.config 文件的 web 角色和辅助角色的 app.config 文件来引用 Microsoft.WindowsAzure.Diagnostics.DiagnosticMonitoirTraceListener.dll 的较新版本。

- Microsoft.WindowsAzure.StorageClient.dll、 Microsoft.WindowsAzure.Diagnostics.dll 和 Microsoft.WindowsAzure.ServiceRuntime.dll 程序集将被升级到最新版本。

- 存储在 Azure 项目文件 (.ccproj) 的发布配置文件被移动到一个单独的文件，扩展名.azurePubXml，**发布**子目录中。

- 发布配置文件中的某些属性将更新，以支持新的和更改的功能。 因为您可以同时或增量更新已部署的云服务，将由**DeploymentReplacementMethod**替换**AllowUpgrade** 。

- 添加**UseIISExpressByDefault**的属性并设置为 false，以便用于调试的 web 服务器不会自动更改为 IIS Express 的从 Internet Information Services (IIS)。 IIS Express 是默认的 web 服务器会使用最新版本的工具创建的项目。

- 如果一个或多个项目的角色中承载了 Azure 缓存，升级项目时，会更改服务配置 （.cscfg 文件） 和服务定义 （.csdef 文件） 中的某些属性。 如果项目使用 Azure 缓存 NuGet 程序包，项目升级到最新版本的软件包。 您应打开 web.config 文件并验证客户端配置已正确维护升级过程。 如果您不使用 NuGet 程序包添加 Azure 缓存客户端程序集的引用，则不会更新这些程序集;您必须手动更新这些引用到最新版本。

>[AZURE.IMPORTANT] 对于 F# 的项目，您必须手动更新，以便它们引用这些程序集的新版本 Azure 的程序集的引用。

### 如何升级到当前版本的 Azure 项目

1. 将 Azure 工具的当前版本安装到您想要升级的项目中，使用的 Visual Studio 安装，然后打开您想要升级的项目。 如果该项目创建使用 Azure 工具释放之前 1.6 （2011 年 11 月），该项目会自动升级到当前版本。 如果创建项目时与 2011 年 11 月发布和还安装了该版本，该版本中打开该项目。

1. 在解决方案资源管理器，打开项目节点的快捷菜单，选择**属性**，然后选择显示的对话框中的**应用程序**选项卡上。 

    **应用程序**选项卡显示与该项目相关联的工具版本。 如果出现 Azure 工具的当前版本，该项目已经升级。 如果您已经安装了比选项卡的显示较新版本的工具，会出现**升级**按钮。

1. 选择**升级**按钮以将项目升级到当前版本的工具。

1. 生成项目时，，然后解决 API 更改所引发的任何错误。 有关如何修改您的代码最新版本的信息，请参阅特定的 API 的文档。

## 资源

[Azure 的工具的版本历史记录]( http://go.microsoft.com/fwlink/p/?LinkId=623548)


测试
