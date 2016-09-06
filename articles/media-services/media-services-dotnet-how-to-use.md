---
ms.openlocfilehash: 37f8498d2fbdd670614650edbfdcdde7df12d355
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="如何将计算机设置为介质服务使用.NET 的开发" 
    description="了解如何使用.NET 媒体服务 SDK 的媒体服务的先决条件。 此外学习如何创建一个 Visual Studio 应用程序。" 
    services="media-services" 
    documentationCenter="" 
    authors="juliako" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="08/11/2015"
    ms.author="juliako"/>

#介质服务使用.NET 的开发 

[AZURE.INCLUDE [media-services-selector-setup](../../includes/media-services-selector-setup.md)]

本主题讨论如何启动开发媒体服务应用程序使用.NET。 

**Azure 媒体服务.NET SDK**库使您能够对介质服务使用.NET 编程。 为了使其更容易地使用.NET 开发，提供了**Azure 媒体服务.NET SDK 扩展**库。 此库包含一组扩展方法和简化.NET 代码的 helper 函数。 两个库都可以通过**NuGet**和**GitHub**。
 

##先决条件

-   在新的或现有的 Azure 订阅媒体服务帐户。 请参阅[如何创建介质服务帐户](media-services-create-account.md)主题。
-   操作系统︰ Windows 7，Windows 2008 R2 或 Windows 8。
-   .NET framework 4.5。
-   Visual Studio 2013年，Visual Studio 2012 或 Visual Studio 2010 SP1 （专业、 特优、 终极，或速成版）。 
  

##创建和配置 Visual Studio 项目 

这一节演示了如何在 Visual Studio 中创建一个项目并将其设置为媒体服务的开发。  在这种情况下，项目是 C# Windows 控制台应用程序，但此处所示的相同安装步骤适用于其他类型的项目，您可以创建的介质服务应用程序 （例如，Windows 窗体应用程序或一个 ASP.NET Web 应用程序）。

本节说明如何使用**NuGet**添加介质服务.NET SDK 和其他相关的库。 

或者，可以从 GitHub （[github.com/Azure/azure-sdk-for-media-services](https://github.com/Azure/azure-sdk-for-media-services)和[github.com/Azure/azure-sdk-for-media-services-extensions](https://github.com/Azure/azure-sdk-for-media-services-extensions)） 获取最新的媒体服务.NET SDK 位、 生成解决方案，并将引用添加到该客户端项目。 请注意，所需的所有依赖项获取下载自动提取。   

1. 在 Visual Studio 2013年，Visual Studio 2012 或 Visual Studio 2010 SP1 中创建一个新 C# 控制台应用程序。 输入**名称**、**位置**和**解决方案名称**，然后单击确定。 

2. 生成解决方案。

2. **NuGet**可用于安装和添加**Azure 媒体服务.NET SDK 扩展**。 安装此软件包，也将安装**介质服务.NET SDK**并添加其他所需的所有依赖项。
    1. 确保您有 NuGet 安装最新版本。 有关详细信息和安装说明，请参阅[NuGet](http://nuget.codeplex.com/)。
    
    2. 在解决方案资源管理器中右击项目名称，并选择管理 NuGet 程序包...
    
        出现管理 NuGet 程序包对话框。

    3. 在联机库 Azure MediaServices 扩展搜索，选择 Azure 媒体服务.NET SDK 扩展，然后单击安装按钮。
 
        修改项目时，对媒体服务.NET SDK 扩展、 媒体服务.NET SDK 和其他依赖程序集的引用添加。

    4. 为提升清洗器的开发环境，可考虑 NuGet 程序包还原。 有关详细信息，请参阅[NuGet 程序包还原"](http://docs.nuget.org/consume/package-restore)。

3. 添加**System.Configuratio**n 程序集的引用。 此程序集包含 System.Configuration。**ConfigurationManager**类，用于访问配置文件 (例如，App.config)。 

    若要添加引用使用管理引用对话框中，执行下列操作︰ 

    1. 在解决方案资源管理器中，右击项目名称。 然后，选择添加和引用。

        此时将显示管理引用对话框。

    2. 在.NET framework 程序集，查找并选择 System.Configuration 程序集。
    3. 按确定。


4. 打开 App.config 文件 （如果未添加默认情况下，则将文件添加到您的项目中） 并将*appSettings*部分添加到该文件。 设置 Azure 媒体服务帐户的名称和帐户键，值，如下面的示例中所示。 
    
    获取的**帐户名**和**帐户密钥**信息，打开**Azure 管理门户**、 选择介质服务帐户并单击**管理密钥**按钮。


    <pre><code>
    &lt;configuration&gt;
        &lt;appSettings&gt;
        &lt;add key="MediaServicesAccountName" value="Media-Services-Account-Name" /&gt;
            &lt;add key="MediaServicesAccountKey" value="Media-Services-Account-Key" /&gt;
        &lt;/appSettings&gt;
    &lt;/configuration&gt;
    </code></pre>


5. 覆盖现有的 Program.cs 文件中使用以下代码的开头使用语句。

        using System;
        using System.Collections.Generic;
        using System.Linq;
        using System.Text;
        using System.Threading.Tasks;
        using System.Configuration;
        using System.Threading;
        using System.IO;
        using Microsoft.WindowsAzure.MediaServices.Client;

在这种情况下，现在可以开始开发媒体服务应用程序。    
 
测试
