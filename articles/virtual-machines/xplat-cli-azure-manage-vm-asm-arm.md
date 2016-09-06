---
ms.openlocfilehash: f20ce177dec07f21808e53689bb961d0075e5bd3
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="虚拟机任务的等效 Azure CLI 命令 |Microsoft Azure"
    description="等效的 Azure CLI 命令可以创建和管理在 Azure 资源管理器和 Azure 服务管理模式下的 Azure Vm"
    services="virtual-machines"
    documentationCenter=""
    authors="dlepow"
    manager="timlt"
    editor=""
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="virtual-machines"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="command-line-interface"
    ms.workload="infrastructure-services"
    ms.date="08/28/2015"
    ms.author="danlep"/>


# 等效资源管理器和服务管理任务的命令 VM 使用 Mac、 Linux、 和 Windows Azure CLI
本文介绍了等效的 Microsoft Azure 命令行界面 (Azure CLI) 命令，可以创建和管理 Azure Azure 服务管理和 Azure 资源管理器中的虚拟机。 使用这本手册为脚本迁移到另一个命令模式。

* 如果已经没有 Azure CLI 安装并连接到您的订阅，请参阅[安装 Azure CLI](../xplat-cli-install.md)和[连接到 Azure CLI 从 Azure 订阅](../xplat-cli-connect.md)。 当您想要使用资源管理器模式命令时，请务必使用 login 方法进行连接。

* 若要开始使用 Azure CLI 中的资源管理器模式和命令模式切换，请参阅[使用 Azure 命令行界面使用资源管理器](xplat-cli-azure-resource-manager.md)。 默认情况下 CLI 启动服务管理模式。 若要更改为资源管理器模式，运行`azure config mode arm`。 若要返回到服务管理模式，运行`azure config mode asm`。

* 有关命令的联机帮助和选项中，键入`azure <command> <subcommand> --help`或`azure help <command> <subcommand>`。

## VM 的任务
下表比较了使用服务管理和资源管理器中的 Azure CLI 命令可以执行的常见虚拟机任务。 使用多个资源管理器命令需要通过现有资源组的名称。

> [AZURE.NOTE] 这些示例不包括基于模板的操作在资源管理器中。 有关信息，请参阅[使用 Azure 命令行界面使用资源管理器中](xplat-cli-azure-resource-manager.md)。

任务 | 服务管理 | 资源管理器
-------------- | ----------- | -------------------------
创建最基本的虚拟机 | `azure vm create [options] <dns-name> <image> [userName] [password]` | `azure vm quick-create [options] <resource-group> <name> <location> <os-type> <image-urn> <admin-username> <admin-password>`<br/><br/>(获得`image-urn`从`azure vm image list`命令。)
创建一个 Linux 虚拟机 | `azure vm create [options] <dns-name> <image> [userName] [password]` | `azure  vm create [options] <resource-group> <name> <location> -y "Linux"`
创建 Windows 虚拟机 | `azure vm create [options] <dns-name> <image> [userName] [password]` | `azure  vm create [options] <resource-group> <name> <location> -y "Windows"`
列表中的虚拟机 | `azure  vm list [options]` | `azure  vm list [options] <resource_group>`
获取虚拟机的信息 | `azure  vm show [options] <vm_name>` | `azure  vm show [options] <resource_group> <name>`
启动虚拟机 | `azure vm start [options] <name>` | `azure vm start [options] <resource_group> <name>`
停止虚拟机 | `azure vm shutdown [options] <name>` | `azure vm stop [options] <resource_group> <name>`
取消分配虚拟机 | 不可用 | `azure vm deallocate [options] <resource-group> <name>`
重新启动虚拟机 | `azure vm restart [options] <vname>` | `azure vm restart [options] <resource_group> <name>`
删除虚拟机 | `azure vm delete [options] <name>` | `azure vm delete [options] <resource_group> <name>`
获取虚拟机 | `azure vm capture [options] <name>` | `azure vm capture [options] <resource_group> <name>`
从用户映像创建虚拟机 | `azure  vm create [options] <dns-name> <image> [userName] [password]` | `azure  vm create [options] –q <image-name> <resource-group> <name> <location> <os-type>`
从一个专用的磁盘创建一个虚拟机 | `azure  vm create [options]-d <custom-data-file> <dns-name> [userName] [password]` | `azue  vm create [options] –d <os-disk-vhd> <resource-group> <name> <location> <os-type>`
将数据磁盘添加到虚拟机 | `azure  vm disk attach [options] <vm-name> <disk-image-name>` -或- <br/>  `vm disk attach-new [options] <vm-name> <size-in-gb> [blob-url]` | `azure  vm disk attach-new [options] <resource-group> <vm-name> <size-in-gb> [vhd-name]`
从虚拟机中删除数据磁盘 | `azure  vm disk detach [options] <vm-name> <lun>` | `azure  vm disk detach [options] <resource-group> <vm-name> <lun>`
将泛型扩展添加到虚拟机 | `azure  vm extension set [options] <vm-name> <extension-name> <publisher-name> <version>` | `azure  vm extension set [options] <resource-group> <vm-name> <name> <publisher-name> <version>`
将虚拟媒体访问扩展添加到虚拟机 | 不可用 | `azure vm reset-access [options] <resource-group> <name>`
将 Docker 扩展添加到虚拟机 | `azure  vm docker create [options] <dns-name> <image> <user-name> [password]` | `azure  vm docker create [options] <resource-group> <name> <location> <os-type>`
将厨师的简历扩展添加到虚拟机 | `azure  vm extension get-chef [options] <vm-name>` | 不可用
禁用虚拟机扩展 | `azure  vm extension set [options] –b <vm-name> <extension-name> <publisher-name> <version>` | 不可用
删除虚拟机扩展 | `azure  vm extension set [options] –u <vm-name> <extension-name> <publisher-name> <version>` | `azure  vm extension set [options] –u <resource-group> <vm-name> <name> <publisher-name> <version>`
VM 扩展列表 | `azure vm extension list [options]` | `azure  vm extension get [options] <resource-group> <vm-name>`
VM 映像列表 | `azure vm image list [options]` | `azure vm image list [options] <location> <publisher> [offer] [sku]` -或- <br/> `azure vm image list-publishers [options] <location>` -或- <br/> `azure vm image list-offers [options] <location>` -或- <br/> `azure vm image list-skus [options] <location>`
显示虚拟机映像 | `azure vm image show [options]` | 不可用
获取虚拟机资源的使用情况 | 不可用 | `azure vm list-usage [options] <location>`
获取所有可用的虚拟机大小 | 不可用 | `azure vm sizes [options]`


## 下一步行动

* 有关使用 Azure CLI 使用资源管理器资源的详细信息，请参阅[使用 Azure 命令行界面使用资源管理器](xplat-cli-azure-resource-manager.md)和[Managing Role-Based 访问控制与 Azure 命令行界面](../role-based-access-control-xplat-cli.md)。
* 有关 CLI 命令的其它示例，请参阅[使用 Azure 命令行界面使用 Azure 服务管理](../virtual-machines-command-line-tools.md)和[使用 Azure CLI 使用 Azure 资源管理器中](azure-cli-arm-commands.md)。
