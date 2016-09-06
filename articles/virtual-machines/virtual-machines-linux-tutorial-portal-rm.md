---
ms.openlocfilehash: ee28c1134a6385aa671c831be7385daced27aa3f
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="创建在 Azure 门户运行 Linux Azure 的虚拟机"
    description="使用 Azure 门户创建使用 Azure 的资源组中运行 Linux Azure 虚拟机 (VM)。"
    services="virtual-machines"
    documentationCenter=""
    authors="squillace"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-management"/>

<tags
    ms.service="virtual-machines"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/13/2015"
    ms.author="rasquill"/>

# 创建虚拟机运行 Linux 使用 Azure 预览门户

> [AZURE.SELECTOR]
- [Azure CLI](virtual-machines-linux-tutorial.md)
- [Azure 预览门户](virtual-machines-linux-tutorial-portal-rm.md)

创建运行 Linux Azure 虚拟机 (VM) 是容易做到的。 本教程演示如何使用 Azure 预览门户创建一个快速，并使用`~/.ssh/id_rsa.pub`公钥文件来保护您的**SSH**连接到 VM。 此外可以创建[您自己的图像作为模板](virtual-machines-linux-create-upload-vhd.md)使用的 Linux 虚拟机。

> [AZURE.NOTE] 本教程中创建由 Azure 资源组 API 管理 Azure 的虚拟机。 有关详细信息，请参阅[Azure 资源组概述](resource-group-overview.md)。

[AZURE.INCLUDE [free-trial-note](../../includes/free-trial-note.md)]

## 选择图像

转到 Azure 市场上预览门户中，查找您希望 Windows 服务器虚拟机映像。

