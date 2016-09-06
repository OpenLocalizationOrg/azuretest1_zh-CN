---
ms.openlocfilehash: 53c4a8c37cf30dc9aa78b2092c209952631de763
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="如何使用 Ubuntu Docker VM 映像快速 Docker"
    description="描述并演示如何使用 Docker Ubuntu 服务器上以分钟为单位直接从 Azure 图像库"
    services="virtual-machines"
    documentationCenter=""
    authors="squillace"
    manager="timlt"
    editor="tysonn"/>

<tags
    ms.service="virtual-machines"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-linux"
    ms.workload="infrastructure"
    ms.date="05/20/2015"
    ms.author="rasquill"/>

# 如何快速开始使用 Azure 市场 Docker

若要开始使用[Docker]的最快方法是转到 Azure 市场并创建虚拟机使用与[MSOpenTech]一起创建的[Canonical] **Docker Ubuntu 服务器上的**映像模板。 这将 Ubuntu 服务器虚拟机并自动安装和预安装的并且在 Azure 上运行**最新**的 Docker 引擎[Docker VM 扩展](virtual-machines-docker-vm-extension.md)。  

您立即可以连接到 VM 使用 SSH 并开始工作 Docker 直接不执行任何其他操作。

> [AZURE.NOTE]通过 Azure 市场模板创建虚拟机不能主动 Docker docker 远程客户端管理远程 API。 若要启用远程控制 Docker 主机在此 VM，看到或者[使用 HTTPS 运行 Docker](https://docs.docker.com/articles/https/)或执行中[使用从 Azure 门户 Docker VM 扩展](virtual-machines-docker-with-portal.md)或[使用 Azure CLI Docker VM 扩展](virtual-machines-docker-with-xplat-cli.md)的步骤。 如果您感到尤其是客，您可以从 GitHub 构建[Windows Docker 客户端](https://github.com/ahmetalpbalkan/Docker.DotNet)再试，也 （或刚从[nuget](https://www.nuget.org/packages/Docker.DotNet/)拿去）。

本主题︰

- [登录到门户网站]
- [从规范 Docker 图像和 MSOpenTech 创建虚拟机]
- [用 SSH 连接和乐趣]

## <a id='logon'>登录到门户网站</a>

除非您没有一个 Azure 帐户，这就是很容易的。 [获取一个空闲轻松，太](http://azure.microsoft.com/pricing/free-trial/)！

## <a id='createvm'>从规范 Docker 图像和 MSOpenTech 创建虚拟机</a>

1. 现在，在您登录后，单击**新建**左下角来创建新的虚拟机映像中。 您可能会立即看到标题栏中的相应图像︰

> ![在标题栏中选择 Docker Ubuntu 图像](./media/virtual-machines-docker-ubuntu-quickstart/CreateNewDockerBanner.png)

2. 但是，如果不是，它在搜索图像库︰

> ![在映像库中找到的图像](./media/virtual-machines-docker-ubuntu-quickstart/DockerOnUbuntuServerMSOpenTech.png)

3. 提供的用户名和密码的实例或启用 SSH 使用证书的**.pem**文件。 (下面指定的用户名和密码组合的图形演示。)在底部，然后按**创建**。

> ![若要配置 vm 实例](./media/virtual-machines-docker-ubuntu-quickstart/CreateVMDockerUbuntuPwd.png)

4. 并等待，直到它在运行。 它不应该花很长时间在所有。

> ![在门户网站中运行 docker 图像](./media/virtual-machines-docker-ubuntu-quickstart/DockerUbuntuRunning.png)

## <a id='havingfun'>用 SSH 连接和乐趣</a>

现在的乐趣开始。 您可以立即连接到 VM 使用 SSH:

> ![用 SSH 连接](./media/virtual-machines-docker-ubuntu-quickstart/SSHToDockerUbuntu.png)

开始发出 Docker 命令，记住，在此 Azure VM 的默认配置要求和**`sudo`**:

> ![取出图像](./media/virtual-machines-docker-ubuntu-quickstart/DockerPullSmallImages.png)

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## 下一步行动

您会想要开始使用[Docker]！

<!--Anchors-->
[登录到门户网站]: #logon
[从规范 Docker 图像和 MSOpenTech 创建虚拟机]: #createvm
[用 SSH 连接和乐趣]: #havingfun
[下一步行动]: #next-steps


[Docker]: https://www.docker.com/
[BusyBox]: http://en.wikipedia.org/wiki/BusyBox
[Docker 草稿图像]: https://docs.docker.com/articles/baseimages/#creating-a-simple-base-image-using-scratch
[规范]: http://www.canonical.com/
[MSOpenTech]: http://msopentech.com/
 