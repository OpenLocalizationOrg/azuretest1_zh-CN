---
ms.openlocfilehash: 1470b72b30c4ff25da9effb0e6966eaf417b5d44
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="解决安全外壳协议 (SSH) 连接到一个基于 Linux 的 Azure 的虚拟机"
    description="如果您无法连接您的基于 Linux 的 Azure 的虚拟机，请使用下列步骤找出问题根源。"
    services="virtual-machines"
    documentationCenter=""
    authors="dsk-2015"
    manager="timlt"
    editor=""
    tags="azure-service-management,azure-resource-manager"/>


<tags
    ms.service="virtual-machines"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/07/2015"
    ms.author="dkshir"/>

# 解决安全外壳协议 (SSH) 连接到一个基于 Linux 的 Azure 的虚拟机

如果您无法连接到基于 Linux 的 Azure 的虚拟机，本文介绍了对于隔离和解决问题的方法。

## 步骤 1︰ 重置 SSH 配置、 密钥或密码

按照[如何重置密码或 SSH 的基于 Linux 的虚拟机](virtual-machines-linux-use-vmaccess-reset-password-or-ssh.md)中的说明进行操作在虚拟机上。 使用这些指令，您可以︰

- 重置密码的 SSH 密钥。
- 创建一个新的 sudo 用户帐户。
- SSH 配置重置。

如果 SSH 客户端仍能达到 SSH 服务，在虚拟机上的，可以由很多原因导致。 下面是所涉及的组件集。

![](./media/virtual-machines-troubleshoot-ssh-connections/ssh-tshoot1.png)

以下各节逐步进行隔离，并确定此问题的各种原因和提供解决方案和替代方法。

## 步骤 2︰ 前详细地故障排除的基本步骤

首先，检查 Azure 管理门户或 Azure 预览门户中的虚拟机的状态。

在 Azure 管理门户︰

1. 单击**虚拟机** > *虚拟机名称*。
2. 单击**仪表板**为该虚拟机可以检查其状态。
3. 单击**监视器**，以查看最近的计算、 存储和网络资源的活动。
4. 单击以确保 SSH 的通信的终结点的**终结点**。

在 Azure 预览门户︰

1. 单击**浏览** > **虚拟机** > *虚拟机名称*。 为虚拟机创建 Azure 资源管理器中，单击**浏览** > **虚拟机 (v2)** > *虚拟机名称*。 虚拟机的状态窗格将显示**运行**。 向下滚动到显示最近的活动的计算、 存储和网络资源。
2. 单击**设置**以检查终结点、 IP 地址和其他设置。

若要验证网络连接，请分析配置的终结点，并确定是否可以访问虚拟机通过其他协议，例如，HTTP 或另一个已知的服务。