1. 登录到[门户网站预览](https://portal.azure.com)。

2. 在集线器菜单上，单击**新建** > **计算** > **Ubuntu 服务器 14.04 LTS**。

    ![选择虚拟机映像](media/virtual-machines-linux-tutorial-portal-rm/chooseubuntuvm.png)

    > [AZURE.TIP] 若要查找其他图像，请单击**市场**，然后搜索或筛选可用项目。

3. 在**Ubuntu 服务器 14.04 LTS**页的底部，选择**使用资源管理器堆栈**Azure 资源管理器中创建虚拟机。 请注意，对于大多数新工作负载，我们建议的资源管理器堆栈。 有关注意事项的信息，请参阅[Azure 计算、 网络和存储提供商在 Azure 资源管理器](virtual-machines-azurerm-versus-azuresm.md)。

4. 接下来，单击 ![创建按钮](media/virtual-machines-linux-tutorial-portal-rm/createbutton.png).

    ![更改为资源管理器计算堆栈](media/virtual-machines-linux-tutorial-portal-rm/changetoresourcestack.png)

## 创建虚拟机

选择图像后，可以对大部分配置使用 Azure 的默认设置并快速创建虚拟机。

1. 在**创建虚拟机**刀片式服务器，单击**基础知识**。 输入要用于虚拟机和公用密钥文件的**名称**(在**ssh rsa**格式，在此情况下，从`~/.ssh/id_rsa.pub`文件)。 如果您有多个订阅，则指定为新的虚拟机，以及新的或现有的**资源组**和 Azure 数据中心**位置**。

    ![](media/virtual-machines-linux-tutorial-portal-rm/step-1-thebasics.png)

    > [AZURE.NOTE] 此外可以选择用户名/密码身份验证，请输入该信息，如果不想保护**ssh**会话使用公钥和私钥的密钥交换。

2. 单击**大小**，然后选择合适的 VM 大小您的需要。 每个大小指定数计算内核、 内存和其他功能，例如支持高级存储，将会影响价格。 Azure 建议某些自动根据您选择的图像的大小。 完成后，单击![选择按钮](media/virtual-machines-linux-tutorial-portal-rm/selectbutton-size.png)。

    >[AZURE.NOTE] 高级存储空间的 DS 系列虚拟机，在某些地区。 高级存储是最适合如数据库数据密集型工作负载的存储选项。 有关详细信息，请参阅[高级存储︰ Azure 虚拟机工作负载的高性能存储](storage-premium-storage-preview-portal.md)。

3. 单击以查看存储和网络设置为新的虚拟机**设置**。 第一个 vm 通常可以接受默认设置。 如果选择了支持该 VM 大小，您可以选择**磁盘类型**下的**最优 (SSD)**尝试出最优存储。 完成后，单击![OK 按钮](media/virtual-machines-linux-tutorial-portal-rm/okbutton.png)。

    ![](media/virtual-machines-linux-tutorial-portal-rm/step-3-settings.png)

6. 单击以查看配置选择**摘要**。 在完成审核或更新这些设置，请单击![OK 按钮](media/virtual-machines-linux-tutorial-portal-rm/createbutton.png)。

    ![创建摘要](media/virtual-machines-linux-tutorial-portal-rm/summarybeforecreation.png)

8. 当 Azure 创建虚拟机时，您可以跟踪集线器菜单的进度**通知**中。 Azure 创建虚拟机后，您会发现它在您 Startboard 除非您在**创建虚拟机**刀片式服务器中清除**附到 Startboard** 。

    > [AZURE.NOTE] 请注意摘要不包含公共的 DNS 名称时创建一个 VM 内部云服务使用服务管理计算堆栈的方式。

## 连接到使用**ssh** Azure Linux VM

现在您可以连接到 Ubuntu VM 使用**ssh**的标准方式。 但是，您将需要发现通过打开 VM 和其资源拼贴到 Azure VM 分配的 IP 地址。 或者你可以通过单击**浏览**，然后选择**最近**和查找的 VM 创建，或单击该图块上的 Startboard 为您创建。 在任一情况下，找到并复制**公用 IP 地址**值，如下图所示。

![成功创建的摘要](media/virtual-machines-linux-tutorial-portal-rm/successresultwithip.png)

现在您可以将**ssh**到 Azure VM，然后您就可以转。

    ssh ops@23.96.106.1 -p 22
    The authenticity of host '23.96.106.1 (23.96.106.1)' can't be established.
    ECDSA key fingerprint is bc:ee:50:2b:ca:67:b0:1a:24:2f:8a:cb:42:00:42:55.
    Are you sure you want to continue connecting (yes/no)? yes
    Warning: Permanently added '23.96.106.1' (ECDSA) to the list of known hosts.
    Welcome to Ubuntu 14.04.2 LTS (GNU/Linux 3.16.0-43-generic x86_64)

     * Documentation:  https://help.ubuntu.com/

      System information as of Mon Jul 13 21:36:28 UTC 2015

      System load: 0.31              Memory usage: 5%   Processes:       208
      Usage of /:  42.1% of 1.94GB   Swap usage:   0%   Users logged in: 0

      Graph this data and manage this system at:
        https://landscape.canonical.com/

      Get cloud support with Ubuntu Advantage Cloud Guest:
        http://www.ubuntu.com/business/services/cloud

    0 packages can be updated.
    0 updates are security updates.

    The programs included with the Ubuntu system are free software;
    the exact distribution terms for each program are described in the
    individual files in /usr/share/doc/*/copyright.

    Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
    applicable law.

    ops@ubuntuvm:~$


> [AZURE.NOTE] 您还可以在门户配置虚拟机的完全合格的域名称 (FQDN)。 阅读更多有关 FQDN[此处](virtual-machines-create-fqdn-on-portal.md)。

## 下一步行动

若要了解有关 Linux 在 Azure 上的详细信息，请参阅︰

- [Linux 和开源在 Azure 上计算](virtual-machines-linux-opensource.md)

- [如何使用 Mac 和 Linux 的 Azure 命令行工具](virtual-machines-command-line-tools.md)

- [将用于 Linux 使用 Azure CustomScript 扩展一个灯应用程序部署](virtual-machines-linux-script-lamp.md)

- [在 Azure 上 Linux Docker 虚拟机扩展](virtual-machines-docker-vm-extension.md)
