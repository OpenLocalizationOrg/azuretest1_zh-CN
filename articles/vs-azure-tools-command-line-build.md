---
ms.openlocfilehash: 2ee8c456b1c9aef0078e95197d4b3b2d0d5797ce
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="Azure 用于命令行生成"
   description="Azure 用于命令行生成"
   services="visual-studio-online"
   documentationCenter="na"
   authors="kempb"
   manager="douge"
   editor="tlee" />
<tags 
   ms.service="multiple"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/24/2015"
   ms.author="kempb" />

# Azure 用于命令行生成

##概述

您可以通过在命令提示符处运行 MSBuild 创建 Azure 的部署包。 您可以配置并定义生成有关调试、 临时和生产中，除了一些生成过程的自动化。


## Microsoft 生成引擎 (MSBuild)

通过使用 Microsoft 构建引擎 (MSBuild)，可以生成中生成的产品安装 Visual Studio 不的实验室环境。 MSBuild 项目文件的扩展和完全受 Microsoft 使用的 XML 格式。 在这种文件格式，您可以描述项目必须是针对一个或多个平台和配置生成。

您还可以在命令提示符下，运行 MSBuild，本主题描述了这种方法。 通过在命令提示符下设置属性，您可以构建一个项目的特定配置。 同样，您还可以定义 MSBuild 命令将构建的目标。 有关命令行参数和 MSBuild 的详细信息，请参阅[MSBuild 命令行参考](https://msdn.microsoft.com/library/ms164311.aspx)。

## 安装

下面的过程所述，必须安装软件和工具在生成服务器上创建使用 MSBuild Azure 包之前︰

1. 安装.NET Framework 4 或更高版本，其中包含 MSBuild。

1. 安装[Azure 创作工具](http://go.microsoft.com/fwlink/?LinkId=394615)（MicrosoftAzureAuthoringTools-x64.msi 或 MicrosoftAzureAuthoringTools x86.msi 的寻找。

1. 安装[.NET 的 Azure 库](http://go.microsoft.com/fwlink/?LinkId=394616)（MicrosoftAzureLibsForNet-x64.msi 或 MicrosoftAzureLibs x86.msi 的寻找。

1. 将 Microsoft.WebApplication.targets 文件复制从另一台计算机上安装 Visual Studio。 

    文件所在目录的文件 C:\Program (x86) \MSBuild\Microsoft\Visual Studio\v12.0\WebApplications (v11.0 为 Visual Studio 2012)，并应将其复制到生成服务器上的同一目录。

1. 安装[Visual Studio Azure 的工具](http://go.microsoft.com/fwlink/?LinkId=394616)。 

    WindowsAzureTools.vs120.exe 以生成 Visual Studio 2013年项目的外观。

## MSBuild 参数

要创建对象包的最简单方法就是运行 MSBuild 以`/t:Publish`选项。 默认情况下，此命令将创建的目录相对于根文件夹中的项目，如 ProjectDir\bin\Configuration\app.publish\.在 Azure 项目生成时，将生成两个文件、 包文件本身和附带的配置文件︰

- Project.cspkg

- ServiceConfiguration.TargetProfile.cscfg

默认情况下，每个 Azure 项目包括一个服务配置文件的本地 （调试） 生成，另一个用于云 （临时或生产） 的生成，但您可以添加或删除服务配置文件，根据需要。 生成在 Visual Studio 中的包时，将要求您的服务配置文件包含包的旁边。 通过使用 MSBuild 生成包时，默认情况下包含本地的服务配置文件。 若要将不同的服务配置文件中，将`TargetProfile`MSBuild 命令属性 (`MSBuild /t:Publish /p:TargetProfile=ProfileName`)。

如果您想要使用的已存储的包和配置文件的备用目录，通过设置路径`/p:PublishDir=Directory\`选项，其中包括尾部的反斜杠分隔符。

## 部署

在生成软件包之后，可以将其部署到 Azure。 有关演示该过程的教程，请参阅 Azure 网站。 有关如何自动执行此过程，请参阅[在 Azure 的云服务不断提供](../cloud-services/cloud-services-dotnet-continuous-delivery)信息。


测试
