---
ms.openlocfilehash: 73a62ddab769731e80b134d5de8e59be137e1b9a
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="管理 Azure 虚拟机使用的 Mac、 Linux、 Windows Azure CLI |Microsoft Azure"
   description="描述如何创建、 管理和删除使用 Mac、 Linux、 Windows Azure CLI 将 Azure Vm。"
   services="virtual-machines"
   documentationCenter="virtual-machines"
   authors="dlepow"
   manager="timlt"
   editor=""/>

   <tags
   ms.service="virtual-machines"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="06/09/2015"
   ms.author="danlep"/>

# 管理您的虚拟机使用的 Mac、 Linux、 Windows Azure CLI

使用 Azure CLI 可通过自动执行每一天，来管理您的虚拟机的多任务。 本文提供了示例命令更简单的任务，以及链接到显示更复杂的任务命令的文章。

>[AZURE.NOTE] 如果您还没有安装并配置了 Azure CLI，则说明[此处](../xplat-cli-install.md)。 如果您希望快速入门中 PowerShell 的相同任务，请参阅[管理虚拟机使用 Azure PowerShell](virtual-machines-manage-vms-powershell.md)。

## 如何使用示例命令
您将需要用适合于您的环境的文本替换某些命令中的文字。 < 和 > 符号指示您需要替换的文本。 替换文本，请移除的符号，但在引号将保留在原处。

> [AZURE.NOTE] 如果您希望以编程方式存储和操作控制台命令的输出，可能要使用 JSON 分析工具如**[jq](https://github.com/stedolan/jq)**、 **[jsawk](https://github.com/micha/jsawk)**或语言库好任务的。

## 显示有关 VM 的信息

这是您将经常使用的基本任务。 使用它来获取 VM 的信息、 在 VM 上执行任务或得到的输出存储在一个变量中。

要获取有关 VM 的信息，请运行以下命令，替换在引号中，包括一切 < 和 > 字符︰

     azure vm show -g <group name> -n <virtual machine name>

若要将输出存储在 $vm 变量中作为 JSON 文档，运行︰

    vmInfo=$(azure vm show -g <group name> -n <virtual machine name> --json)

或者，您可以管道标准输出到文件。

## 登录到基于 Linux 的虚拟机

通常 Linux 机器通过 SSH 连接到中。 有关详细信息，请参阅[如何使用 SSH 使用 Linux 在 Azure 上](virtual-machines-linux-use-ssh-key.md)。
Azure 的资源管理器概述
## 停止虚拟机

运行以下命令︰

    azure vm stop <group name> <virtual machine name>

>[AZURE.IMPORTANT] 此参数用于保持云服务的虚拟 IP (VIP)，它是最后一个 VM 中的云服务的情况下。 <br><br> 如果您使用 StayProvisioned 参数，您将仍然货款用于 VM。

## 启动虚拟机

运行下面的命令︰ Azure 的资源管理器概述 azure 的虚拟机启动 <group name> <virtual machine name>

## 附加数据磁盘

您将需要决定是否要附加新磁盘，或在表单中包含的数据。 为新磁盘命令创建.vhd 文件，并将其附加在相同的命令。

若要添加新的磁盘，请运行以下命令︰

     azure vm disk attach-new <resource-group> <vm-name> <size-in-gb>

若要附加现有数据磁盘，请运行以下命令︰

    azure vm disk attach <resource-group> <vm-name> [vhd-url]

## 创建一个 Linux 虚拟机

若要创建新的基于 Linux 的 VM，您要需要现货，有几个值包括资源组的名称、 位置、 图像名称、 一个虚拟机的名称和后备.vhd 映像存储的存储帐户。 一旦您要使用的信息，Azure CLI 可以创建交互式会话，以提示您输入这些值︰

    azure vm create

当然，如果您已经有了这些值可以找到正确的开关，通过它们直接通过键入`azure help vm create`。

## 下一步行动

有关的 Azure CLI 使用**arm**模式的更多示例，请参阅[使用 Microsoft Azure CLI 的 Mac、 Linux、 Windows Azure 资源管理](../xplat-cli-resource-manager.md)。 若要了解有关 Azure 资源和其概念的详细信息，请参阅[Azure 资源管理器的概述](../resource-group-overview.md)。
 