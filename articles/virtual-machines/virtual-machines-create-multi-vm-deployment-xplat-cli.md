---
ms.openlocfilehash: f294566dbb1f76ac9f7f29490fe7dca0047d7ebb
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="创建多个 VM 部署使用 Azure CLI |Microsoft Azure"
   description="了解如何创建多个虚拟机部署使用 Azure CLI"
   services="virtual-machines"
   documentationCenter="nodejs"
   authors="AlanSt"
   manager="timlt"
   editor=""/>

   <tags
   ms.service="virtual-machines"
   ms.devlang="nodejs"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="02/20/2015"
   ms.author="alanst;kasing"/>

# 创建多个 VM 部署使用 Azure CLI

下面的脚本将显示如何配置多个虚拟机多云服务部署在 VNET 中使用 Azure 命令行界面 (Azure CLI)。

下图说明了部署脚本完成后的外观︰

![](./media/virtual-machines-create-multi-vm-deployment-xplat-cli/multi-vm-xplat-cli.png)

该脚本在云服务**workercs**创建一个虚拟机 (**servervm**) 在两个附加的数据磁盘的云服务**servercs**和两个虚拟机 （**clientvm1、 clientvm2**）。 云服务都放在 VNET **samplevnet**。 **Servercs**云服务也有配置外部连接的终结点。

## CLI 脚本以使它发生
要对此进行设置的代码是非常简单直接︰

>[AZURE.NOTE] 您可能需要更改云服务名称 servercs 和 workercs 是唯一的云服务名称

    azure network vnet create samplevnet -l "West US"
    azure vm create -l "West US" -w samplevnet -e 10000 -z Small -n servervm servercs b39f27a8b8c64d52b05eac6a62ebad85__Ubuntu-14_10-amd64-server-20150202-en-us-30GB azureuser Password@1
    azure vm create -l "West US" -w samplevnet -e 10001 -z Small –n clientvm1 clientcs b39f27a8b8c64d52b05eac6a62ebad85__Ubuntu-14_10-amd64-server-20150202-en-us-30GB azureuser Password@1
    azure vm create -l "West US" -w samplevnet -e 10002 -c -z Small -n clientvm2 clientcs b39f27a8b8c64d52b05eac6a62ebad85__Ubuntu-14_10-amd64-server-20150202-en-us-30GB azureuser Password@1
    azure vm disk attach-new servervm 100
    azure vm disk attach-new servervm 500
    azure vm endpoint create servervm 443 443 -n https -o tcp

因为是销毁它的代码︰

    azure vm delete -b -q servervm
    azure vm delete -b -q clientvm1
    azure vm delete -b -q clientvm2
    azure network vnet delete -q samplevnet

*-Q 选项取消删除对象的交互式确认、 b 清理磁盘 / blob 与 VM。*

## 使用的命令的一般形式

虽然您可以使用来查找详细信息 – 帮助选项在任何 Azure CLI 命令，使用上面的每个命令的一般形式是︰

    azure network vnet create -l <Region> <VNet_name>
    azure network vnet delete -q <VNet_name>

    azure vm create -l <Region> -w <Vnet_name> -e <SSH_port> -z <VM_size> -n <VM_name> <Cloud_service_name> <VM_image> <Username> <Password>
    azure vm delete -b -q <VM_name>
    azure vm disk attach-new <VM_name>
    azure vm endpoint create <VM_name> <External_port> <Internal_port> -n <Endpoint_name> -o <TCP/UDP>

## 下一步行动


* [Linux 和开源在 Azure 上计算](virtual-machines-linux-opensource.md)
* [如何登录到运行 Linux 的虚拟机](virtual-machines-linux-how-to-log-on.md)
 
