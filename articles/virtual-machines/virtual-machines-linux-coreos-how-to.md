---
ms.openlocfilehash: 3eb539cfe562ed94cf1c8dd6f7bacceb156c4359
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="如何使用 CoreOS |Microsoft Azure"
    description="介绍了 CoreOS，如何创建 CoreOS 虚拟机群集在 Azure，以及它的基本用法。"
    services="virtual-machines"
    documentationCenter=""
    authors="squillace"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="vm-linux"
    ms.workload="infrastructure-services"
    ms.date="08/03/2015"
    ms.author="rasquill"/>

# 如何在 Azure 上使用 CoreOS

本主题描述了[CoreOS] ，并演示如何为快速启动对理解此操作系统 Azure 上创建三个 CoreOS 虚拟机群集。 它使用非常基本的元素 CoreOS 部署和[使用 Azure 的 CoreOS]、 [Tim Park CoreOS 教程]和[Patrick Chanezon CoreOS 教程]中的示例演示既了解 CoreOS 部署的基本结构，并获得成功运行的三个虚拟机群集的绝对最低要求。

>[AZURE.NOTE] 本文介绍了如何使用 Azure 的命令行界面使用服务管理命令来创建 CoreOS 虚拟机。 若要开始使用 CoreOS Azure 资源管理器中，请尝试此[快速启动模板](https://azure.microsoft.com/documentation/templates/coreos-with-fleet-multivm/)。

## <a id='intro'>CoreOS、 群集和 Linux 容器</a>

CoreOS 是用于支持的可能非常大簇的 Linux 容器用作唯一的虚拟机快速创建 Linux 的轻量版本打包机制，包括[Docker]容器。 CoreOS 旨在支持︰

+ 非常高的自动化
+ 更容易、 更一致的应用程序部署
+ 在应用程序级别和系统级别的可伸缩性

从较高层次，为支持这些目标的 CoreOS 功能︰

1. 一个软件包系统︰ CoreOS 运行只在速度、 一致性，而且易于部署 Linux 容器中运行 Linux 容器图像
2. 以原子方式执行，以便操作系统更新作为单个实体，可以轻松地回滚到一个已知状态的操作系统更新
3. 内置[etcd](https://github.com/coreos/etcd)和[五星](https://github.com/coreos/fleet)守护程序 （服务） 的动态虚拟机和群集通信和管理

这是非常一般的 CoreOS 和它的功能说明。 有关 CoreOS 的详细信息，请参阅[CoreOS 概述]。

## <a id='security'>安全注意事项</a>
目前，CoreOS 假定可以 SSH 到群集的任何人有权对其进行管理。 其结果是不做任何修改，CoreOS 群集是未完成的测试和开发环境，但是应该应用在任何生产环境中的进一步安全措施。

## <a id='usingcoreos'>如何在 Azure 上使用 CoreOS</a>

本部分介绍如何创建具有三个 CoreOS 虚拟机中使用[Azure 命令行界面 (Azure CLI)]的 Azure 云 cervice。 基本步骤如下所示︰

1. 创建 SSH 证书和密钥来确保与 CoreOS 虚拟机的通信安全
2. 获得您的群集内部通信 etcd id
3. [YAML]格式创建云配置文件
4. 使用 Azure CLI 来创建新的 Azure 的云服务和三个 CoreOS 虚拟机
5. 测试从 Azure VM CoreOS 群集
6. 测试从本地主机 CoreOS 群集

### 创建公共和私有密钥进行通信

使用中的说明[如何使用 SSH 使用 Linux 在 Azure 上](virtual-machines-linux-use-ssh-key.md)创建 SSH 公钥和私钥。 （基本的步骤是在下面的说明）。要使用这些密钥来验证它们正在然后会与群集中连接到虚拟机。

> [AZURE.NOTE] 本主题假定您没有这些注册表项，并要求您创建`myPrivateKey.pem`和`myCert.pem`为澄清文件。 如果您已经有一个公共和私有密钥对保存到`~/.ssh/id_rsa`，您只需键入`openssl req -x509 -key ~/.ssh/id_rsa -nodes -days 365 -newkey rsa:2048 -out myCert.pem`以获取您需要上传到 Azure 的.pem 文件。

1. 在工作目录中，键入`openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout myPrivateKey.key -out myCert.pem`创建的专用密钥和一个与之关联的 X.509 证书。

2. 要断言私钥的所有者可以读取或写入该文件，请键入`chmod 600 myPrivateKey.key`。

您现在应该有两个`myPrivateKey.key`，`myCert.pem`在工作目录中的文件。


### 获取群集的 etcd id

CoreOS 的`etcd`守护程序要求发现 id 可以在群集中所有节点的自动查询。 要检索您发现 id 并保存到`etcdid`文件，请键入

```
curl https://discovery.etcd.io/new | grep ^http.* > etcdid
```

### 创建云配置文件

仍在同一个工作目录，使用您喜欢的文本与下面的文本编辑器创建一个文件并另存为`cloud-config.yaml`。 (可以保存任何文件的名称，但下一步中创建您的 Vm 时，您将需要引用该文件的名称，在您**-自定义数据** **azure vm 创建**命令的选项。)

> [AZURE.NOTE] 记住要键入`cat etcdid`检索的 etcd 发现 id`etcdid`上面创建文件并替换`<token>`在下面的示例`cloud-config.yaml`文件从生成的编号与您`etcdid`文件。 如果您不能验证您的群集在结束时，这可能是您被忽视的步骤之一 ！

```
#cloud-config

coreos:
  etcd:
    # generate a new token for each unique cluster from https://discovery.etcd.io/new
    discovery: https://discovery.etcd.io/<token>
    # deployments across multiple cloud services will need to use $public_ipv4
    addr: $private_ipv4:4001
    peer-addr: $private_ipv4:7001
  units:
    - name: etcd.service
      command: start
    - name: fleet.service
      command: start
```

（云配置文件有关的详细信息，请参阅[使用云配置](https://coreos.com/docs/cluster-management/setup/cloudinit-cloud-config/)CoreOS 文档中。

### 使用 Azure CLI 来创建一个新的 CoreOS 虚拟机
<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->

1. 如果您有不这样做，并通过工作或者登录安装[Azure 命令行界面 (Azure CLI)]或学校 ID，或下载.publishsettings 文件并将它导入到您的帐户。
2. 找到您的 CoreOS 图像。 若要找到该图像是否随时可用，请键入`azure vm image list | grep CoreOS`，您应看到相似结果的列表︰

    数据︰ 522.6.0-2b171e93f07c4903bcad35bda10acf22__CoreOS-稳定公共 Linux

3. 通过键入来创建云服务为基本群集`azure service create <cloud-service-name>`其中 <*云服务名称*> 是 CoreOS 云服务的名称。 此示例使用名称**`coreos-cluster`**;您将需要重复使用选择创建 CoreOS VM 内部云服务的实例的名称。

    一个注意︰ 如果您观察到目前为止在[预览门户网站](https://portal.azure.com)所做的工作，就会发现您的云服务名称是一个资源组和域，如下面的图像所示︰

    ![][CloudServiceInNewPortal]

4. 连接到云服务并使用**azure vm 创建**命令创建一个新的 CoreOS 虚拟机内部。 将传递中的 X.509 证书的位置**— ssh cert**选项。 通过键入以下命令，创建第一个 VM 映像记忆替换**coreos 群集**使用您创建的云服务名称︰

    ```
azure vm create --custom-data=cloud-config.yaml --ssh=22 --ssh-cert=./myCert.pem --no-ssh-password --vm-name=node-1 --connect=coreos-cluster --location="West US" 2b171e93f07c4903bcad35bda10acf22__CoreOS-Stable-522.6.0 core
```

5. 通过重复步骤 4 **-虚拟机名称**值替换**节点 2**中的命令来创建第二个节点和**— ssh**端口 2022年具有的值。

6. 重复步骤 4 **-虚拟机名称**值替换**节点 3**中的命令创建第三个节点， **— ssh**端口与 3022 值。

您可以看到从下面 CoreOS 群集在门户中的显示方式拍摄。

![][EmptyCoreOSCluster]

### 测试从 Azure VM CoreOS 群集

要测试您的群集，请确保您在工作目录中，然后连接到**节点 1** ，使用**ssh**，通过键入传递私钥︰

    ssh core@coreos-cluster.cloudapp.net -p 22 -i ./myPrivateKey.key

一旦建立连接，请键入`sudo fleetctl list-machines`来查看是否群集已标识群集中的所有虚拟机。 您应该收到类似下面的响应︰


    core@node-1 ~ $ sudo fleetctl list-machines
    MACHINE     IP      METADATA
    442e6cfb... 100.71.168.115  -
    a05e2d7c... 100.71.168.87   -
    f7de6717... 100.71.188.96   -


### 测试从本地主机 CoreOS 群集

最后，我们将测试从您本地的 Linux 客户端 CoreOS 群集。 您可能能够使用**npm**，安装**fleetctl** ，或者您可能想要安装**队**和自己本地客户端上生成**fleetctl** 。 **队**需要**golang**，因此您可能需要安装该首先通过键入︰

`sudo apt-get install golang`

然后通过键入克隆从 github**五星**存储库︰

`git clone https://github.com/coreos/fleet.git`

通过更改为生成**五星**`fleet`目录并键入

`./build`

并最后将**五星**（取决于您的配置，您可能会或可能不需要**sudo**） 易于使用︰

`cp bin/fleetctl /usr/local/bin`

请确保**队**有权访问您`myPrivateKey.key`通过键入工作目录中︰

`ssh-add ./myPrivateKey.key`

> [AZURE.NOTE] 如果您已经在使用`~/.ssh/id_rsa`键，然后添加与`ssh-add ~/.ssh/id_rsa`。

现在您可以开始测试远程使用从**节点 1**，使用相同的**fleetctl**命令，但传递某些远程参数︰

`fleetctl --tunnel coreos-cluster.cloudapp.net:22 list-machines`

结果应完全相同︰


    MACHINE     IP      METADATA
    442e6cfb... 100.71.168.115  -
    a05e2d7c... 100.71.168.87   -
    f7de6717... 100.71.188.96   -

## 下一步行动

您现在应该拥有在 Azure 上运行的三个节点 CoreOS 群集。 从这里，您可以探索如何创建更复杂的群集并使用 Docker 创建更加有趣的应用程序。 尝试几个快速的示例，请参阅[开始使用 Azure 上 CoreOS 上的五星]。

<!--Anchors-->
[CoreOS、 群集和 Linux 容器]: #intro
[安全注意事项]: #security
[如何在 Azure 上使用 CoreOS]: #usingcoreos
[副标题 3]: #subheading-3
[下一步行动]: #next-steps

<!--Image references-->
[CloudServiceInNewPortal]: ./media/virtual-machines-linux-coreos-how-to/cloudservicefromnewportal.png
[EmptyCoreOSCluster]: ./media/virtual-machines-linux-coreos-how-to/endresultemptycluster.png
[6]: ./media/markdown-template-for-new-articles/pretty49.png
[7]: ./media/markdown-template-for-new-articles/channel-9.png


<!--Link references-->
[Azure 命令行接口 (CLI Azure)]: ../xplat-cli.md
[CoreOS]: https://coreos.com/
[CoreOS 概述]: https://coreos.com/using-coreos/
[使用 Azure CoreOS]: https://coreos.com/docs/running-coreos/cloud-providers/azure/
[Tim Park CoreOS 教程]: https://github.com/timfpark/coreos-azure
[Patrick Chanezon CoreOS 教程]: https://github.com/chanezon/azure-linux/tree/master/coreos/cloud-init
[Docker]: http://docker.io
[YAML]: http://yaml.org/
[入门上 CoreOS 在 Azure 上五星]: virtual-machines-linux-coreos-fleet-get-started.md
