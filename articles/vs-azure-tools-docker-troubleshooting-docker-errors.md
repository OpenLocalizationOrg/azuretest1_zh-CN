---
ms.openlocfilehash: 657c261378e1c5bb3cde82d45c6469202fa51183
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="故障排除 Docker 使用 Visual Studio 的 Windows 上的客户端错误 |Microsoft Azure"
   description="解决使用 Visual Studio 创建并使用 Visual Studio 将 web 应用程序部署到 Windows 上 Docker 时遇到的问题。"
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
   ms.date="08/20/2015"
   ms.author="kempb" />

# Docker 错误疑难解答

您配置的所有设置为您的应用程序 Docker 容器后，应确保设置和路径正确。 Visual Studio 提供了一个验证按钮，在发布对话框可帮助您做到这一点。

此主题可以帮助您诊断和修复或变通办法承载 Docker 在 Visual Studio 应用程序时，才会遇到的最常见问题。 更多的问题将被添加到本主题中，所遇到的这些。

## 当您尝试验证的连接到您 Docker 主机上的发布网站对话框中获取失败的邮件

下面是此问题某些可能的解决方案。

- 在**发布**对话框中**连接**选项卡上，请确保该**服务器的 Url**是否正确，尾随`:<port_number>`的**服务器 URL**是 Docker 守护进程在侦听的端口。

- 在**发布**对话框中的**连接**选项卡中，展开**Docker 高级选项**部分，确保指定了正确的**身份验证**选项。
  - 如果 Docker 守护进程在服务器上的已配置为使用 TLS 安全性，则外观 (key.pem) 的客户端密钥和证书 (cert.pem)，默认情况下 Windows Docker 命令行界面 (docker.exe)`<%userprofile%>\.docker`文件夹。 如果这些项目没有它们需要使用 OpenSSL 生成。 有关配置 tls Docker 的详细信息，请参阅[保护 Docker 守护程序使用 HTTPS 的套接字](https://docs.docker.com/articles/https/)。

    确保 Docker 是否正常的一种方法向 Linux 服务器验证来自 Windows 客户端是通过预览文本框中的内容复制到一个新的命令窗口并更改`<command>`到"信息"，如以下所示︰

    ```
    // This example assumes the Docker daemon is configured to use the default port
    // of 2376 to listen for connections.docker.

    --tls -H tcp://contoso.cloudapp.net:2376 info
    ```

    作为复制到.docker 文件夹的客户端证书和密钥文件的替代方法，您可以通过添加以下参数更改**身份验证**选项︰

    ```
    --tls --tlscert=C:\mycert\cert.pem --tlskey=C:\mycert\key.pem
    ```
- 请确保 Docker 主机上的 Docker 守护进程是 1.6 或更高版本。

## 在 Docker 文件夹中使用您自己的证书没有客户端证书时的超时错误

如果您选择使用您自己的证书，当在 Visual Studio 中创建 Docker 主机 （即清除**Microsoft Azure 上创建虚拟机**对话框中的**自动生成 Docker 证书**复选框），您将需要将客户端证书和密钥文件 （cert.pem 和 key.pem） 复制到 Docker 文件夹 (`<%userprofile%>\.docker`)。 否则为发布您的项目时，您将获得一个小时的时间超时错误，该发布操作会失败。

## 发布到 Docker 容器所需的 PowerShell 3.0

如果您的操作系统是 Windows 7 或 Windows Server 2008，您需要安装 PowerShell 3.0 才能发布到 Docker 容器。 PowerShell 3.0 包含在[Windows 管理框架 3.0](https://www.microsoft.com/en-us/download/details.aspx?id=34595)。 您需要在安装后重新启动系统。

作为替代的解决办法，您可以升级到 Windows 8.1 或 Windows 10，其中已经 PowerShell 3.0。

## PowerShell 窗口不会自动关闭

在创建虚拟机之后, 有时 PowerShell 窗口不会关闭自动。 关闭此窗口也会关闭 Visual Studio。 因为窗口不会影响任何 Visual Studio 或 Docker 工具的功能，请保持打开状态直到您完成您的工作。

## 常见问题

问︰ 如何在 Azure 使用 Visual Studio 工具创建一个新的 Linux Docker 启用机？

答︰ 有关如何执行此操作的信息，请参阅[在 Docker 承载 Web 应用程序](vs-azure-tools-docker-hosting-web-apps-in-docker.md)。

问︰ 哪些 Visual Studio 项目模板发布到 Linux Docker 容器支持？

答︰ Visual Studio 当前支持的 C# 控制台应用程序 （封装） 和 C# ASP.NET 5 预览 web 模板，其中包括︰

- 空

- API web

- Web 应用程序

问︰ 如何将 ASP.NET 5 web 或控制台项目发布到从命令行使用 MSBUILD Docker？

答︰ 请使用下面的 MSBuild 命令︰

    `msbuild <projectname.xproj> /p:deployOnBuild=true;publishProfile=<profilename>`

问︰ 如何将 ASP.NET 5 web 或控制台项目发布到从命令行使用 PowerShell Docker？

答︰ 使用以下 PowerShell 命令︰

```
.\contoso-Docker-publish.ps1 -packOutput $env:USERPROFILE\AppData\Local\Temp\PublishTemp -pubxmlFile .\contoso-Docker.pubxml
```

问︰ 我有我自己的 Linux 服务器与安装 Docker，如何指定这在**发布 Web**对话框？

答︰ 请参见主题[中 Docker 承载 Web 应用程序](vs-azure-tools-docker-hosting-web-apps-in-docker.md)中的节**提供自定义 Docker 主机**。

问︰ 我正在与 Docker 安装使用我自己的 Linux 服务器。 如何才能配置使用 TLS 身份验证生成密钥和证书？

答︰ 一种方法是在服务器上使用 OpenSSL 来为 CA、 服务器和客户端生成所需的证书和密钥。 可以使用第三方软件建立 SSH/SFTP 连接，然后将证书复制到本地计算机的 Windows 开发。 默认情况下，(CLI) Docker 将尝试使用证书位于`<userprofile>\.docker`文件夹。

另一种方法是下载 Windows 的 OpenSSL 并生成所需的证书和密钥，并将 CA、 服务器证书和密钥的 Linux 计算机上载。 建立到 Docker 安全连接的详细信息，请参阅[保护 Docker 守护程序使用 HTTPS 的套接字](https://docs.docker.com/articles/https/)。

测试
