---
ms.openlocfilehash: db06239a017dea4e1a20a9d8f0072c5064434ab6
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="捕获运行 Linux 虚拟机映像"
    description="了解如何捕获运行 Linux Azure 虚拟机 (VM) 映像。"
    services="virtual-machines"
    documentationCenter=""
    authors="dsk-2015"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/16/2015"
    ms.author="dkshir"/>


# 如何捕获 Linux 虚拟机作为模板使用

本文演示如何捕获 Azure 运行 Linux，因此可用它像一个模板来创建其他虚拟机的虚拟机。 此模板包括操作系统磁盘和数据磁盘连接到虚拟机。 不包括网络配置，因此您将需要配置它，当您创建其他使用该模板的虚拟机。

Azure 将此模板作为图像处理，并将其存储在**图像**下。 这是您已上载的任何图像的存储位置。 有关映像的详细信息，请参阅[关于虚拟机映像在 Azure 中] []。

## 开始之前

这些步骤假定已经创建 Azure 使用传统部署模型的虚拟机并配置操作系统，包括附加任何数据磁盘。 如果尚未尚未执行此操作，请参阅以下说明︰

- [如何创建运行 Linux 的虚拟机] []


## 获取虚拟机

1. 连接到虚拟机使用您选择的 SSH 客户端。 有关详细信息，请参阅[如何登录到虚拟机运行 Linux] []。

2. 在 SSH 窗口中，键入下面的命令。  请注意，输出`waagent`根据此实用程序的版本可能略有不同︰

    `sudo waagent -deprovision`

    此命令将尝试清理系统并使其适用于重新调配。 此操作将执行以下任务︰

    - 删除主机的 SSH 密钥 （如果 Provisioning.RegenerateSshHostKeyPair 是在配置文件中的 y）
    - 清除在 /etc/resolv.conf 中的名称服务器配置
    - 删除`root`/等/影子的设定 （如果 Provisioning.DeleteRootPassword 是在配置文件中的 y） 从用户的密码
    - 删除缓存的 DHCP 客户端租约
    - 重置为 localhost.localdomain 的主机名
    - 删除最后一个已设置的用户帐户 （来自 /var/lib/waagent）**和相关联的数据**。

    >[AZURE.NOTE] 为了"一般化"映像中撤消删除的文件和数据。 只有在您想要捕获新映像模板的虚拟机上运行此命令。 它不保证图像清除所有敏感信息，或适用于重新分发给第三方。


3. 输入**y**继续。 您可以添加`-force`参数，以避免此确认步骤。

4. 键入**Exit**关闭 SSH 客户端。


    >[AZURE.NOTE] 下面的步骤假设您已经[安装](../xplat-cli-install.md)了 Azure CLI 在客户端计算机上。 [管理门户] []中也可以完成下面的所有步骤。

5. 从客户端计算机，打开 Azure CLI 和 Azure 订阅的登录。 有关详细信息，请阅读[连接到 Azure CLI 从 Azure 订阅](../xplat-cli-connect.md)。

6. 请确保您处于服务管理模式︰

    `azure config mode asm`

7. 关闭虚拟机已 deprovisioned 中使用上述步骤︰

    `azure vm shutdown <your-virtual-machine-name>`

    >[AZURE.NOTE] 您可以找出使用创建您的订阅中的所有虚拟机 `azure vm list`

8. 停止虚拟机后，捕获图像的命令︰

    `azure vm capture -t <your-virtual-machine-name> <new-image-name>`

    键入_新映像名称_替换所需的图像名称。 此命令创建通用的操作系统映像。 `-t`子命令将删除原始虚拟机。

9.  新的图像列表中的图像，可用于配置任何新的虚拟机现。 您可以使用命令来查看它︰

    `azure vm image list`

    在[管理门户网站] []，它将出现在**图像**列表中。

    ![图像捕获成功](./media/virtual-machines-linux-capture-image/VMCapturedImageAvailable.png)


## 下一步行动
该图像就可以用作模板创建虚拟机。 您可以使用 Azure CLI 命令`azure vm create`，并提供您刚刚创建的映像名称。 有关命令的详细信息，请参阅[服务管理 api 使用 Azure CLI](virtual-machines-command-line-tools.md) 。 或者，可以使用[管理门户] []通过使用**剪辑库中的**方法，选择刚才创建的映像创建一个自定义的虚拟机。 有关详细信息，请参阅[如何创建自定义虚拟机] []。

**另请参见︰**[Azure Linux 代理用户指南](virtual-machines-linux-agent-user-guide.md)

[管理门户]: http://manage.windowsazure.com
[如何登录到运行 Linux 的虚拟机]: virtual-machines-linux-how-to-log-on.md
[有关在 Azure 中的虚拟机映像]: http://msdn.microsoft.com/library/azure/dn790290.aspx
[如何创建自定义的虚拟机]: virtual-machines-create-custom.md
[如何附加到虚拟机的数据磁盘]: storage-windows-attach-disk.md
[如何创建运行 Linux 的虚拟机]: virtual-machines-linux-tutorial.md
