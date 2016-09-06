---
ms.openlocfilehash: 8c5827da90a4dcb57ca796fa9d61a789c174960f
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---

<properties
   pageTitle="使用 Azure 上蜂群 docker 入门教程"
   description="描述如何创建具有 Docker VM 扩展的一组虚拟机并使用蜂群创建 Docker 群集。"
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
   ms.date="05/24/2015"
   ms.author="rasquill"/>

# 如何使用蜂群 docker

本主题显示了非常简单的方法使用[docker](https://www.docker.com/) [群](https://github.com/docker/swarm)在 Azure 上创建一个蜂群管理群集。 它在 Azure，一个作为蜂群管理器和三个作为 docker 主机的群集的一部分创建了四个虚拟机。 当您完成时，可以使用蜂群，请参阅群集，然后开始使用 docker。 此外，本主题中的 Azure CLI 调用将使用服务管理 (asm) 模式。 

> [AZURE.NOTE] 这是软件的早期版本，因此获得有关使用 Azure 上这来创建大，更新检查平衡，控制的群集 Docker 容器，以及检查 docker 群文档，以了解它的所有功能。
<!-- -->
> 此外，本主题使用 docker 蜂群和 Azure CLI*不*使用**docker 机器**来显示不同的工具是如何协同工作，但仍保持独立。 **docker 机器**有**-群**交换机，您可以使用**docker 机器**可以直接将节点添加到一个蜂群。 有关示例，请参见[docker 计算机](https://github.com/docker/machine)文档。 如果您错过了**docker 机**对 Azure 虚拟机运行，请参阅[如何使用 Azure docker 机器](virtual-machines-docker-machine.md)。

## 创建 docker 主机使用 Azure 的虚拟机

本主题创建四个 Vm，但您可以使用任何所需的数量。 调用以下*&lt;密码&gt;*替换为您选择的密码。

    azure vm docker create swarm-master -l "East US" -e 22 $imagename ops <password>
    azure vm docker create swarm-node-1 -l "East US" -e 22 $imagename ops <password>
    azure vm docker create swarm-node-2 -l "East US" -e 22 $imagename ops <password>
    azure vm docker create swarm-node-3 -l "East US" -e 22 $imagename ops <password>

当您完成时您应该能够使用**azure 虚拟机列表**来查看 Azure Vm:

    $ azure vm list | grep "swarm-[mn]"
    data:    swarm-master     ReadyRole           East US       swarm-master.cloudapp.net                               100.78.186.65
    data:    swarm-node-1     ReadyRole           East US       swarm-node-1.cloudapp.net                               100.66.72.126
    data:    swarm-node-2     ReadyRole           East US       swarm-node-2.cloudapp.net                               100.72.18.47  
    data:    swarm-node-3     ReadyRole           East US       swarm-node-3.cloudapp.net                               100.78.24.68  

## 在蜂群主虚拟机上安装的蜂群

本主题使用[docker 安装容器模型群文档](https://github.com/docker/swarm#1---docker-image)，但您还可以 SSH 到**蜂群母版**。 在此模型中，**群**下载作为 docker 容器运行的蜂群中。 下面，我们将执行此步骤*通过 docker 我们便携式计算机通过远程*连接**的蜂群主**虚拟机，告诉它使用**蜂群创建**的群集 id 创建命令。 群集 id 是如何**群**发现蜂群组的成员。 (还可以克隆存储库中，然后生成它自己，这将让您完全控制并启用调试。)

    $ docker --tls -H tcp://swarm-master.cloudapp.net:2376 run --rm swarm create
    Unable to find image 'swarm:latest' locally
    511136ea3c5a: Pull complete
    a8bbe4db330c: Pull complete
    9dfb95669acc: Pull complete
    0b3950daf974: Pull complete
    633f3d9a9685: Pull complete
    bba5f98a0414: Pull complete
    defbc1ab4462: Pull complete
    92d78d321ff2: Pull complete
    swarm:latest: The image you are pulling has been verified. Important: image verification is a tech preview feature and should not be relied on to provide security.
    Status: Downloaded newer image for swarm:latest
    36731c17189fd8f450c395db8437befd

最后一行是群集 id;将它复制某个地方因为将在 Vm 的节点加入创建"蜂群"蜂群主时再次使用。 在此示例中，该群集 id 是**36731c17189fd8f450c395db8437befd**。

> [AZURE.NOTE] 只是为了要清楚，我们将使用我们本地 docker 安装来连接到中的**蜂群主**VM Azure 和指令**的蜂群母版**下载、 安装和运行**创建**命令，返回我们稍后将用于查询目的我们群集 id。
<!-- -->
> 要确认这一点，请运行`docker -H tcp://`*&lt;主机名&gt;*` images`列出容器流程在**蜂群主控**机上，进行比较的另一个节点上 （因为我们使用**-rm**开关运行前一个蜂群命令、 容器中移除了它完成之后，使用**docker ps-**将不会返回任何内容）。:


        $ docker --tls -H tcp://swarm-master.cloudapp.net:2376 images
        REPOSITORY          TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
        swarm               latest              92d78d321ff2        11 days ago         7.19 MB
        $ docker --tls -H tcp://swarm-node-1.cloudapp.net:2376 images
        REPOSITORY          TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
        $
<P />
> 如果您熟悉**docker**，您就知道其他节点具有任何项，因为没有图像已经下载并运行。

## 加入我们 docker 群集节点的虚拟机

对于每个节点，列出了使用 Azure CLI 的终结点信息。 下面我们这样做的**蜂群节点 1** docker 主机获取节点的 docker 端口。

    $ azure vm endpoint list swarm-node-1
    info:    Executing command vm endpoint list
    + Getting virtual machines
    data:    Name    Protocol  Public Port  Private Port  Virtual IP      EnableDirectServerReturn  Load Balanced
    data:    ------  --------  -----------  ------------  --------------  ------------------------  -------------
    data:    docker  tcp       2376         2376          138.91.112.194  false                     No
    data:    ssh     tcp       22           22            138.91.112.194  false                     No
    info:    vm endpoint list command OK


使用**docker**和`-H`选项指向 docker 客户端虚拟机，您的节点将该节点加入到您正在创建由群集 id 和节点 docker 端口传递的蜂群的 (后者使用**-地址**):

    $ docker --tls -H tcp://swarm-node-1.cloudapp.net:2376 run -d swarm join --addr=138.91.112.194:2376 token://36731c17189fd8f450c395db8437befd
    Unable to find image 'swarm:latest' locally
    511136ea3c5a: Pull complete
    a8bbe4db330c: Pull complete
    9dfb95669acc: Pull complete
    0b3950daf974: Pull complete
    633f3d9a9685: Pull complete
    bba5f98a0414: Pull complete
    defbc1ab4462: Pull complete
    92d78d321ff2: Pull complete
    swarm:latest: The image you are pulling has been verified. Important: image verification is a tech preview feature and should not be relied on to provide security.
    Status: Downloaded newer image for swarm:latest
    bbf88f61300bf876c6202d4cf886874b363cd7e2899345ac34dc8ab10c7ae924

这看起来很好。 若要确认**群**我们键入**蜂群节点 1**上运行︰

    $ docker --tls -H tcp://swarm-node-1.cloudapp.net:2376 ps -a
        CONTAINER ID        IMAGE               COMMAND                CREATED             STATUS              PORTS               NAMES
        bbf88f61300b        swarm:latest        "/swarm join --addr=   13 seconds ago      Up 12 seconds       2375/tcp            angry_mclean

对群集中的其他所有节点重复。 在我们的例子中，我们做的**蜂群节点 2**和**节点 3 的蜂群**。

## 开始管理蜂群群集

    $ docker --tls -H tcp://swarm-master.cloudapp.net:2376 run -d -p 2375:2375 swarm manage token://36731c17189fd8f450c395db8437befd
    d7e87c2c147ade438cb4b663bda0ee20981d4818770958f5d317d6aebdcaedd5

则可以出您的节点列出群集中︰

    ralph@local:~$ docker --tls -H tcp://swarm-master.cloudapp.net:2376 run --rm swarm list token://73f8bc512e94195210fad6e9cd58986f
    54.149.104.203:2375
    54.187.164.89:2375
    92.222.76.190:2375

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## 下一步行动

转到您的蜂群上运行操作。 来寻找灵感时，请参阅[https://github.com/docker/swarm/](https://github.com/docker/swarm/)，或可能是[视频](https://www.youtube.com/watch?v=EC25ARhZ5bI)。

<!-- links -->

[docker-机-azure]: virtual-machines-docker-machine.md
 