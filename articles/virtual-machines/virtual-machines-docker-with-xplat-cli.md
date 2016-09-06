---
ms.openlocfilehash: 92d92381de6d132cdf6a20bdc97d8bb8eecc8b2e
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="对于 Linux 在 Azure 上使用 Docker VM 扩展"
    description="介绍 Docker 以及 Azure 的虚拟机的扩展，并演示如何以编程方式对 Azure 从命令行使用 Azure CLI docker 主机上创建虚拟机。"
    services="virtual-machines"
    documentationCenter=""
    authors="squillace"
    manager="timlt"
    editor="tysonn"/>

<tags
    ms.service="virtual-machines"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="vm-linux"
    ms.workload="infrastructure-services"
    ms.date="05/22/2015"
    ms.author="rasquill"/>

# 使用命令行 Azure Docker VM 扩展接口 (Azure CLI)

本主题介绍如何使用来创建一个虚拟机在任何平台上的服务管理 (asm) 模式在 Azure CLI Docker VM 扩展。 [Docker](https://www.docker.com/)是[Linux 的容器](http://en.wikipedia.org/wiki/LXC)，而不是虚拟机用作一种隔离数据，并计算在共享资源上最受欢迎的虚拟化方法之一。 [Azure Linux 代理](virtual-machines-linux-agent-user-guide.md)Docker VM 扩展用于创建承载任意数量的容器的应用程序在 Azure 上 Docker VM。 若要查看容器和其各自的优点更高级的讨论，请参阅[Docker 高级别白板](http://channel9.msdn.com/Blogs/Regular-IT-Guy/Docker-High-Level-Whiteboard)。

+ [如何使用 Azure Docker VM 扩展]
+ [对于 Linux 和 Windows 虚拟机扩展]
+ [容器和 Azure 的容器管理资源]
+ [下一步行动]

## <a id='How to use the Docker VM Extension with Azure'>如何使用 Azure Docker VM 扩展</a>
若要使用 Azure Docker VM 扩展，必须安装[Azure 的命令行界面](https://github.com/Azure/azure-sdk-tools-xplat)(Azure CLI) 比 0.8.6 更高的版本 （在撰写本文时的最新版本是 0.8.10）。 您可以安装在 Mac、 Linux、 Windows Azure CLI。



在 Azure 上使用 Docker 完整过程非常简单︰

+ 要控制 Azure 的计算机上安装 Azure CLI 及其依赖项 （在 Windows 上，这将是作为虚拟机运行 Linux 分发）
+ 使用 Azure CLI Docker 命令在 Azure 创建 VM Docker 主机
+ 使用本地 Docker 命令来管理在 Azure Docker VM 中的您 Docker 容器。


### 安装 Azure 命令行界面 (CLI Azure)

若要安装和配置 Azure CLI，请参阅[如何安装 Azure 命令行界面](../xplat-cli-install.md)。 若要确认安装，请键入`azure`在命令提示符处，您应该会看到 Azure CLI ASCII 艺术短片刻后其中列出了可用的基本命令。 如果安装正确工作，您应该能够键入`azure help vm`，看到其中一个列出的命令是"docker"。

> [AZURE.NOTE] Docker 对于 Windows， [Boot2Docker](https://docs.docker.com/installation/windows/)，您还可以使用自动 docker 客户机可用于使用 Azure 作为 docker 主机的虚拟机创建具有安装程序。

### Azure CLI 连接到 Azure 帐户
可以使用 Azure CLI 之前必须将 Azure 帐户凭据使用 Azure CLI 关联您的平台上。 [如何连接到 Azure 订购](../xplat-cli-connect.md)部分说明了如何下载并导入**.publishsettings**文件或将 Azure CLI 组织 id 相关联。

> [AZURE.NOTE] 使用一个或其他身份验证方法，因此请不要请务必阅读上面来了解不同功能的文档时行为的某些差异。

### 安装 Docker 并使用 Azure Docker VM 扩展
按照您的计算机上本地安装 Docker [Docker 安装说明](https://docs.docker.com/installation/#installation)。

若要使用 Docker Azure 虚拟机使用，用于 VM 的 Linux 图像必须安装的[Azure Linux 虚拟机代理](virtual-machines-linux-agent-user-guide.md)。 目前，有只有两种类型的图像提供此︰

+ Ubuntu Azure 图像库中的图像或

+ 使用 Azure Linux 虚拟机代理创建一个自定义 Linux 映像安装和配置。 有关如何生成自定义 Linux 虚拟机与 Azure VM 代理的详细信息，请参阅[Azure Linux 虚拟机代理](virtual-machines-linux-agent-user-guide.md)。

### 使用 Azure 图像库

在 Bash 或终端会话中，使用以下 Azure CLI 命令通过键入使用 VM 库中找到最新的 Ubuntu 图像

`azure vm image list | grep Ubuntu-14_04`

并选择一个图像名称，如`b39f27a8b8c64d52b05eac6a62ebad85__Ubuntu-14_04-LTS-amd64-server-20140724-en-us-30GB`，并使用下面的命令来创建新的虚拟机使用该图像。

```
azure vm docker create -e 22 -l "West US" <vm-cloudservice name> "b39f27a8b8c64d52b05eac6a62ebad85__Ubuntu-14_04-LTS-amd64-server-20140724-en-us-30GB" <username> <password>
```

其中：

+ * &lt;cloudservice 虚拟机的名称&gt;*将成为 Docker 容器主机在 Azure 中的虚拟机的名称

+  *&lt;用户名&gt;*是 VM 的默认根用户的用户名

+ *&lt;密码&gt;*是符合标准的复杂性的 Azure 的*用户名*帐户的密码

> [AZURE.NOTE] 目前，密码必须至少为 8 个字符，包含一个小写字母和一个大写字符、 数字和特殊字符，如以下字符之一︰ `!@#$%^&+=`。 不对，在前一个句子的结束期限不是特殊字符。

如果该命令成功，您应该看到类似于以下内容，这取决于精确参数和您使用的选项︰

![](./media/virtual-machines-docker-with-xplat-cli/dockercreateresults.png)

> [AZURE.NOTE] 创建虚拟机可能要花几分钟的时间，但已设置后 Docker 守护进程 （Docker 服务） 启动，您可以连接到 Docker 容器主机。

若要测试 Docker VM 在 Azure 创建，请键入

`docker --tls -H tcp://<vm-name-you-used>.cloudapp.net:2376 info`

其中*< vm--您的使用名称 >*是在调用中使用的虚拟机的名称`azure vm docker create`。 您应该看到类似于以下内容，这表明 Docker 主机 VM 启动并运行在 Azure，等待您的命令。

![](./media/virtual-machines-docker-with-xplat-cli/connectingtodockerhost.png)

### Docker 主机虚拟机身份验证
除了创建 Docker VM，`azure vm docker create`命令也会自动创建允许 Docker 客户端计算机连接到使用 HTTPS，Azure 容器主机的必要证书和证书存储在客户端和主机计算机，根据。 在后续运行，现有的证书被重用，并且与新的主机共享。

默认情况下，将证书放置在`~/.docker`，和 Docker 将被配置为在端口**2376年**上运行。 如果您想要使用不同的端口或目录，则您可以使用下列任一`azure vm docker create`命令行选项来配置 Docker 容器承载虚拟机连接的客户端使用不同的端口或不同的证书︰

```
-dp, --docker-port [port]              Port to use for docker [2376]
-dc, --docker-cert-dir [dir]           Directory containing docker certs [.docker/]
```

Docker 守护程序在主机上的配置为侦听并进行身份验证的客户端连接上使用生成的证书的指定端口`azure vm docker create`命令。 客户端计算机必须有这些证书访问 Docker 主机。

> [AZURE.NOTE] 没有这些证书的情况下运行的网络的主机可以连接到该计算机的任何人都将受到攻击。 修改默认配置之前，请确保您了解您的计算机和应用程序的风险。




## 下一步行动

您就可以转到[Docker 用户指南]和使用 Docker VM。 若要创建新的门户中 Docker 启用虚拟机，请参阅[如何使用与该门户 Docker VM 扩展]。

<!--Anchors-->
[子标题 1]: #subheading-1
[2 小标题]: #subheading-2
[副标题 3]: #subheading-3
[下一步行动]: #next-steps

[如何使用 Azure Docker VM 扩展]: #How-to-use-the-Docker-VM-Extension-with-Azure
[对于 Linux 和 Windows 虚拟机扩展]: #Virtual-Machine-Extensions-For-Linux-and-Windows
[容器和 Azure 的容器管理资源]: #Container-and-Container-Management-Resources-for-Azure

<!--Image references-->
[5]: ./media/markdown-template-for-new-articles/octocats.png
[6]: ./media/markdown-template-for-new-articles/pretty49.png
[7]: ./media/markdown-template-for-new-articles/channel-9.png


<!--Link references-->
[链接 1 到另一个 azure.microsoft.com 文档主题]: virtual-machines-windows-tutorial.md
[另一 azure.microsoft.com 文档主题链接 2]: ../web-sites-custom-domain-name.md
[链接到另一个 azure.microsoft.com 文档主题的 3]: ../storage-whatis-account.md
[如何使用门户 Docker VM 扩展]: http://azure.microsoft.com/documentation/articles/virtual-machines-docker-with-portal/

[Docker 用户指南]: https://docs.docker.com/userguide/
 