---
ms.openlocfilehash: 504a91fe34448c41b3704f819d8a44f890ce3423
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="如何使用 Azure 使用 docker 计算机"
   description="演示如何在 Azure Docker 机器在 Ubuntu 上得到启动和运行。"
   services="virtual-machines"
   documentationCenter="virtual-machines"
   authors="squillace"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="virtual-machines"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="05/25/2015"
   ms.author="rasquill"/>

# 如何使用 Azure docker 机

本主题介绍如何使用[机器](https://github.com/docker/machine)和[Azure CLI](https://github.com/Azure/azure-xplat-cli) [Docker](https://www.docker.com/)创建到 Azure 虚拟机快速和方便地从运行 Ubuntu 的计算机管理 Linux 容器。 为了演示，本教程演示如何部署[busybox Docker 中心图像](https://registry.hub.docker.com/_/busybox/)图像及还[nginx Docker 中心图像](https://registry.hub.docker.com/_/nginx/)，并配置 web 请求路由到 nginx 容器的容器。 （Docker**计算机**文档介绍如何修改这些指令的其他平台）。

有一些先决条件完成本教程。 您将需要安装以下︰

1. [npm](https://docs.npmjs.com/)和[Node.js](http://nodejs.org/)
2. [Azure CLI](https://github.com/Azure/azure-xplat-cli)
3. [Docker 客户端](https://docs.docker.com/installation/)

如果按此顺序安装这些项目，Ubuntu 计算机就本教程可以使用。 （本教程应该很大程度上是相同的任何其他基于 dpkg distro 如 Debian。 让我们知道在注释中发现额外的要求或步骤。）

## 获取 docker 机器-或生成

起步**docker 计算机**的最快方法是直接从[发行份额](https://github.com/docker/machine/releases)的适当版本下载。 在本教程中的客户端计算机正在运行 Ubuntu 在 x64 计算机，所以**docker-machine_linux-amd64**图像是使用。

您还可以生成**docker 机器**自己按照[分配给计算机](https://github.com/docker/machine#contributing)的步骤。 您应该准备好下载多达 1 GB 或更多执行生成，但您可以精确地自定义您的体验这样希望的方式。

> [AZURE.NOTE] 您也可以创建[符号链接](http://en.wikipedia.org/wiki/Symbolic_link)平台版本，但本教程使用二进制文件直接要非常清楚地说明问题。 结果是，而不是命令如`docker-machine env` **docker 计算机**文档所示，本教程将使用`docker-machine_linux-amd64 env`相反。 创建符号链接还是只需使用该二进制文件名称直接取决于您，但如果您更改了您正在使用的名称，请务必在下面的说明修改该名称。

<br />

>  任何一种方法选择，您必须直接在命令行或放置二进制文件的路径，如**/usr/local/bin**二进制文件中调用。 请记住，若要确保键入标记为可执行`chmod +x` &lt; *`binaryName`* &gt; ， &lt; *`binaryName`* &gt; Docker 机器可执行文件的名称。 本教程使用**docker-machine_linux-amd64**。

## 创建 docker、 计算机和 Azure 的证书和密钥文件

现在，您必须创建 Azure 需要确认您的身份和权限，以及这些**docker 机器**需要通信与您 Azure 的虚拟机创建和远程管理容器的证书和密钥文件。 如果您已经有了这些文件中的目录--也许是用于 docker — 可以重用它们。 但是，最好测试**docker 机器**将在一个单独的目录中创建它们并在它们指向 docker 机器。

> [AZURE.NOTE] 如果您最终一次次地尝试**docker 机器**，请务必或者重复使用相同的证书和密钥文件。 **docker 计算机**创建一组客户端的证书以及-一切它创建可以在检查`~/.docker/machine`。 如果这些证书移动到另一台计算机时，您将需要移动**docker 机器**证书文件夹。 如果您打算在另一个平台上，例如使用**docker 机器**，只需查看它如何工作，这使得存在差异。

如果您接触过 Linux 发行版本，您可能已经可在特定地点和[Docker HTTPS 文档很好地解释这些步骤](https://docs.docker.com/articles/https/)在您的计算机上使用这些文件。 但是，以下是最简单的形式的这一步。

1. 创建一个本地文件夹，在命令行中，导航到该文件夹和类型︰

        openssl req -x509 -nodes -days 365 -newkey rsa:1024 -keyout mycert.pem -out mycert.pem
        openssl pkcs12 -export -out mycert.pfx -in mycert.pem -name "My Certificate"

    准备在此处输入您的证书导出密码并捕获其供将来使用。 然后键入︰

        openssl x509 -inform pem -in mycert.pem -outform der -out mycert.cer

2. 将您的证书的.cer 文件上载到 Azure。 在[Azure 门户网站](https://manage.windowsazure.com)中，请单击左下角 （如下所示） 服务区域中的**设置**

    ![][portalsettingsitem]

    然后单击**管理证书**︰

    ![][managementcertificatesitem]

    然后 （页面底部）**上载**和![][uploaditem]上载您在上一步中创建的**mycert.cer**文件。

3. 在门户中的相同**设置**窗格中，单击**订阅**并捕获订阅 ID 创建时要使用 VM，因为不会使用它的下一步。 (您还可以在命令行上使用 Azure CLI 命令查找订阅 id `azure account list`，您拥有每个订阅的订阅 id 显示在帐户中。)

4. 在 Azure 使用**docker 机器 create**命令创建 docker 宿主虚拟机。 该命令要求订阅 ID 只捕获在上一步和步骤 1 中创建的**.pem**文件的路径。 本主题使用"计算机名"以及 Azure 云服务 （VM） 的名称，但应将其替换为您自己选择，请记住要在需要 vm 名称的任何其他步骤中使用您的云服务名称。 （请记住，在此示例中，我们将使用完全二进制文件的名称并不**docker 计算机**符号链接）。

        docker-machine_linux-amd64 create \
        -d azure \
        --azure-subscription-id="<subscription ID acquired above>" \
        --azure-subscription-cert="mycert.pem" \
        machine-name

    随着前两个步骤需要的新的虚拟机创建和加载 Linux Azure 代理的以及更新的新的虚拟机，您应该看到类似于以下。

        INFO[0001] Creating Azure machine...
        INFO[0049] Waiting for SSH...
        modprobe: FATAL: Module aufs not found.
        + sudo -E sh -c sleep 3; apt-get update
        + sudo -E sh -c sleep 3; apt-get install -y -q linux-image-extra-3.13.0-36-generic
        E: Unable to correct problems, you have held broken packages.
        modprobe: FATAL: Module aufs not found.
        Warning: tried to install linux-image-extra-3.13.0-36-generic (for AUFS)
         but we still have no AUFS.  Docker may not work. Proceeding anyways!
        + sleep 10
        + [ https://get.docker.com/ = https://get.docker.com/ ]
        + sudo -E sh -c apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 36A1D7869245C8950F966E92D8576A8BA88D21E9
        gpg: requesting key A88D21E9 from hkp server keyserver.ubuntu.com
        gpg: key A88D21E9: public key "Docker Release Tool (releasedocker) <docker@dotcloud.com>" imported
        gpg: Total number processed: 1
        gpg:               imported: 1  (RSA: 1)
        + sudo -E sh -c echo deb https://get.docker.com/ubuntu docker main > /etc/apt/sources.list.d/docker.list
        + sudo -E sh -c sleep 3; apt-get update; apt-get install -y -q lxc-docker
        + sudo -E sh -c docker version
        INFO[0368] "machine-name" has been created and is now the active machine.
        INFO[0368] To point your Docker client at it, run this in your shell: $(docker-machine_linux-amd64 env machine-name)

    > [AZURE.NOTE] 因为创建虚拟机后，可能需要几分钟的时间处于就绪状态。 等待您，您可以通过键入检查新 Docker 主机的状态`azure vm list`使用 Azure CLI，直到您看到您的 Vm 使用**ReadyRole**状态。

5. 为终端会话设置 docker 和计算机环境变量。 最后一行的反馈建议您立即运行导出 docker 客户直接使用特定计算机所需的环境变量**env**命令。

        $(docker-machine_linux-amd64 env machine-name)

    一旦这样做，不需要任何特殊的命令来使用您的本地 docker 客户端连接到 Docker**计算机**创建虚拟机。

        $ docker info
        Containers: 0
        Images: 0
        Storage Driver: devicemapper
         Pool Name: docker-8:1-131736-pool
         Pool Blocksize: 65.54 kB
         Backing Filesystem: extfs
         Data file: /dev/loop0
         Metadata file: /dev/loop1
         Data Space Used: 305.7 MB
         Data Space Total: 107.4 GB
         Metadata Space Used: 729.1 kB
         Metadata Space Total: 2.147 GB
         Udev Sync Supported: false
         Data loop file: /var/lib/docker/devicemapper/devicemapper/data
         Metadata loop file: /var/lib/docker/devicemapper/devicemapper/metadata
         Library Version: 1.02.82-git (2013-10-04)
        Execution Driver: native-0.2
        Kernel Version: 3.13.0-36-generic
        Operating System: Ubuntu 14.04.1 LTS
        CPUs: 1
        Total Memory: 1.639 GiB
        Name: machine-name
        ID: W3FZ:BCZW:UX24:GDSV:FR4N:N3JW:XOC2:RI56:IWQX:LRTZ:3G4P:6KJK
        WARNING: No swap limit support

> [AZURE.NOTE] 本教程展示**docker 计算机**创建一个 VM。 但是，您可以重复创建尽可能多的计算机所需的步骤。 如果这样做，docker 与虚拟机之间进行切换的最好办法是使用**env**命令内联设置**docker**每个单个命令的环境变量。 例如，若要使用不同的 VM **docker 信息**，您可以键入`docker $(docker-machine env <VM name>) info`和**env**命令的 docker 连接信息填入该虚拟机上使用。

## 我们正在做。 让我们运行一些应用程序使用远程 docker 和图像从 Docker 集线器。

现在，您可以使用 docker 以正常方式在容器中创建的应用程序。 最简单的方法来演示是[busybox](https://registry.hub.docker.com/_/busybox/):

        $  docker run busybox echo hello world
        Unable to find image 'busybox:latest' locally
        511136ea3c5a: Pull complete
        df7546f9f060: Pull complete
        ea13149945cb: Pull complete
        4986bf8c1536: Pull complete
        busybox:latest: The image you are pulling has been verified. Important: image verification is a tech preview feature and should not be relied on to provide security.
        Status: Downloaded newer image for busybox:latest
        hello world

但是，您可以创建应用程序，您可以在 internet 上，如[nginx](https://registry.hub.docker.com/_/nginx/) [Docker 中心](https://registry.hub.docker.com/)立即看到。

> [AZURE.NOTE] 请记住使用**-P**选项让**docker**分配给图像和**-d** ，以确保容器运行在后台连续随机端口。 （如果您忘记了，您将开始 nginx，然后它将立即关闭。 不要忘记 ！）

    $ docker run --name machinenginx -P -d nginx
    Unable to find image 'nginx:latest' locally
    30d39e59ffe2: Pull complete
    c90d655b99b2: Pull complete
    d9ee0b8eeda7: Pull complete
    3225d58a895a: Pull complete
    224fea58b6cc: Pull complete
    ef9d79968cc6: Pull complete
    f22d05624ebc: Pull complete
    117696d1464e: Pull complete
    2ebe3e67fb76: Pull complete
    ad82b43d6595: Pull complete
    e90c322c3a1c: Pull complete
    4b5657a3d162: Pull complete
    511136ea3c5a: Already exists
    nginx:latest: The image you are pulling has been verified. Important: image verification is a tech preview feature and should not be relied on to provide security.
    Status: Downloaded newer image for nginx:latest
    5883e2ff55a4ba0aa55c5c9179cebb590ad86539ea1d4d367d83dc98a4976848

若要从互联网中看到它，Azure VM 在端口 80 上创建公共端点，nginx 容器的端口映射的端口。 首先，使用`docker ps -a`找到容器和查找哪些端口**docker**已分配给容器的端口 80。 （下面编辑所显示的信息只显示端口信息; 您将看到更多）。

    $ docker ps -a
    IMAGE               PORTS
    nginx:latest        0.0.0.0:49153->80/tcp, 0.0.0.0:49154->443/tcp
    busybox:latest

我们可以看到该 docker 已分配给虚拟机的端口 49153 的容器的端口 80。 我们现在可以使用 Azure CLI 外部云服务的公用端口 80 映射到虚拟机上的端口 49153。 然后 docker 可确保虚拟机 49153 端口上的入站的 tcp 通信被路由到 nginx 的容器。

    $ azure vm endpoint create machine-name 80 49153
    info:    Executing command vm endpoint create
    + Getting virtual machines
    + Reading network configuration
    + Updating network configuration
    info:    vm endpoint create command OK

打开您最喜爱的浏览器，然后看一看。

![][nginx]

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## 下一步行动
转到[Docker 用户指南](https://docs.docker.com/userguide/)，在 Microsoft Azure 上创建一些应用程序。 或者，转[**docker**和蜂群](https://github.com/docker/swarm)在 Azure](virtual-machines-docker-swarm) 上播放，并了解如何使用 Azure docker 与蜂群。

<!--Image references-->
[nginx]: ./media/virtual-machines-docker-machine/nginxondocker.png
[portalsettingsitem]: ./media/virtual-machines-docker-machine/portalsettingsitem.png
[managementcertificatesitem]: ./media/virtual-machines-docker-machine/managementcertificatesitem.png
[uploaditem]: ./media/virtual-machines-docker-machine/uploaditem.png

<!--Link references-->
[链接 1 到另一个 azure.microsoft.com 文档主题]: virtual-machines-windows-tutorial.md
[另一 azure.microsoft.com 文档主题链接 2]: ../web-sites-custom-domain-name.md
[链接到另一个 azure.microsoft.com 文档主题的 3]: ../storage-whatis-account.md
 