---
ms.openlocfilehash: 0ec28835a088b6fe0dd60101eaa2fd0b2ce9d135
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="虚拟机和角色实例解析"
   description="Azure IaaS，混合解决方案，不同的云服务、 活动目录和使用 DNS 服务器之间的名称解析方案 "
   services="virtual-network"
   documentationCenter="na"
   authors="joaoma"
   manager="jdial"
   editor="tysonn" />
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/10/2015"
   ms.author="joaoma" />

# 虚拟机和角色实例名称解析

这取决于如何使用 Azure 主机 IaaS、 PaaS 和混合解决方案，您可能需要允许虚拟机和您创建的用来相互通信的角色实例。 虽然这种通信可以通过使用 IP 地址，则更易于使用，可以很容易地记住，而不更改名称。 

当角色实例和 Azure 中承载的虚拟机需要域名解析为内部 IP 地址时，他们可以使用两种方法之一︰

- [Azure 提供名称解析](#azure-provided-name-resolution)

- [使用您自己的 DNS 服务器的名称解析](#name-resolution-using-your-own-dns-server) 

您使用的名称解析的类型取决于如何您的虚拟机和角色实例需要相互通信。

**下表说明了方案和相应的名称解析解决方案︰**

| **方案** | **通过提供名称解析︰** | **有关详细信息，请参阅︰** |
|--------------|----------------------------------|-------------------------------|
| 角色实例或位于同一个云服务的虚拟机之间的名称解析 | Azure 提供名称解析 | [Azure 提供名称解析](#azure-provided-name-resolution)|
| 虚拟机之间的角色实例位于相同的虚拟网络名称解析 | Azure 提供名称解析 （仅基于 ARM 的部署） 或使用您自己的 DNS 服务器的名称解析 | [提供 azure 的名称解析](#azure-provided-name-resolution)，[使用 DNS 服务器的名称解析](#name-resolution-using-your-own-dns-server)和[DNS 服务器要求](#dns-server-requirements) |
| 虚拟机之间的角色实例位于不同的虚拟网络名称解析 | 使用您自己的 DNS 服务器的名称解析| [使用您自己的 DNS 服务器的名称解析](#name-resolution-using-your-own-dns-server)和[DNS 服务器的要求](#dns-server-requirements)|
| 在 Azure 和内部计算机跨场所︰ 名称解析角色实例或虚拟机之间| 使用您自己的 DNS 服务器的名称解析| [使用您自己的 DNS 服务器的名称解析](#name-resolution-using-your-own-dns-server)和[DNS 服务器的要求](#dns-server-requirements)|
| 反向搜索的内部 Ip| 使用您自己的 DNS 服务器的名称解析| [使用您自己的 DNS 服务器的名称解析](#name-resolution-using-your-own-dns-server)和[DNS 服务器的要求](#dns-server-requirements)|
| 名称解析为非公共域 （如 Active Directory 域）| 使用您自己的 DNS 服务器的名称解析| [使用您自己的 DNS 服务器的名称解析](#name-resolution-using-your-own-dns-server)和[DNS 服务器的要求](#dns-server-requirements)|
| 位于不同的云服务，不是在虚拟网络中的角色实例之间的名称解析| 不适用。 虚拟机和其他云服务中的角色实例之间的连接不支持外部虚拟网络。| 不适用。|


## Azure 提供名称解析

公共 DNS 名称的解析，以及 Azure 的虚拟机和角色实例驻留在同一个虚拟网络或云服务提供内部名称解析。  在云服务中的虚拟机/实例共享相同的 DNS 后缀，以便单独主机名称就足够了。  在经典的虚拟网络中，不同的云服务因此 FQDN 需要有不同的 DNS 后缀。  在基于 ARM 的虚拟网络中，DNS 后缀是常见虚拟网络因此 FQDN 不必需的和 DNS 名称可以分配给 NIC 或虚拟机。 尽管提供 Azure 的名称解析不需要任何配置，但不适合所有的部署方案上表, 所示。

> [AZURE.NOTE] 在 web 和辅助的角色，也可以访问角色实例基于角色的名称和实例编号，使用 Azure 服务管理 REST API 的内部 IP 地址。 有关详细信息，请参阅[服务管理 REST API 参考](https://msdn.microsoft.com/library/azure/ee460799.aspx)。

### 功能和注意事项

**功能︰**

- 易用性︰ 无需配置需要使用 Azure 提供名称解析。

- 名称解析提供角色实例之间或 fqdn 需要不在同一个云服务中的虚拟机。

- 名称解析是前提之间无需 FQDN 基于 ARM 的虚拟网络中的虚拟机，经典的网络 FQDN 时需要不同的云服务中的名称解析。 

- 您可以创建主机名来最好地描述您的部署，而不是使用自动生成的名称。

**注意事项︰**

- 虚拟的网络之间的名称解析将不可用。

- 您只能注册虚拟机和角色实例驻留在前 180 云服务添加到 Azure 的虚拟网络中的主机名。 如果有 180 多个云服务，独立数的虚拟机和角色实例中每个服务，您需要提供您自己的 DNS 服务器进行名称解析。

- 跨内部名称解析将不可用。

- Azure 创建 DNS 后缀不能修改。

- 您不能手动注册您自己的记录。

- 不支持 WINS 和 NetBIOS。 （您不能列出您在 Windows 资源管理器中的网络浏览器中的虚拟机）。

- 主机名必须是 DNS 兼容 (他们必须使用仅 0-9、 a 到 z 和-，并不能启动或结束与-。 请参阅 RFC 3696 第 2 部分。

- 每个 VM，DNS 查询通信量被遏制。 如果您的应用程序会对多个目标名称执行频繁的 DNS 查询，则可能有些查询可能会超时。 若要避免该问题，建议以启用客户端缓存。  这默认启用的 Windows 上，但一些 Linux distros 可能不具有已启用缓存。

## 使用您自己的 DNS 服务器的名称解析

如果您的名称解析需求超出了 Azure 所提供的功能，您可以使用您自己的 DNS 服务器。 当您使用您自己的 DNS 服务器时，您负责管理的 DNS 记录。

> [AZURE.NOTE] 建议避免使用的外部 DNS 服务器，除非您的部署方案需要它。

## DNS 服务器的要求

如果您计划使用 Azure 未提供的名称解析，您指定的 DNS 服务器必须支持︰

- DNS 服务器应接受动态 DNS 注册，通过动态 DNS (DDNS) 协议，也必须创建所需的记录。

- 如果依赖于动态 DNS，DNS 服务器应具有记录清理关闭。 在 Azure，IP 地址必须长 DHCP 租约，这可能会导致在清理从 DNS 服务器记录的删除。

- DNS 服务器必须能够允许的外部域名解析的递归。

- （TCP/UDP 端口 53 上传入），DNS 服务器必须能够访问通过名称解析请求的客户端和服务并将其名称注册的虚拟机。

- 它还建议从 internet 时保护许多机器人扫描打开递归 DNS 解析程序阻止访问的 DNS 服务器。

- 为了获得最佳性能，当使用 Azure 虚拟机用作 DNS 服务器，应禁用 IPv6 和[实例级公用 IP](virtual-networks-instance-level-public-ip.mp)应分配给虚拟机的每个 DNS 服务器。

## 指定 DNS 服务器

您可以指定多个 DNS 服务器，将由您的虚拟机和角色实例。  对于每个 DNS 查询，客户端将首先尝试在首选的 DNS 服务器并只尝试备用服务器，如果首选的一没有反应，即 DNS 查询不是负载平衡跨不同的 DNS 服务器。 出于此原因，请验证您有为您的环境的正确顺序列出 DNS 服务器。

> [AZURE.NOTE] 如果您更改网络配置文件，为已部署的虚拟网络上的 DNS 设置，您需要重新启动以使更改生效每个 VM。

### 管理门户中指定的 DNS 服务器

管理门户中创建虚拟网络时，您可以指定 IP 地址和要使用的 DNS 服务器的名称。 一旦创建了虚拟的网络，虚拟机和虚拟网络部署的角色实例自动配置与指定的 DNS 设置。  为特定的云服务 (经典 Azure) 指定 DNS 服务器或网络接口卡 （基于 ARM 的部署） 优先于指定的虚拟网络。  请参阅[关于配置管理门户中的虚拟网络](virtual-networks-settings.md)。

### 通过使用配置文件 （Azure 经典） 中指定的 DNS 服务器

对于经典的虚拟网络，可以通过使用两个不同的配置文件指定 DNS 设置︰*网络配置*文件，*服务配置*文件。

> [AZURE.NOTE] DNS 服务器服务配置文件中的重写该网络的配置文件中的设置。 
 
网络配置文件描述了您的 subscripion 中的虚拟网络。 向云服务，在虚拟的网络中添加角色实例或虚拟机，网络配置文件中的 DNS 设置应用于每个角色实例或虚拟机除非已指定了云服务特定的 DNS 服务器。

为每个添加到 Azure 的云服务创建服务配置文件。 当向云服务中添加角色实例或虚拟机时，您的服务配置文件中的 DNS 设置应用于每个角色实例或虚拟机。



## 下一步行动

[Azure 服务配置架构](https://msdn.microsoft.com/library/azure/ee758710)

[虚拟网络配置架构](https://msdn.microsoft.com/library/azure/jj157100)

[有关配置管理门户中的虚拟网络设置](virtual-networks-settings.md) 

[通过网络配置文件来配置虚拟网络](virtual-networks-using-network-configuration-file.md) 


