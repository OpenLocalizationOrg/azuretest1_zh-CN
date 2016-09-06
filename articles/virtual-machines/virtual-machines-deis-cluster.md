---
ms.openlocfilehash: 82ffe50ecf654fcc98022e9b1fe8f770d4aebff0
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="部署一个 3 节点 Deis Azure 上的群集"
   description="本指南介绍了如何创建一个 3 节点 Deis Azure 使用 Azure 资源管理器模板上的群集"
   services="virtual-machines"
   documentationCenter=""
   authors="HaishiBai"
   manager="larar"
   editor=""/>

<tags
   ms.service="virtual-machines"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="06/24/2015"
   ms.author="hbai"/>

# 部署一个 3 节点 Deis 群集

这篇文章将引导您完成资源调配[Deis](http://deis.io/) Azure 上的群集。 它涵盖了从创建必要的证书对部署和调整新调配群集上一个示例**转到**应用程序的所有步骤。

下图显示了已部署的系统的体系结构。 系统管理员可以管理群集使用 Deis **deis**和**deisctl**这样的工具。 通过 Azure 负载平衡器，转发给该成员之一群集上的节点的连接建立了连接。 客户端访问部署通过负载平衡器的应用程序。 在这种情况下，负载平衡器将通讯转发到 Deis 路由器网格，进一步 routs 对驻留在群集上的相应 Docker 容器的通信。

  ![已部署的 Desis 群集体系结构关系图](media/virtual-machines-deis-cluster/architecture-overview.png)

为了运行完成以下步骤，您需要︰

 * 有效的 Azure 预订。 如果您没有，您可以免费试用[azure.com](https://azure.microsoft.com)上。
 * 要使用 Azure 的资源组的工作或学校 id。 如果您有一个个人帐户和登录 Microsoft id，您需要[创建工作 id 从您个人第一](resource-group-create-work-id-from-personal.md)。
 * 或者，根据客户端操作系统 — [Azure PowerShell](powershell-install-configure.md)或[Mac、 Linux、 和 Windows Azure CLI](xplat-cli-install.md)。
 * [OpenSSL](https://www.openssl.org/)。 使用 OpenSSL 来生成必要的证书。
 * Git 客户端如[Git 大扫除](https://git-scm.com/)。
 * 若要测试该示例应用程序，还需要一个 DNS 服务器。 您可以使用的任何 DNS 服务器或服务支持通配符的记录。
 * 计算机运行 Deis 客户端工具。 您可以使用本地计算机或虚拟机。 您可以运行这些工具上几乎所有的 Linux 分发，但下面的说明使用 Ubuntu。

## 配置群集

在本节中，您将使用开放源代码存储库[azure 快速启动模板](https://github.com/Azure/azure-quickstart-templates)从[Azure 资源管理器](../resource-group-overview.md)模板。 首先，将关闭该模板进行复制。 然后，您将创建新的 SSH 密钥对进行身份验证。 然后，您将配置您的群集的新的标识符。 然后，最后，您将使用 Shell 脚本或者 PowerShell 的脚本来配置群集。

1. 克隆存储库︰ [https://github.com/Azure/azure-quickstart-templates](https://github.com/Azure/azure-quickstart-templates)。

        git clone https://github.com/Azure/azure-quickstart-templates

2. 转到模板文件夹︰

        cd azure-quickstart-templates\deis-cluster-coreos

3. 创建新使用 ssh keygen 的 SSH 密钥对︰

        ssh-keygen -t rsa -b 4096 -c "[your_email@domain.com]"

4. 生成使用上面的专用密钥的证书︰

        openssl req -x509 -days 365 -new -key [your private key file] -out [cert file to be generated]

5. 转到[https://discovery.etcd.io/new](https://discovery.etcd.io/new)以生成新的群集令牌，看上去类似于︰

        https://discovery.etcd.io/6a28e078895c5ec737174db2419bb2f3
<br />
每个 CoreOS 群集都需要一个唯一的标记，从这一免费服务。 请有关更多详细信息，参阅[CoreOS 文档](https://coreos.com/docs/cluster-management/setup/cluster-discovery/)。

6. 修改现有的**发现**标记替换为新的令牌的**云 config.yaml**文件︰

        #cloud-config
        ---
        coreos:
          etcd:
            # generate a new token for each unique cluster from https://discovery.etcd.io/new
            # uncomment the following line and replace it with your discovery URL
            discovery: https://discovery.etcd.io/3973057f670770a7628f917d58c2208a
        ...

7. 修改**azuredeploy parameters.json**文件︰ 打开一个文本编辑器中的步骤 4 中创建的证书。 复制所有文本之间`----BEGIN CERTIFICATE-----`和`-----END CERTIFICATE-----`到**sshKeyData**参数 （您将需要删除所有换行符）。

8. 修改的**newStorageAccountName**参数。 这是虚拟机操作系统磁盘的存储帐户。 此帐户名称必须是全局唯一的。

9. 修改的**publicDomainName**参数。 这将成为与负载平衡器公用 IP 的 DNS 名称的一部分。 最终的 FQDN 必须_[此参数的值]_的格式。_[区域]_。 cloudapp.azure.com。 例如，如果您指定名称为 deishbai32，并且资源组被部署到西部美国地区，然后到负载平衡器的最终 FQDN 将是 deishbai32.westus.cloudapp.azure.com。

10. 保存参数文件。 并且您可以设置使用 Azure PowerShell 的群集︰

        .\deploy-deis.ps1 -ResourceGroupName [resource group name] -ResourceGroupLocation "West US" -TemplateFile
        .\azuredeploy.json -ParametersFile .\azuredeploy-parameters.json -CloudInitFile .\cloud-config.yaml

  或 Azure CLI:

        ./deploy-deis.sh -n "[resource group name]" -l "West US" -f ./azuredeploy.json -e ./azuredeploy-parameters.json
        -c ./cloud-config.yaml  

11. 一旦配置资源组时，您可以在 Azure 门户网站上看到组中的所有资源。 下面的屏幕快照中所示，该资源组包含使用联接到可用性组相同的三个虚拟机的虚拟网络。 此组还包含一个负载平衡器，它具有相关联的公用 IP。

  ![在 Azure 门户上调配的资源组](media/virtual-machines-deis-cluster/resource-group.png)

## 安装客户端

您需要控制的**deisctl**您 Deis 群集。 尽管 deisctl 将自动安装在所有群集节点中，最好在一台管理员计算机上使用 deisctl。 此外，因为所有节点都配置的专用 IP 地址，您将需要使用 SSH 隧道通过负载平衡器，其中包含要连接到的节点计算机的公共 IP。 下面是设置 deisctl 上单独的 Ubuntu 物理计算机或虚拟机的步骤。

1. 安装 deisctl:mkdir deis

        cd deis
        curl -sSL http://deis.io/deisctl/install.sh | sh -s 1.6.1
        sudo ln -fs $PWD/deisctl /usr/local/bin/deisctl

2. 添加您的私钥对 ssh 代理︰

        eval `ssh-agent -s`
        ssh-add [path to the private key file, see step 1 in the previous section]

3. 配置 deisctl:

        export DEISCTL_TUNNEL=[public ip of the load balancer]:2223

模板定义入站映射 2223 以实例 1、 2224 实例 2 和 3 实例 2225年的 NAT 规则。 这为使用 deisctl 工具提供冗余。 您可以检查这些规则在 Azure 门户︰

![在负载平衡器上的 NAT 规则](media/virtual-machines-deis-cluster/nat-rules.png)

> [AZURE.NOTE] 当前模板仅支持 3 双节点群集。 这是因为 Azure 资源管理器模板 NAT 规则定义，它不支持循环语法中的限制。

## 安装并启动 Deis 平台

现在，您可以使用 deisctl 安装并启动 Deis 平台︰

    deisctl config platform set domain=[some domain]
    deisctl config platform set sshPrivateKey=[path to the private key file]
    deisctl install platform
    deisctl start platform

> [AZURE.NOTE] 启动该平台需要一段时间 （最高可达 10 分钟）。 尤其是，启动生成器服务可以需要很长时间。 并且有时需要几次尝试才会成功︰ 如果操作挂起，请键入`ctrl+c`中断执行该命令，然后重试。

您可以使用`deisctl list`来验证所有服务是否正在都运行︰

    deisctl list
    UNIT                            MACHINE                 LOAD    ACTIVE          SUB
    deis-builder.service            ebe3005e.../10.0.0.6    loaded  active          running
    deis-controller.service         ebe3005e.../10.0.0.6    loaded  active          running
    deis-database.service           9c79bbdd.../10.0.0.5    loaded  active          running
    deis-logger.service             9c79bbdd.../10.0.0.5    loaded  active          running
    deis-logspout.service           8d658d5a.../10.0.0.4    loaded  active          running
    deis-logspout.service           9c79bbdd.../10.0.0.5    loaded  active          running
    deis-logspout.service           ebe3005e.../10.0.0.6    loaded  active          running
    deis-publisher.service          8d658d5a.../10.0.0.4    loaded  active          running
    deis-publisher.service          9c79bbdd.../10.0.0.5    loaded  active          running
    deis-publisher.service          ebe3005e.../10.0.0.6    loaded  active          running
    deis-registry@1.service         ebe3005e.../10.0.0.6    loaded  active          running
    deis-router@1.service           ebe3005e.../10.0.0.6    loaded  active          running
    deis-router@2.service           8d658d5a.../10.0.0.4    loaded  active          running
    deis-router@3.service           9c79bbdd.../10.0.0.5    loaded  active          running
    deis-store-daemon.service       8d658d5a.../10.0.0.4    loaded  active          running
    deis-store-daemon.service       9c79bbdd.../10.0.0.5    loaded  active          running
    deis-store-daemon.service       ebe3005e.../10.0.0.6    loaded  active          running
    deis-store-gateway@1.service    9c79bbdd.../10.0.0.5    loaded  active          running
    deis-store-metadata.service     8d658d5a.../10.0.0.4    loaded  active          running
    deis-store-metadata.service     9c79bbdd.../10.0.0.5    loaded  active          running
    deis-store-metadata.service     ebe3005e.../10.0.0.6    loaded  active          running
    deis-store-monitor.service      8d658d5a.../10.0.0.4    loaded  active          running
    deis-store-monitor.service      9c79bbdd.../10.0.0.5    loaded  active          running
    deis-store-monitor.service      ebe3005e.../10.0.0.6    loaded  active          running
    deis-store-volume.service       8d658d5a.../10.0.0.4    loaded  active          running
    deis-store-volume.service       9c79bbdd.../10.0.0.5    loaded  active          running
    deis-store-volume.service       ebe3005e.../10.0.0.6    loaded  active          running

祝贺您 ！ 现在你正在运行 Deis clsuter 在 Azure 上 ！ 接下来，让我们将样本转到应用程序以查看操作中的群集部署。

## 部署和扩展 Hello World 应用程序

下面的步骤演示如何部署"Hello World"转到群集的应用程序。 根据[Deis 文档](http://docs.deis.io/en/latest/using_deis/using-dockerfiles/#using-dockerfiles)的步骤。

1. 对于路由正常工作的网格，将需要有指向负载平衡器的公共 IP 域通配符的记录。 下面的屏幕快照显示在 GoDaddy 示例域注册的 A 记录︰

    ![Godaddy A 记录](media/virtual-machines-deis-cluster/go-daddy.png)
<p />
2. 将 deis 安装︰

        mkdir deis
        cd deis
        curl -sSL http://deis.io/deis-cli/install.sh | sh
        ln -fs $PWD/deis /usr/local/bin/deis
        
3. 创建新的 SSH 密钥，并将公钥添加到 GitHub （当然，您还可以重用您现有的密钥）。 若要创建新的 SSH 密钥对，请使用︰

        cd ~/.ssh
        ssh-keygen (press [Enter]s to use default file names and empty passcode)

4. 到 GitHub 添加 id_rsa.pub 或您选择的公共密钥。 你可以使用 SSH 密钥配置屏幕中的添加 SSH 密钥按钮︰

  ![Github 键](media/virtual-machines-deis-cluster/github-key.png)
<p />
5. 注册一个新用户︰

        deis register http://deis.[your domain]
<p />
6. 添加的 SSH 密钥︰

        deis keys:add [path to your SSH public key]
  <p />      
7. 创建应用程序。

        git clone https://github.com/deis/helloworld.git
        cd helloworld
        deis create
        git push deis master
<p />
8. Git 推将触发 Docker 图像要生成和部署，这将花费几分钟的时间。 从我的经验，有时，步骤 10 （压入图像到专用存储库） 可能会挂起。 这种情况下，您可以停止该过程，删除应用程序中使用 deis 应用程序︰ 销毁 – <application name>` to remove the application and try again. You can use `deis apps:list 以了解您的应用程序的名称。 如果一切正常，您应该看到类似于以下命令输出的末尾︰

        -----> Launching...
               done, lambda-underdog:v2 deployed to Deis
               http://lambda-underdog.artitrack.com
               To learn more, use `deis help` or visit http://deis.io
        To ssh://git@deis.artitrack.com:2222/lambda-underdog.git
         * [new branch]      master -> master
<p />
9. 验证是否该应用程序是否正常工作︰

        curl -S http://[your application name].[your domain]
  您应该看到︰

        Welcome to Deis!
        See the documentation at http://docs.deis.io/ for more information.
        (you can use geis apps:list to get the name of your application).
<p />
10. 规模为 3 个实例的应用程序︰

        deis scale cmd=3
<p />
11. 或者，您可以使用 deis 来检查您的应用程序的详细信息的信息。 下面的输出是从我的应用程序部署︰

        deis info
        === lambda-underdog Application
        {
          "updated": "2015-05-22T06:14:10UTC",
          "uuid": "10c74ee7-b7ff-4786-967a-7e65af7eabc3",
          "created": "2015-05-22T06:07:55UTC",
          "url": "lambda-underdog.artitrack.com",
          "owner": "haishi",
          "id": "lambda-underdog",
          "structure": {
            "cmd": 3
          }
        }

        === lambda-underdog Processes
        --- cmd:
        cmd.1 up (v2)
        cmd.2 up (v2)
        cmd.3 up (v2)

        === lambda-underdog Domains
        No domains

## 下一步行动

这篇文章在您遍历所有的步骤，以设置一个新 Deis Azure 使用 Azure 资源管理器模板上的群集。 模板中刀具连接以及负载平衡部署的应用程序以支持冗余。 该模板还可以避免成员在节点上，从而节省了宝贵的公共 IP 资源，并提供了对主机应用程序更安全的环境中使用公用的 IPs。 若要了解详细信息，请参阅下列文章︰

[Azure 的资源管理器概述] [资源组概述]  
[如何使用 Azure CLI] [azure 命令行工具]  
[使用 Azure PowerShell 使用 Azure 资源管理器] [powershell azure 的资源管理器]  

[azure 命令行工具]: ../xplat-cli.md
[资源组概述]: ../resource-group-overview.md
[powershell azure 的资源管理器]: ../powershell-azure-resource-manager.md
