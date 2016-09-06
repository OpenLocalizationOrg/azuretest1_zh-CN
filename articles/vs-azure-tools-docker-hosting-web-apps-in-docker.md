---
ms.openlocfilehash: 1abadff1d338696a0e559f30df376e523f767620
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="承载 Docker 在 Web 应用程序 |Microsoft Azure"
   description="了解如何使用 Visual Studio Docker 容器中的 web 应用程序的宿主。"
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

# 在 Docker 承载 Web 应用程序

[Docker](https://www.docker.com/whatisdocker/)是一个轻量级容器的引擎，在某些方面与虚拟机，您可以使用宿主应用程序和服务相似。 Visual Studio 支持 Docker Ubuntu，CoreOS 和 Windows 上。

本示例演示如何使用 Visual Studio 2015年工具 Docker 扩展 Docker 扩展名与 ASP.NET 5 web 应用程序一起安装，将 ASP.NET 5 应用程序发布到 Ubuntu Linux 虚拟机 （这里称为 Docker 主机） 在 Azure 上。 用于发布到 Windows 容器相同的体验。

您可以使用**自定义主机**设置，到新 Docker 主机位于 Azure，或给后端服务器、 Hyper-V 或 Boot2Docker 主机发布应用程序。 在发布后您的应用程序到 Docker 主机，可以使用 Docker 命令行工具与您的应用程序发布到的容器进行交互。

## 创建和发布新 Docker 容器

在这些过程中，将创建新的 ASP.NET 5 web 应用程序项目、 创建容器主机，然后生成和 Docker 容器中运行的 web 应用程序项目。 若要开始，请下载并安装[Visual Studio 2015 Docker 工具](https://aka.ms/DockerToolsForVS)。

### 添加 ASP.NET 5 web 应用程序项目

1. 创建新的 ASP.NET web 应用程序项目。 在主菜单中，选择**文件**，**新的项目**。 在**C#**，**网站**，选择**ASP.NET Web 应用程序**。

1. 在**ASP.NET 5 预览模板**列表中，选择**Web 站点**。

1. 由于 web 应用程序将在 Docker 承载/运行，清除**在云环境中的主机**复选框，如果它处于选中状态，然后选择**确定**按钮。

  ![][0]

  这点在您要通常将代码添加到 web 应用程序，使其做某事很有用，但对于此示例，让我们只是将其保留为其缺省设置。 （请注意，您还可以选择使用现有 ASP.NET 5 web 应用程序）。

### 发布项目

1. 在 ASP.NET 项目的上下文菜单中选择**发布**。

1. 在**发布网站**对话框中的**选择发布目标**部分中，选择**Docker 容器**按钮。

    如果您看不到 Docker 容器选项，请确保您安装了 Visual Studio 的 2015年工具 Docker 和您在前面的过程中选择 ASP.NET 5 Web 站点模板。

    ![][1]

    出现**选择 Docker 虚拟机**对话框。 这允许您指定要在其中发布项目 Docker 主机。 您可以创建新 Docker 主机或选择现有的虚拟机承载在 Azure 上或其他地方。 以后在此示例中，我们将创建一个新的 Azure Docker 主机。

1. 如果您已经登录到 Azure 帐户，请跳到步骤 5。 如果您不在登录到的帐户，请选择**添加帐户**按钮。

    ![][2]

1. 在**Visual Studio 在登录**对话框中，输入 Azure 订阅的电子邮件帐户，然后选择**继续**按钮。

1. 选择**新建**按钮创建新的 Azure Docker 虚拟机，然后选择**确定**按钮。

    ![][3]

    注意您还可以选择使用现有 Docker 主机。 要做到这一点，**现有的 Azure Docker 虚拟计算机**下拉列表中选择它而不是选择**新建**按钮。 此列表不能显示只容器主机，它列出了所有您 Azure 的租户中的虚拟机。

    作为一种替代方法，您可以选择将发布到自定义 Docker 主机。 稍后在本主题的详细信息，请参阅**提供自定义 Docker 主机**。

1. 在**创建 Microsoft Azure 上的虚拟机**对话框中输入以下信息。 当您完成时，选择**确定**按钮。 这将创建 Linux 虚拟机配置 Docker 扩展名。

    ![][4]

    请注意，您现在还可以创建 Windows 容器主机使用 Windows 服务器 2016年技术预览 3 (TP3) 的选择。

    ![][8]

  	|属性名称|设置|
  	|---|---|
  	|位置|将此设置更改为靠近您的区域设置的区域。|
  	|DNS 名称|输入该虚拟机的唯一名称。 如果通过 Azure 接受该名称，则右侧会出现绿色圆的白色复选标记。 如果名称将不被接受，将出现一个带白色 x 的红色圆圈。 在这种情况下，输入新的唯一名称。|
  	|Image|如果有，请选择用于 Docker 主机 OS 映像。 对于此示例，选择 Ubuntu 服务器映像。 （请注意目前可用的图像列表中的可用的 Windows 服务器映像）。|
  	|Username|输入该虚拟机的唯一的用户名称。|
  	|密码|输入用户的密码，然后确认它。|
  	|证书目录 |此参数指定 Docker 证书的存储位置的文件夹。 虽然您可以创建一个新文件夹，或指向现有文件夹，建议使用默认的证书文件夹 (c:\\用户\\[*用户名*]\\.docker)。 否则，身份验证选项无法自动检索如果重复使用相同的主机上另一个项目或系统。|

1. 选择**证书目录**条目旁边的省略号 （...） 按钮然后 Docker 证书，为创建新文件夹，或定位到现有 Docker 证书文件夹。

    如果 Docker 证书所需的 VM 不找到，惊叫图标旁边有条目，让您知道，无法找到所需的证书，然后继续将删除并重新创建任何现有证书。

1. 选择**确定**按钮以开始创建虚拟机。 您将收到一条消息，该虚拟机创建在 Azure。

    Visual Studio 创建 Azure 资源管理器 (ARM) 模板文件、 参数文件和 PowerShell 脚本，以便可以在以后再次执行命令。

    ![][7]

    Visual Studio 将输出到**输出**窗口操作的进度。 Visual Studio 调用 PowerShell 脚本来部署虚拟机。 该脚本使用 Azure PowerShell cmdlet 部署 Azure 资源组。 之后，另一个 PowerShell 脚本使用问题 Docker 命令发布，就像如果您手动创建 Docker 主机。

    资源调配 Docker 主机可以需要一段时间，因此请查看在输出窗口以查看该作业完成时的状态。

1. Docker 主机完全设置 Azure 中后，您可以检查您的帐户在 Azure 的门户网站上。 您应该可以在 Azure 的门户网站上看到新的虚拟机的**虚拟机**类别下。

1. 一旦准备就绪 Docker 主机后，回过头来发布该 web 应用程序项目。 在**解决方案资源管理器**中的 web 应用程序项目节点的上下文菜单，选择**发布**。 Visual Studio 创建基于的 VM 创建一个发布文件。

1. **发布 Web**对话框中**连接**选项卡上，选择**验证连接**框中，以确保 Docker 主机已准备就绪。 如果连接不好，选择发布该 web 应用程序**发布**按钮。

    第一次将应用程序发布到 Docker 主机，它将花费时间下载的任何基 Docker 文件 （如**FROM** *imagename*) 中引用的图像。

    请注意 Docker 文件是特定于操作系统。 如果您选择重新发布到不同的操作系统，您需要重命名 Docker 文件，Visual Studio 可以创建新的基于目标操作系统默认。 例如，如果第一次发布到 Linux 容器而稍后将发布到 Windows，您应文件重命名 Docker 为一个唯一的名称，如 DockerLinux。 然后，当重新发布到 Windows 时，Visual studio 会重新创建默认 Docker 文件的窗口。 以后，当您重新发布之一，只需选择相应 Docker 文件所针对的 OS。

## 提供自定义 Docker 主机

上述过程有您创建托管在 Azure 上 Docker 虚拟机。 但是，如果您已经在其他地方有现有 Docker 主机，您可以选择要发布到它而不是 Azure。

### 如何提供自定义 Docker 主机

1. 在**选择 Docker 虚拟机**对话框中，选择**自定义 Docker 主机**复选框。

    ![][5]

1. 选择**确定**按钮。

1. 在**发布网站**对话框中，将值添加到**CustomDockerHost**部分中的设置如︰ 服务器 URL、 映像名称、 Dockerfile 位置和主机和容器端口号。

    在**Docker 高级选项**部分中，您可以查看或更改身份验证和运行选项，以及 Docker 命令行。

    ![][6]

1. 您已输入所有所需的值后，选择**验证连接**按钮，以确保正常连接到 Docker 主机的工作原理。

1. 如果连接正常工作，您可以选择**下一步**按钮以将发布组件的列表，请参阅或您可以选择**发布**按钮立即发布该项目。

## 测试 Docker 宿主

既然在 Azure 上 Docker 主机已发布项目，您可以通过检查其设置测试它。 因为 Docker 命令行工具安装的 Visual Studio 扩展，可以直接从 Windows 命令提示符 Docker 到发出命令。

下面的过程是与已经部署到 Azure Docker 主机进行通信。

### 如何测试 Docker 主机

1. 打开 Windows 命令提示符。

1. 分配 Docker 主机，并验证环境变量。 若要执行此操作，请在命令提示符下输入以下命令。 （替代您 Docker 主机的名称为*NameofAzureVM*）。

    ```
    Set DOCKER_HOST=tcp://<NameofAzureVM>.cloudapp.net:2376
    Set DOCKER_TLS_VERIFY=1
    ```

    调用这些命令禁止无需添加`–H (Host) tcp://<NameofAzureVM>.cloudapp.net:2376`和`--TLSVERIFY`为您颁发的每个命令。

1. 现在您可以发出类似的命令测试 Docker 主机是否存在并正常工作。 

  	|命令行|说明|
  	|---|---|
  	|`docker info`|获取 Docker 版本信息。|
  	|`docker ps`|获取正在运行的容器的列表。|
  	|`docker ps –a`|获取列表的容器，包括那些已停止。|
  	|`docker logs <Docker container name>`|获取日志指定的容器。|
  	|`docker images`|获取映像的列表。|

    Docker 命令的完整列表，只需输入命令`docker`在命令提示符处。 有关详细信息，请参阅[Docker 命令行](https://docs.docker.com/reference/commandline/cli/)。

## 下一步行动

既然有 Docker 主机，您可以向它发出 Docker 命令。 若要了解有关 Docker 的详细信息，请参阅[Docker 文档](https://docs.docker.com/)和[Docker 在线教程](https://www.docker.com/tryit/)。

若要了解如何使用 Linux 在 Azure 上 Docker VM 扩展，请参阅[在 Azure 上 linux Docker 虚拟机扩展](virtual-machines-docker-vm-extension.md)。

与在 Visual Studio 中使用 Docker 问题，请参阅[Windows 使用 Visual Studio 在故障排除 Docker 客户端错误](vs-azure-tools-docker-troubleshooting-docker-errors.md)。

[0]: ./media/vs-azure-tools-docker-hosting-web-apps-in-docker/IC796678.png
[1]: ./media/vs-azure-tools-docker-hosting-web-apps-in-docker/IC796679.png
[2]: ./media/vs-azure-tools-docker-hosting-web-apps-in-docker/IC796680.png
[3]: ./media/vs-azure-tools-docker-hosting-web-apps-in-docker/IC796681.png
[4]: ./media/vs-azure-tools-docker-hosting-web-apps-in-docker/IC796682.png
[5]: ./media/vs-azure-tools-docker-hosting-web-apps-in-docker/IC796683.png
[6]: ./media/vs-azure-tools-docker-hosting-web-apps-in-docker/IC796684.png
[7]: ./media/vs-azure-tools-docker-hosting-web-apps-in-docker/IC796685.png
[8]: ./media/vs-azure-tools-docker-hosting-web-apps-in-docker/IC796686.png

测试