如果需要请[重新启动该虚拟机](https://msdn.microsoft.com/library/azure/dn763934.aspx)或[调整其大小的虚拟机](https://msdn.microsoft.com/library/dn168976.aspx)。

在这些步骤中之后, 尝试的 SSH 连接。

## 第 3 步︰ 详细的疑难解答

SSH 客户端无法访问 Azure 的虚拟机上的 SSH 服务可能是由于下列问题或配置错误的来源︰

- SSH 客户端计算机
- 组织边缘设备
- 云服务终结点和访问控制列表 (ACL)
- 网络安全组
- 基于 linux * 的 Azure 的虚拟机

### 源 1: SSH 客户端计算机

要消除问题或配置错误的源计算机，请验证您的计算机可以使 SSH 连接到另一个内部，基于 Linux 的计算机。

![](./media/virtual-machines-troubleshoot-ssh-connections/ssh-tshoot2.png)

如果不能检查这些在您的计算机上︰

- 本地的防火墙设置，将阻止入站或出站 SSH 通信 (TCP 22)
- 本地安装使 SSH 连接的客户端代理服务器软件
- 本地安装监控软件，阻止 SSH 连接的网络
- 其他类型的安全软件监视的通信量，或者允许/禁止特定类型的通信，阻止 SSH 连接

在所有这些情况下，请暂时禁用该软件并尝试 SSH 连接到本地计算机以确定原因。 然后，使用您的网络管理员联系以纠正软件的设置，以允许 SSH 连接。

如果您使用的证书身份验证，则验证.ssh 文件夹到您的主目录中具有这些权限︰

- Chmod 700 ~/.ssh
- Chmod 644 ~/.ssh/*.pub
- Chmod 600 ~/.ssh/id_rsa （或任何其他文件可能具有私钥存储在）
- Chmod 644 ~/.ssh/known_hosts （包含已通过 SSH 连接到主机）

### 源 2︰ 组织边缘设备

为消除组织边缘问题或配置错误的源设备，验证直接连接到 Internet 的计算机可以使 SSH 连接到 Azure 的虚拟机。 如果您要通过一个站点到站点 VPN 或 ExpressRoute 连接访问虚拟机，请跳到[源 4︰ 网络安全组](#nsg)。

![](./media/virtual-machines-troubleshoot-ssh-connections/ssh-tshoot3.png)

如果您没有直接连接到 Internet 的计算机，您可以轻松地在其自己的资源组中创建一个新的 Azure 虚拟机或云服务并使用它。 有关详细信息，请参阅[创建虚拟机运行在 Azure 中的 Linux](virtual-machines-linux-tutorial.md)。 与您的测试完成后删除的资源组或虚拟机和云服务。

如果可以 SSH 连接直接连接到 Internet 的计算机，请检查为您组织边缘设备︰

- 内部防火墙 SSH 通信与互联网阻塞
- 您使 SSH 连接的代理服务器
- 入侵检测或网络监视使 SSH 连接边缘网络中的设备上运行的软件

与网络管理员联系以纠正组织边缘设备的设置，以允许 SSH 通信与互联网合作。

### 来源 3︰ 云服务终结点和 ACL

若要消除的云服务终结点和 ACL 问题或使用服务管理 API 创建的虚拟机的配置错误的源，验证相同的虚拟网络中的其他 Azure 的虚拟机可以使 SSH 连接到 Azure 的虚拟机。

![](./media/virtual-machines-troubleshoot-ssh-connections/ssh-tshoot4.png)

> [AZURE.NOTE] Azure 资源管理器中创建的虚拟机，请跳到[源 4︰ 网络安全组](#nsg)。

如果您没有相同的虚拟网络中的其他虚拟机，您可以轻松地创建一个新。 有关详细信息，请参阅[创建虚拟机运行在 Azure 中的 Linux](virtual-machines-linux-tutorial.md)。 与您的测试完成后删除额外的虚拟机。

如果您可以使用相同的虚拟网络中的虚拟机创建 SSH 连接，请检查︰

- 目标虚拟机上的 SSH 通信的终结点配置。 终结点的专用 TCP 端口必须匹配该虚拟机上的 SSH 服务正在侦听的 TCP 端口默认为 22。 Azure 资源管理器中，使用模板创建的虚拟机验证在 Azure 预览门户与**浏览**的 SSH TCP 端口号为 > **虚拟机 (v2)** > *虚拟机名称* > **设置** > **的终结点**。
- SSH 通信终结点目标虚拟机上的 ACL。 Acl，可以指定允许或拒绝从互联网，基于其源 IP 地址的传入通信。 错误配置的 Acl 可以防止 SSH 的传入通信的终结点。 检查您的 Acl 来确保从您的代理服务器的公用 IP 地址的传入通信，或允许其他边缘服务器。 有关详细信息，请参阅[关于网络访问控制列表 (Acl)](../virtual-network/virtual-networks-acl.md)。

若要消除该终结点作为源的问题，删除的当前终结点和创建新的终结点，指定**SSH**名称 （TCP 端口 22 的公钥和私钥的端口号）。 有关详细信息，请参阅[设置 Azure 中的虚拟机上的终结点](virtual-machines-set-up-endpoints.md)。

### <a id="nsg"></a>源 4︰ 网络安全组

网络安全组让您可以更细致地控制允许入站和出站通信。 您可以创建规则，跨越子网和云在 Azure 的虚拟网络服务。 检查以确保允许 SSH 通信与互联网网络安全组规则。
有关详细信息，请参阅[关于网络安全组](../traffic-manager/virtual-networks-nsg.md)。

### 源 5︰ 基于 Linux 的 Azure 虚拟机

最后可能出现的问题根源是 Azure 虚拟机本身。

![](./media/virtual-machines-troubleshoot-ssh-connections/ssh-tshoot5.png)

如果您有不这样做，在虚拟机上按照[如何重置密码或 SSH 的基于 Linux 的虚拟机](virtual-machines-linux-use-vmaccess-reset-password-or-ssh.md)中的说明进行操作。

请尝试重新连接从您的计算机。 如果您不是成功的下面是一些可能出现的问题︰

- SSH 服务没有在目标虚拟机上运行。
- SSH 服务未侦听 TCP 端口 22。 要测试这一点，在您的本地计算机上安装的 telnet 客户端并运行"远程登录*cloudServiceName*。 cloudapp.net 22"。 这将确定虚拟机是否允许入站和出站通信到 SSH 的终结点。
- 目标虚拟机上的本地防火墙已阻止入站或出站 SSH 通信的规则。
- 入侵检测或网络监视 Azure 的虚拟机上运行的软件使 SSH 连接。


## 第 4 步︰ 提交您的问题到 Azure 支持论坛

来自世界各地的 Azure 专家获得帮助，您可以提交您到 MSDN Azure 或堆栈溢出论坛的问题。 有关详细信息，请参阅[Microsoft Azure 论坛](http://azure.microsoft.com/support/forums/)。

## 步骤 5︰ 文件 Azure 支持事件

如果您已完成步骤 1 到 4 这篇文章中并提交您的问题为 Azure 支持论坛，但仍无法创建 SSH 连接，需要考虑的一种选择是是否可以重新创建该虚拟机。

如果无法重新创建虚拟机，则可能是文件 Azure 支持事件的时间。

以文件事件、 转到[Azure 支持站点](http://azure.microsoft.com/support/options/)，然后单击**获得支持**。

有关使用 Azure 支持信息，请参阅[Microsoft Azure 支持常见问题解答](http://azure.microsoft.com/support/faq/)。

## 其他资源

[如何重置密码或 SSH 的基于 Linux 的虚拟机](virtual-machines-linux-use-vmaccess-reset-password-or-ssh.md)

[疑难解答 Windows 远程桌面连接到基于 Windows Azure 的虚拟机](virtual-machines-troubleshoot-remote-desktop-connections.md)

[解决对 Azure 的虚拟机上运行的应用程序的访问](virtual-machines-troubleshoot-access-application.md)
